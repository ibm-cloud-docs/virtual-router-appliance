---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 高可用性配置
{: #vyatta-5400-high-availability-configuration}

透過使用 VRRP（虛擬遞送備援通訊協定）來支援 Vyatta 高可用性。每一個閘道群組都會有兩個主要 VRRP IP 位址：一個用於專用，另一個則用於網路的公用端。 

**附註：**僅限專用 Vyatta 才會有專用 VRRP。 

這些 IP 位址是 Softlayer 網路基礎架構的目標 IP，可遞送 VLAN 上與閘道成員相關聯的所有子網路。一次只有一個 Vyatta 會執行這些 VRRP IP，其他閘道群組成員則會透過管理方式關閉它們。

使用 "config-sync" 配置指令，可以同步化兩個 Vyatta 之間的配置。此配置將容許一位成員將特定選項的配置推送至群組中的另一個 Vyatta，並選擇性地這麼做。您只能推送防火牆規則、只能推送 IPsec 配置，或推送任意規則集組合。 

建議您不要嘗試推送 IP 位址或其他網路配置，因為 config-sync 會立即確定其他 Vyatta 上的變更，讓這些介面上線。如果您要動態啟用介面及服務，則應該使用轉移 Script 執行此動作以進行失效接手。此外，建議您針對關聯 VLAN 上的閘道 IP 使用其他 VRRP IP 位址，這樣可更輕鬆地管理失效接手。

您在兩部機器上的基本 VRRP 配置與下列範例配置類似：

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP 需要一個實際 IP 位址先連結至虛擬介面，才能傳送所有 VRRP 公告。在許多情況下，您只需要從主要子網路中新增 IP 即可，但這可能會與未來的佈建發生衝突，或是您可能要遞送已將每個主要子網路 IP 配置給伺服器的 VLAN。若要解決這個問題，您可以使用兩個 Vyatta 上它們用來彼此交談的頻外 IP 位址配對。範例如下：

在第一個 vyatta 上：

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

在第二個 vyatta 上：

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

在此情況下，兩個 vyatta 都會有自己的 IP，而 IP 不會與已配置的子網路發生衝突。您幾乎可以選擇任何想要的專用範圍，只要挑選不會與您可能會有的任何其他路徑發生衝突的小型子網路（例如透過 IPsec 通道的子網路範圍，或與 Softlayer 發生衝突的 10.0.0.0/8 位址）。

您也想要新增 "sync-group" 名稱。所有 VRRP 位址都應該是相同 sync-group 的一部分。某個介面上有任何失敗，也會導致相同 sync-group 中的所有介面都進行失效接手。否則，最後會有一些成為 MASTER，其他則在 BACKUP 中。在 bond0 及 bond1 原生 VLAN 配置中，會使用相同名稱。

附註：bond0 及 bond1 VRRP 配置可能會有一行是針對 rfc3768 相容性。在 vif 上，VRRP 不需要這個項目，只有原生 VLAN bond0 及 bond1 才需要。

在新佈建的閘道配對中，config-sync 只會有最小配置：


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

您需要新增規則，以判斷要移轉的配置分支。例如，如果您要推送 firewall 及 ipsec 配置，請新增下列指令：


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

確定這些項目之後，在確定時，會將對 firewall 或 ipsec 配置進行的全部變更推送至另一個 vyatta。

**附註：**sync-group 及 sync-map 是兩個不同的事項。sync-map 配置是讓規則將配置變更推送至另一個 Vyatta。另一個 sync-group 則是讓 VRRP IP 以群組方式進行失效接手，而不是一次一個。配置其中一個並不會影響另一個。

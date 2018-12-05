---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400 高可用性構成

Vyatta 高可用性は、VRRP (Virtual Routing Redundancy Protocol) の使用を介してサポートされます。 各ゲートウェイ・グループには 2 つのプライマリー VRRP IP アドレスがあります。1 つはプライベート用、もう 1 つはネットワークのパブリック・サイド用です。 

**注:** プライベート専用 Vyatta にあるのはプライベート VRRP のみです。 

これらの IP アドレスは、Softlayer ネットワーク・インフラストラクチャーが、ゲートウェイ・メンバーと関連付けられた VLAN 上のすべてのサブネットを経路指定するためのターゲット IP です。 これらの VRRP IP は、1 つの Vyatta でのみ同時に実行状態になり、ゲートウェイ・グループの他のメンバーでは管理上ダウン状態になります。

「config-sync」構成コマンドを使用して、2 つの Vyatta の間で構成を同期化できます。 この構成により、あるメンバーがグループ内の他方の Vyatta に特定のオプションの構成をプッシュできるようになります。このプッシュは選択的に行うことができます。 ファイアウォール・ルールのみ、IPsec 構成のみ、または、ルール・セットの任意の組み合わせをプッシュすることができます。 

config-sync は変更を他方の Vyatta で即時にコミットし、それらのインターフェースをオンラインにするため、IP アドレスまたは他のネットワーク構成をプッシュしないことをお勧めします。 動的にインターフェースおよびサービスを使用可能にしたい場合は、このアクションをフェイルオーバー時に実行するように遷移スクリプトを使用する必要があります。 また、関連付けられた VLAN 上のゲートウェイ IP に対して、より多くの VRRP IP アドレスを使用することをお勧めします。これにより、フェイルオーバーの管理が簡単になります。

両方のマシンでの VRRP の基本構成は、以下のサンプル構成のようになります。

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

VRRP では、VRRP パケット (アドバタイズメント) が送信できるためには、その前にまず仮想インターフェースに実際の IP アドレスがバインドされている必要があります。 多くの場合、単にサブプライマリー・ネットから IP を追加できますが、これは将来のプロビジョンと競合する可能性があります。あるいは、既にすべてのプライマリー・サブネット IP がサーバーに割り振られている VLAN を経路指定することが必要な場合もあります。 これを回避するために、両方の Vyatta で、それらが相互に通信するために使用できるアウト・オブ・バンド IP アドレスのペアを使用できます。 以下に例を示します。

1 番目の Vyatta に対して、以下のように指定します。

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

2 番目の Vyatta に対して、以下のように指定します。

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

この例では、両方の Vyatta が、割り振られたサブネットと競合することのない独自の IP を持つことになります。 ほとんどどのようなプライベート範囲でも選択できますが、例えば、IPsec トンネルを介したサブネット範囲または Softlayer のものと競合する 10.0.0.0/8 アドレスなど、他のどの経路とも競合しないような小さなサブネットを選出することをお勧めします。

同期グループ名も追加することが必要になります。 すべての VRRP アドレスが、同じ同期グループの一部である必要があります。 これは、1 つのインターフェースで障害があると、同じ同期グループのすべてのインターフェースでもフェイルオーバーが起こるようにするためです。 そのようにしないと、最終的に、いくつかは MASTER で、他は BACKUP であるという状態になり得ます。 bond0 と bond1 のネイティブ VLAN 構成で同じ名前を使用してください。

注: bond0 および bond1 の VRRP 構成には rfc3768-compatibility の行を含めることができます。 これは、vif 上の VRRP には必要ではなく、bond0 および bond1 のネイティブ VLAN のみです。

新しくプロビジョンされたゲートウェイ・ペアでは、config-sync は最小限の構成のみになります。


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

マイグレーションする構成ブランチを決定するルールを追加する必要があります。 例えば、ファイアウォールおよび ipsec の構成がプッシュされるようにしたい場合、以下のコマンドを追加します。


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

これらがコミットされたら、ファイアウォールまたは ipsec の構成に対する変更はコミット時に他の vyatta にプッシュされるようになります。

**注:** 同期グループ (sync-group) と同期マップ (sync-map) は、2 つの別々のものです。 同期マップ構成は、構成変更を別の Vyatta にプッシュするルールのためのものです。 他方、同期グループは、VRRP が、一度に 1 つではなく、グループとしてフェイルオーバーするためのものです。 一方の構成が他方に影響することはありません。

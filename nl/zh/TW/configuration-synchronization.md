---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, sync, synchronize, config-sync

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# 同步化高可用性配置
{: #synchronizing-high-availability-configurations}

「高可用性 (HA)」配對中的兩個 {{site.data.keyword.vra_full}} (VRA) 必須要充分同步化其配置，讓這兩個裝置的行為方式類似。這是透過 `configuration sync-maps` 來執行，而且您可以選擇要同步化的配置部分。如果您在一台機器上進行變更，它會將已標示的配置推送到另一個裝置上。

這會同步化並儲存遠端裝置上本端裝置的執行中配置。不過，因為是確定處理程序的一個步驟，所以不會將配置儲存至本端裝置。
{: note}

某個系統所獨有的配置不應該與另一個系統同步化。例如，實際 IP 位址和 MAC 不應該同步化。`system config-sync` 配置節點本身及 `service https` 節點完全無法同步化。

下列範例說明 config-sync：

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

前兩行建立實際的 sync-map 本身。在這裡，`security firewall` 的配置段落將會在 `sync-map` 中設定。因此，在配置節點內所做的任何變更，都會推送至遠端裝置。不過，對 `security user` 所做的變更不會傳送，因為這與規則不符。您可以視需要讓 `sync-map` 特定或一般化。

接下來的三行指定遠端路由器的 `config-sync` 使用者和密碼、IP 以及要推送的 sync-map。任何符合 `TEST` 之規則的變更，都會使用這項登入資訊，前往 `remote-router 192.168.1.22`。請注意，使用 VRA API 執行此作業會進行 `REST` 呼叫，因此 HTTPS 伺服器必須在遠端路由器上執行（並且解除封鎖）。

若要同步化密碼的配置，例如 IPSec VPN 的預先共用密碼，待命系統必須已配置 `secrets` 群組，且 config-sync 使用者必須在該群組中。

```
set system login group secrets
set system login user vyatta authentication plaintext-password '****'
set system login user vyatta group secrets
```

只要您確定變更，就會發生 config-sync。請查看來自遠端裝置的錯誤訊息。如果配置不同步，則您需要在遠端裝置上修正該配置，才能再次作業。

您也可以使用 `show config-sync difference` 指令來查看配置差異。

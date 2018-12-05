---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# サービスの構成
仮想ルーター・アプライアンス (VRA) は、次のようなさまざまなサービスを実行するように構成できます。

`vyatta@vrouter# set service`

設定できるサービスには、以下のものがあります。

```
 > connsync        接続トラッキング同期 (conn-sync) サービス
 > dhcp-relay      動的ホスト構成プロトコル (DHCP) リレー・エージェント
 > dhcp-server     DHCP サーバー用の動的ホスト構成プロトコル (DHCP)
 > dhcpv6-relay    DHCPv6 リレー・エージェント・パラメーター
 > dhcpv6-server   IPv6 (DHCPv6) サーバー用の DHCP
 > dns             ドメイン・ネーム・サーバー (DNS) パラメーター
 > flow-monitoring フロー・モニタリングのトラフィック・モニター構成
 > https           Web サーバーの有効化/無効化
 > lldp            LLDP 設定
 > nat             ネットワーク・アドレス変換 (NAT)
 > netconf         NETCONF (RFC 6241)
 > portmonitor     ポート・モニター構成
 > sflow           データ・プレーンの sflow 構成
 > snmp            Simple Network Management Protocol (SNMP)
 > ssh             セキュア・シェル (SSH) プロトコル
 > telnet          Network Virtual Terminal Protocol (TELNET) プロトコルの有効化/無効化
 > twamp           Two-Way Active Measurement Protocol
```

`nat` はサービスの一部として構成され、`https` は Web GUI および API へのアクセスを制御します。 `connsync` は、ステートフル・ファイアウォール・フェイルオーバーなどのために 2 つの VRA が接続トラッキング同期を共有する方法を定義します。

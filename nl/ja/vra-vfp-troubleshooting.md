---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: troubleshooting, vfp, problems, nat, dnat, gre

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# VFP インターフェースのトラブルシューティング
{: #troubleshooting-your-vfp-interface}

このトピックでは、VFP インターフェースのトラブルシューティング情報を提供しています。

* VFP インターフェースは、`dp0bond0` (あるいは VIF や TUN) などの「実」インターフェースではありません。 ファイアウォールおよび NAT プロセスがトラフィックを適切に処理できるように、インターフェースを停止するためのプレースホルダーです。 通常のインターフェース同様、VFP 上にトラフィックを経路指定することは引き続きできますが、`tshark` およびその他のモニター・コマンドはトラフィックを表示しません。
* NAT の場合、IPsec により作成されたカーネル経路ではなく、VFP にトラフィックが経路指定されるようにするには、より具体的なサブネット範囲を使用する必要があります。 静的ルートが設定されていない場合、カーネル経路に従います。 これは、`show ip route x.x.x.x` を使用して検証できます。
* DNAT は、VFP から出てきて適切に処理されるはずですが、戻りトラフィックには引き続き静的ルートが設定されている必要があります。 IPsec インターフェース `dp0bond1` または `dp0bond0` (あるいは IPsec トラフィックを使用している任意のインターフェース) から NAT 以外のトラフィック・ヘッディングを検索します。
* VFP でのルーティング・プロトコルの使用は未テストです。
* VFP での GRE トンネルの使用は未テストです。

---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: vlan, route

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

# VLAN の経路指定
{: #routing-your-vlans}

Virtual Router Appliance は、同じネットワーク・インターフェース (例えば、`dp0bond0` または `dp0bond1`) 上に複数の VLAN を経路指定することができます。 これを行うには、スイッチ・ポートをトランク・モードに設定し、デバイス上で仮想インターフェース (VIF) を構成します。

以下に例を示します。

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

上記のコマンドは、`dp0bond0`  インターフェース上に 2 つの仮想インターフェースを作成します。 インターフェース `dp0bond0.1432` は VLAN 1432 のトラフィックを処理し、インターフェース `dp0bond0.1693` は VLAN 1693 のトラフィックを処理します。

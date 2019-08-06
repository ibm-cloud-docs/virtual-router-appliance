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

# 遞送 VLAN
{: #routing-your-vlans}

{{site.data.keyword.vra_full}} 可以透過相同的網路介面來遞送多個 VLAN（例如，`dp0bond0` 或 `dp0bond1`）。作法是將交換器埠設為幹線模式，並在裝置上配置虛擬介面 (VIF)。

例如：

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

以上指令會在 `dp0bond0` 介面上建立兩個虛擬介面。介面 `dp0bond0.1432` 會處理 VLAN 1432 的資料流量，而介面 `dp0bond0.1693` 會處理 VLAN 1693 的資料流量。

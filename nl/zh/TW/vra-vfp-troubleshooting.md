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

# VFP 介面疑難排解
{: #troubleshooting-your-vfp-interface}

本主題包含 VFP 介面的疑難排解資訊。

* VFP 介面不是「實際」介面，例如 `dp0bond0`（甚至是 VIF 或 TUN）。它是讓介面當掉的防火牆及 NAT 處理程序的位置保留元，以讓它們適當地處理資料流量。您仍然可以透過 VFP（例如一般介面）來遞送資料流量，但 `tshark` 及其他監視器指令將不會顯示任何資料流量。
* 使用 NAT，您必須使用更具體的子網路範圍，以取得遞送至 VFP 的資料流量，而不是 IPsec 所建立的核心路徑。如果未設定靜態路徑，則會遵循核心路徑。您可以使用 `show ip route x.x.x.x` 來測試此項目。
* 應該透過 VFP 適當地處理 DNAT，但傳回資料流量仍然需要靜態路徑集。請在 IPsec 介面 `dp0bond1` 或 `dp0bond0`（或任何使用 IPsec 資料流量的介面）中尋找非 NAT 資料流量標題。
* 未測試透過 VFP 使用遞送通訊協定。 
* 也未測試透過 VFP 使用 GRE 通道。

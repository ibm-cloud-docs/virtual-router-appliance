---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: faq, faqs, questions, gateway, vrouter, vyatta, 5400, 5600, password, traffic, firewall, provision, login, ha, high availability

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:note: .note}
{:important: .important}

# {{site.data.keyword.vra_full}} 的常見問題
{: #faqs-for-ibm-virtual-router-appliance}

以下是使用 {{site.data.keyword.vra_full}} (VRA) 時的常見問題。

## 何謂 VRA？
{: faq}

{{site.data.keyword.vra_full}} (VRA) 容許 IBM Cloud 客戶選擇性地透過具有防火牆、資料流量塑形、原則式遞送、VPN 及其他特性之主機的完整特性企業路由器，遞送專用與公用網路資料流量。所有 VRA 特性都是由客戶管理。VRA 讓 IBM Cloud 客戶能有一定的控制程度，這通常是保留給內部部署網路。

## 何謂閘道應用裝置？
{: faq}

「閘道應用裝置」可讓您使用 Web 入口網站或 API 來選擇要透過 VRA 遞送的網路區段 (VLAN)。您可以隨時變更 VLAN 選擇。「閘道應用裝置」也會處理 VRA 高可用性 (HA)，配置第二個 VRA，以在第一個 VRA 失敗時接管。

## 我有時會看到提及像是 "Vyatta" 和 "vRouter" 等詞。它們與 VRA 有什麼關係？
{: faq}

Vyatta 過去是開放程式碼的 PC 型路由器軟體，並且被完全購得且轉移為封閉程式碼。現在，"Vyatta" 和 "Vyatta OS" 說明從該項封閉程式碼專案衍生而來的商用軟體改寫。IBM VRA 納入 Vyatta OS 的元素，以及專門透過 IBM Cloud 提供的大量特性與服務加強功能。

"vRouter" 過去是短暫存在的 Vyatta 品牌重新定義（由當時的擁有者所進行）。在說明文件中看到時，它可以視為與 Vyatta 同義。

## 是否仍支援 Vyatta 5400？
{: faq}

截至 2019 年 3 月 31 日為止，IBM 將不再支援 Vyatta 5400。

## {{site.data.keyword.vra_full}} (Vyatta 5600) 相較於 Vyatta 5400 有什麼改良功能？
{: #what-improvements-does-the-virtual-router-appliance-vyatta-5600-have-over-the-vyatta-5400-}

Vyatta 5600 提供相較於 Vyatta 5400 的下列加強功能：

- 更快的傳輸量，每個 CPU 核心最多為 10Gbps
- 每個 IPsec VPN 階段作業的傳輸量增加超過 3 倍（進階加密標準）
- 提高路由器、流程及並行連線的容量
- 更新的標準支援，包括第 2 層通道通訊協定第 3 版 (L2TPv3)、網際網路金鑰交換第 2 版 (IKEv2)、安全雜湊演算法 2 (SHA-2)，以及 802.1Q 通道作業 (Q-in-Q) 封裝

## 關於 AT&T vRouter 5600 供應項目呢？
{: faq}

AT&T（早期為 Brocade）已宣布其 Brocade vRouter 5600 供應項目生命週期結束和終止支援。雖然 Brocade vRouter 5600 為 {{site.data.keyword.vra_full}} 提供基礎技術功能，但這項宣布並不適用於 IBM 客戶。IBM 客戶將繼續受支援使用此項新供應項目。

## 如何遞送 VRA？
{: faq}

您透過訂購「網路閘道」來取得 VRA。這項簡化的程序可讓您選擇一個資料中心及合適的 VRA 伺服器，以及您是否要部署 VRA 的 HA 配對。所有伺服器、作業系統及閘道應用裝置都是自動佈建的。佈建完成之後，您便可以使用「閘道應用裝置」介面，透過 VRA 來遞送 VLAN。您可以使用 SSH（安全 Shell）直接配置您的 VRA 伺服器，並使用「客戶入口網站」的「硬體詳細資料」區段中提供的密碼。

## 我的密碼安全嗎？
{: faq}

是的。所有 VRA 被指派的隨機密碼只有帳戶持有者可以看見。密碼很容易變更，因為是 SSH 公開金鑰及管理 IP 存取限制。

## 我可以在沒有「閘道應用裝置」的情況下取得 VRA 嗎？
{: faq}

可以，但只能管理 VRA 的公用及專用介面之間的資料流量。VLAN 及 HA 需要「閘道應用裝置」。

## 所有網路資料流量都透過 VRA 傳送嗎？
{: faq}

否。「閘道應用裝置」可讓您選取要透過 VRA 遞送的專用及公用網路區段 (VLAN)。您可以隨時變更及略過 VLAN 選擇。VRA 也可讓您定義適用於子網路或 IP 範圍的 IP 型規則。僅當包含那些子網路的 VLAN 透過 VRA 進行遞送時，這些規則才會運作。

## VRA 或專用的防火牆能避免新的伺服器佈建嗎？
{: faq}

是的。您應該儘可能不要鎖定網路，直到您移入要使用的伺服器為止。

在沒有客戶明確參與的情況下，原則會禁止 IBM 支援人員檢查或變更 VRA 或專用防火牆配置，因此在大部分情況下，支援人員並不知道 VRA 負責停滯或失敗的伺服器佈建。

客戶需負責確定已配置 VRA 或防火牆來允許自動化伺服器佈建，然後再訂購伺服器。由客戶管理的 VRA 或防火牆封鎖的佈建，需由客戶負責解決。此類佈建延遲不受 SLA 或額度的約束。如果客戶未迅速回應，則可能會將訂購的系統退回庫存（在客戶資料刪除之後）。

同樣地，如果在下訂單之後略過 VRA/防火牆，訂單仍然可能失敗。可能會有一小段時間嘗試進行自動化重試。最好是在沒有網路干擾的情況下進行整個佈建程序。

## IBM 提供哪些防火牆產品？
{: faq}

檢閱[本主題](/docs/infrastructure/fortigate-10g?topic=fortigate-10g-exploring-firewalls)，即可找到 IBM Cloud 中提供之所有防火牆產品的詳細比較。

## VRA 可能會混淆客戶支援中心的工作嗎？
{: faq}

是的，原因如上所述。VRA 是個「黑箱」：VLAN 進，VLAN 出，IBM 無法得知客戶如何利用中間的封包。

支援人員必定盡力，但對於 VRA 和專用防火牆：a) 客戶隱私權勝過連線功能，且 b) 我們的標準支援人員無法分析格式欠佳或高度複雜的 VRA/防火牆配置。

作為第一個診斷步驟，我們可能需要您暫時略過 VRA 或防火牆 VLAN。如果在此狀態時，失敗的佈建開始完成，則我們必須假設問題出在您的 VRA/防火牆配置。

## VRA 對我的網路效能有什麼影響？
{: faq}

請記住，即使他們看不到您，公用雲端也會與其他客戶分享網路。真正的最佳狀況 VRA 傳輸量取決於某個時間點可用的網路容量，加上資料必須行經的距離。

撇開這些變數，VRA 能夠在多個介面之間轉遞 80 Gbps 的未修改資料流量，這是使用約略的公式：每 10 Gbps 的傳輸量需要一個完整處理器核心（不包括超執行緒）。假設現行伺服器最高產出為 40 Gbps（2 x 10 Gbps 公用 + 2 x 10 Gb 專用），具有 8 個以上核心的伺服器應該有足夠的運算餘裕可處理多個常用 VRA 特性，並具有接近最佳狀況的網路效能

## 遺失 VSA 密碼的話怎麼辦？
{: faq}

如果能夠存取系統，請執行下列指令來設定新密碼：

```
set system login user [account] authentication plaintext-password [password]  
```

如果無法存取系統，您可以將裝置重新開機，並在 GRUB 功能表上使用密碼回復選項來重設 root 使用者密碼。

## 已被防火牆鎖定的話怎麼辦？
{: faq}

在測試可能有危險的防火牆規則時，`reboot at [time]` 建構很有用。

如果規則可運作，則使用 `reboot cancel` 指令取消重新開機。如果規則鎖定您的存取，只需要等待排定的重新開機發生。

如果無法存取系統，則可以使用重新開機來回復存取。在重新開機時，系統會讀取配置檔，而該配置檔會因為先前捨棄的項目而保持不變。

如果使用 IPMI 存取，您可以執行下列動作以回復存取：

1. 透過執行下列指令來停用不當的規則：

	```
	set security firewall name [firewall name] rule [rule number] disable
	commit
	```

2. 透過執行下列指令，從必要的介面取消連結整個具名規則集：

	```
	delete interfaces dataplane [interface] firewall [type][firewall name]
	commit
	```

**附註：**使用這些指令時若不正確，可能會清除您的介面配置。

## 為什麼要在「高可用性 (HA)」配對中執行兩個 Vyatta 裝置？
{: faq}

大部分雲端客戶都想要使用高可用性 (HA) 服務。如此一來，您的工作負載將在兩個（或多個）完全分開的（硬體）機器上進行管理，或甚至更好地在兩個不同的可用性區域（如資料中心）中進行管理，如果某一個發生故障，另一個將能夠繼續提供服務。如果某部機器故障，將由另一部機器進行「失效接手」，這表示服務可以繼續執行。這就是所謂的 "HA" 服務 — 它幾乎總是可用的。

## 如何啟用 VRA 的 root 登入？
{: faq}

若要啟用透過 SSH 的 root 存取權，請執行下列指令：

`set service ssh allow-root`

請注意，使用 SSH 容許 root 存取視為安全。存取 root Shell 的替代方案是以另一位使用者的身分登入，並在本端使用 `su -` 或容許「超級使用者」的 sudo 指令來提升為 root。

例如，若要以超級使用者身分配置 vyatta，請執行下列指令：

`set system login vyatta level superuser`

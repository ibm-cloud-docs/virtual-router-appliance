---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 如何將防火牆功能新增至 Vyatta（無狀態及有狀態）

使用 Brocade 5400 vRouter (Vyatta) 裝置時，將防火牆規則集套用至每一個介面是一種防火牆方法。每一個介面都有三個可能的防火牆實例（In、Out 及 Local），而且每一個實例都已套用規則。預設動作是「捨棄」，而且其規則容許以規則 1 到 N 的形式來套用特定資料流量。只要有相符項，防火牆就會套用相符規則的特定動作。

對於下面這三個防火牆實例，**只能套用其中一個**。

* **IN**：防火牆會過濾進入介面且遍訪 Brocade 系統的封包。您需要允許特定 SoftLayer IP 範圍存取前端及後端來進行管理（連線測試、監視等等）。
* **OUT**：防火牆會過濾離開介面的封包。這些封包可以遍訪或起源於 Brocade 系統。
* **LOCAL**：防火牆會透過系統介面來過濾送往 Brocade vRouter 系統本身的封包。如果存取埠是從不受限的外部 IP 位址進入 Brocade vRouter 裝置，則您應該建立其限制。<sup>1</sup>

使用下列步驟設定範例防火牆規則，以關閉 Brocade 5400 vRouter 介面的「網際網路控制訊息通訊協定 (ICMP)」*（連線測試 - IPv4 回應回覆訊息）*（這是無狀態動作；稍後會檢閱有狀態動作）：

1\. 在命令提示字元中鍵入 *show configuration* 指令，以查看已設定的配置。您將會看到已在裝置上設定的所有指令清單（如果您決定移轉且想要查看所有配置，這會十分有用）。請注意 *set firewall all-ping 'enable'* 指令，這表示仍會針對「裝置」啟用 ICMP。

2\. 鍵入 *configure*。

3\. 鍵入 *set firwall all-ping 'disable'*。

4\. 鍵入 *commit*。

如果您現在嘗試對 Brocade 5400 vRouter 裝置進行連線測試，則不會再收到回應。

為了能夠將防火牆規則指派給已遞送的資料流量，必須將規則套用至 Brocade 5400 vRouter 的**介面**，或是**建立區域**，然後套用至這些區域。

在此範例中，將會針對目前已使用的 VLAN 建立幾個區域。

**VLAN = 區域**

bond1 = dmz

bond1.2007 = prod（適用於正式作業）

bond0.2254 = private（適用於開發）

bond1.1280 = 保留供未來使用

bond1.1894 = 保留供未來使用

**建立防火牆規則**

實際建立區域之前，最好先建立要套用至這些區域的防火牆規則。在建立區域之前先建立規則，可讓您立即套用規則；但相對地，如果先建立區域再建立規則，則必須回到區域套用規則。。<sup>2</sup>

下列指令將：

* 建立名為 **dmz2private** 的防火牆規則，預設動作是捨棄任何封包
* 啟用記載功能，來記載名為 **dmz2private** 的規則的已接受及已拒絕資料流量


1\. 在命令提示字元中，鍵入 *configure*。

2\. 在提示中，輸入下列指令：

  * *set firewall name dmz2private default-action drop*
  * *set firewall name dmz2private enable-default-log*

下一組指令是讓 **iptables** 容許傳回已建立階段作業的資料流量。依預設，**iptables** 不容許這項作業，這就是需要明確規則的原因。

  * *set firewall name dmz2private rule 1 action accept*
  * *set firewall name dmz2private rule 1 state established enable*
  * *set firewall name dmz2private rule 1 state related enable*

第三組指令容許 TCP/UDP 通過埠 22（SSH 的預設值）。

  * *set firewall name dmz2private rule 2 action accept*
  * *set firewall name dmz2private rule 2 protocol tcp_udp*
  * *set firewall name dmz2private rule 2 destination port 22*

最後一組指令容許 TCP/UDP 通過埠 80（HTTP 的預設值）。

  * *set firewall name dmz2private rule 3 action accept*
  * *set firewall name dmz2private rule 3 protocol tcp_udp*
  * *set firewall name dmz2private rule 3 destination port 80*

3\. 鍵入 *commit*，確保在完成時採取所有規則。

4\. 在命令提示字元中鍵入 *show firewall name dmz2private*，以檢視配置。

我們建立的下一個防火牆規則將會套用至 **dmz** 區域。此規則將命名為 **public**。請在提示中輸入下列指令。

  * *set firewall name public default-action drop*
  * *set firewall name public enable-default-log*
  * *set firewall name public rule 1 action accept*
  * *set firewall name public rule 1 state established enable*
  * *set firewall name public rule 1 state related enable*

**建立區域**

區域是介面的邏輯呈現。下列指令將：

* 建立區域及名為 **dmz** 的原則，且預設動作是捨棄送往此區域的封包。
* 設定 **dmz** 原則，以使用 **bond1** 介面
* 設定 **prod** 原則，以使用 **bond1.2007** 介面
* 建立名為 **private** 的區域原則，預設動作是捨棄送往此區域的封包
* 設定名為 **private** 的原則，以使用 **bond0.2254** 介面

1\. 在提示中，輸入下列指令：

* *configure*
* *set zone policy zone dmz default-action drop*
* *set zone-policy zone dmz interface bond1*
* *set zone-policy zone prod default-action drop*
* *set zone-policy zone prod interface bond1.2007*
* *set zone-policy zone private default-action drop*
* *set zone-policy zone private interface bond0.2254*

2\. 使用下列指令，以設定區域的防火牆原則：

* *set zone-policy zone private from dmz firewall name dmz2private*
* *set zone-policy zone prod from dmz firewall name dmz2private*
* *set zone-policy zone dmz from prod firewall name public4*
* *commit*

請注意，如果您不想將防火牆規則套用至區域原則，則可以將它套用至特定介面。使用下面的指令，以將規則套用至介面。

* *set interfaces bonding bond1 firewall local name public*
* *commit*

**附註：**

<sup>1</sup>*有狀態* 防火牆會保留先前看過的流程表，而且可以根據封包與先前封包的關係來接受或捨棄封包。一般而言，一般會偏好普遍具有應用程式資料流量的有狀態防火牆。 

<span style="text-decoration: underline">*Brocade 5400 vRouter 不會追蹤具有預設配置之連線的狀態。除非符合下列其中一個條件，否則防火牆會是無狀態：*</span>

* 配置防火牆，並且任何規則都具有 state 參數集
* 配置防火牆 state-policy 
* 配置 NAT
* 啟用透通 Web Proxy 服務
* 啟用 WAN 負載平衡配置

*無狀態* 防火牆會分別考量每個封包。可以根據僅限基本存取控制清單 (ACL) 準則（例如 IP 或「傳輸控制通訊協定/使用者資料封包通訊協定 (TCP/UDP)」標頭中的來源及目的地欄位）來接受或捨棄封包。無狀態 Brocade 5400 vRouter 不會儲存連線資訊，而且不需要查閱每個封包與先前流程的關係，而且兩者都耗用少量記憶體及 CPU 時間。因此，在無狀態系統上，原始轉遞效能最佳。如果您不需要有狀態性特定的特性，則 Brocade 建議將路由器保持無狀態以獲得最佳效能。

<sup>2</sup>「區域」及「規則集」都有預設動作陳述式。使用「區域原則」時，預設動作是藉由 zone-policy 陳述式進行設定，並以規則 10,000 呈現。也請務必記住防火牆規則集的預設動作是**捨棄**所有資料流量。

<sup>3</sup>防火牆規則需要往外從 **prod** 流向 **dmz**。使用下列指令以協助疑難排解網路流程：*sudo tcpdump -i any host 10.52.69.202*

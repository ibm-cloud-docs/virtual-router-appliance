---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 將防火牆功能新增至 Virtual Router Appliance（無狀態及有狀態）
使用 Virtual Router Appliance (VRA) 時，將防火牆規則集套用至每一個介面是一種防火牆方法。每一個介面都有三個可能的防火牆實例（In、Out 及 Local），而且每一個實例都已套用規則。 

預設動作是「捨棄」，而且其規則容許以規則 1 到 N 的形式來套用特定資料流量。只要有相符項，防火牆就會套用相符規則的特定動作。

對於下面這三個防火牆實例，只能套用其中一個。

**IN**：防火牆會過濾進入介面且遍訪系統的封包。您需要允許特定 IP 範圍存取前端及後端來進行管理（連線測試、監視等等）。

**OUT**：防火牆會過濾離開介面的封包。這些封包可以遍訪或起源於 VRA 系統。

**LOCAL**：防火牆會使用系統介面來過濾送往 VRA 系統本身的封包。如果存取埠是從不受限的外部 IP 位址進入裝置，則您應該建立其限制。

使用下列步驟設定範例防火牆規則，以關閉 Virtual Router Appliance 介面的「網際網路控制訊息通訊協定 (ICMP)」（連線測試 - IPv4 回應回覆訊息）（這是無狀態動作；稍後會檢閱有狀態動作）：

1. 在命令提示字元中鍵入 `show configuration` 指令，以查看已設定的配置。您將會看到已在裝置上設定的所有指令清單（如果您決定移轉且想要查看所有配置，這會十分有用）。請注意 `set firewall all-ping enable` 指令，這表示仍會針對「裝置」啟用 ICMP。

2. 鍵入 `configure`。

3. 鍵入 `set firwall all-ping disable`。

4. 鍵入 `commit`。

如果您現在嘗試對 VRA 裝置進行連線測試，則不會再收到回應。

為了能夠將防火牆規則指派給已遞送的資料流量，必須將規則套用至 VRA 的介面，或是建立區域，然後套用至這些區域。

在此範例中，將會針對目前已使用的 VLAN 建立幾個區域。

 VLAN |區域
 ---- | ---- 
bond1 |dmz
bond1.2007 |prod（適用於正式作業）
bond0.2254 |private（適用於開發）
bond1.1280 |保留供未來使用
bond1.1894 |保留供未來使用

## 建立防火牆規則
實際建立區域之前，最好先建立要套用至這些區域的防火牆規則。在建立區域之前先建立規則，可讓您立即套用規則，這相當於建立區域，接著建立規則，然後傳回到區域以進行規則套用。

下列指令將：

* 建立名為 `dmz2private` 的防火牆規則，預設動作是捨棄任何封包。
* 啟用記載名為 `dmz2private` 的規則的已接受及已拒絕資料流量。

1. 在命令提示字元中，鍵入 `configure`。

2. 輸入下列指令：

	~~~
	set firewall name dmz2private default-action drop
	set firewall name dmz2private enable-default-log
	~~~

3. 輸入下一組指令，iptables 即會容許傳回已建立階段作業的資料流量。依預設，iptables 不容許這項作業，這就是需要明確規則的原因。

	~~~
	set firewall name dmz2private rule 1 action accept
	set firewall name dmz2private rule 1 state established enable
	set firewall name dmz2private rule 1 state related enable
	~~~

4. 輸入下列指令，以容許 TCP/UDP 通過埠 22（SSH 的預設值）。
	
	~~~
	set firewall name dmz2private rule 2 action accept
	set firewall name dmz2private rule 2 protocol tcp_udp
	set firewall name dmz2private rule 2 destination port 22
	~~~

5. 輸入下列指令，以容許 TCP/UDP 通過埠 80（HTTP 的預設值）。

	~~~
	set firewall name dmz2private rule 3 action accept
	set firewall name dmz2private rule 3 protocol tcp_udp
	set firewall name dmz2private rule 3 destination port 80
	~~~

6. 鍵入 `commit`，確保在完成時採取所有規則。

7. 在命令提示字元中鍵入 `show firewall name dmz2private`，以檢視新的配置。

8. 在提示中輸入下列指令，以建立要套用至 dmz 區域的防火牆規則。此規則將命名為 public。 

	~~~
	set firewall name public default-action drop
	set firewall name public enable-default-log
	set firewall name public rule 1 action accept
	set firewall name public rule 1 state established 	enable
	set firewall name public rule 1 state related enable
	~~~
	
## 建立區域

區域是介面的邏輯呈現。在此章節中，您將：

* 建立區域及名為 dmz 的原則，預設動作是捨棄送往此區域的封包
* 設定 dmz 原則，以使用 `bond1` 介面
* 設定 prod 原則，以使用 `bond1.2007` 介面
* 建立名為 `private` 的區域原則，預設動作是捨棄送往此區域的封包
* 設定名為 `private` 的原則，以使用 `bond0.2254` 介面

1. 在提示中，輸入下列指令：

	~~~
	configure
	set zone policy zone dmz default-action drop
	set zone-policy zone dmz interface bond1
	set zone-policy zone prod default-action drop
	set zone-policy zone prod interface bond1.2007
	set zone-policy zone private default-action drop
	set zone-policy zone private interface bond0.2254
	~~~
	
2. 使用下列指令，以設定區域的防火牆原則：

	~~~
	set zone-policy zone private from dmz firewall name 	dmz2private
	set zone-policy zone prod from dmz firewall name 	dmz2private
	set zone-policy zone dmz from prod firewall name 	public4
	commit
	~~~
	
如果您不想將防火牆規則套用至區域原則，則可以將它套用至特定介面。使用下面的指令，以將規則套用至介面。

~~~
set interfaces bonding bond1 fireawll local name public
commit
~~~

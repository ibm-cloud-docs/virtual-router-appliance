---

copyright:
  years: 2017
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Vyatta 5400 一般移轉問題
下表說明從 Vyatta 5400 裝置移轉至 IBM Virtual Router Appliance 之後您可能遇到的一般問題或行為變更。在某些情況下，它還包括能處理問題的暫行解決方法。

## 有狀態防火牆的介面型 Global-State 原則

### 問題
從 5.1 版開始，為狀態防火牆設定 "State of State-policy" 的行為已變更。在 5.1 版之前的版本中，如果您設定有狀態防火牆為 `state - global -state -policy`，vRouter 會自動新增隱含的 `Allow` 規則，使階段作業的傳回通訊自動化。

在 5.1 版以及更新版本中，您必須在 Virtual Router Appliance 上新增 `Allow` 規則設定。有狀態設定在 Vyatta 5400 裝置上的作用是根據介面，在 VRA 裝置上則是根據通訊協定。

### 暫行解決方法
如果 `firewall-in` 規則已套用在 Ingress/Inside 介面上，則 `Firewall-out` 規則必須套用在 Egress/Outside 介面上。否則，傳回資料流量將在 Egress/Outside 介面上捨棄。        

## 防火牆規則中的 State-Enable

### 問題
如果未配置 `global-state-policy`，則此行為變更不受影響。

還有，如果您在每一個規則中使用 `state enable` 選項而非 `global-state-policy`，則行為變更不受影響。

## 區域型原則：當地時區處理

### 問題
沒有「當地時區」虛擬介面可指派給區域原則。 

### 暫行解決方法
可對實體介面套用區域型防火牆以及對迴圈介面套用介面防火牆，來模擬此行為。迴圈介面中的防火牆會過濾在路由器進出的一切。 

例如：

set security zone-policy zone internal default-action 'drop'
set security zone-policy zone internal description 'Private zone'
set security zone-policy zone internal interface 'dp0bond0'
set security zone-policy zone internal to external firewall 'internal-2-external'
set security zone-policy zone internal to ovpn firewall 'internal-2-ovpn'

set security zone-policy zone ovpn default-action 'drop'
set security zone-policy zone ovpn description 'OpenVPN'
set security zone-policy zone ovpn interface 'vtun0'
set security zone-policy zone ovpn to external firewall 'ovpn-2-external'
set security zone-policy zone ovpn to internal firewall 'ovpn-2-internal'
 
set interfaces loopback lo firewall local 'Local'
 
set security firewall name ovpn-2-external default-action accept
set security firewall name ovpn-2-internal default-action accept
 
set security firewall name external-2-ovpn default-action accept
set security firewall name external-2-internal default-action accept
 
set security firewall name internal-2-external default-action accept
set security firewall name internal-2-ovpn default-action accept
 
set security firewall name Local default-action 'drop'
set security firewall name Local 'default-log'
set security firewall name Local rule 10 action 'accept'
set security firewall name Local rule 10 description 'RIP' ("/opt/vyatta/etc/cpp.conf" )
```

## 防火牆、NAT、遞送及 DNS 作業的順序

### 問題
在 Masquerade Source NAT 部署於 IBM Virtual Router Appliance 的情境中，您不能使用防火牆來決定主機對網際網路的存取。這是因為 post NAT 位址會是相同的。

此作業對於 Vyatta 5400 裝置是可行的，因為防火牆作業是在 NAT 之前進行，容許限制主機存取網際網路。

### 暫行解決方法
VRA 需要新的遞送方法：
![routing dns](./images/routing-dns.png)

## 原則型遞送表

### 問題
配置檔中的 "Table" 一字在 v5400「原則型遞送」中是選用的，但對於 VRA 而言，如果此動作為 'accept'，則 **Table** 欄位是必要的。如果在 VRA 配置中，動作是 'drop'，則 Table 欄位是選用的。

### 暫行解決方法
在 Vyatta 5400「原則型遞送」中，"Table Main" 是可用的選項。VRA 中的對等項目是 "routing-instance default"。

## 通道介面中的原則型遞送

### 問題
在 Virtual Router Appliance PBR（原則型遞送）上，原則可以套用至入埠資料流量的資料平面介面，而不能套用至迴圈、通道、橋接器、OpenVPN、VTI 及 IP 未編號介面。

### 暫行解決方法
此問題目前沒有暫行解決方法。

## TCP-MSS

### 問題
IBM Virtual Router Appliance 使用 nftables，且不支援 TCP-MSS。

### 暫行解決方法
此問題目前沒有暫行解決方法。

## OpenVPN

### 問題
在 Virtual Router Appliance 上使用 'push-route' 參數時，OpenVPN 未能開始運作。

### 暫行解決方法
使用 'openvpn-option' 參數而非 'push-route'。

## GRE/VTI over IPSEC + OSPF

### 問題
* 當 VIF 配置了多個子網路之後，資料流量無法透過 VRA 中的那些子網路傳送。                                      * InterVlan Routing 在 VRA 上無法運作。
### 暫行解決方法
使用「隱含的容許」規則，以接受透過 VIF 介面的資料流量。

## IPSec

### 問題
IPSec（基於字首）無法與「IN 過濾器」搭配使用。

### 暫行解決方法
使用 IPSec (VTI BASED)。

## IPSEC 'match-none"

### 問題
使用 Vyatta 5400 裝置時，容許下列防火牆規則：

set firewall name allow rule 10 ipsec 

不過，使用 IBM Virtual Router Appliance 時，沒有 IPSec。

### 暫行解決方法
VRA 裝置的可能替代規則：

```
   match-ipsec  Inbound IPsec packets
   match-none   Inbound non-IPsec packets                                                                                                                
```

## IPSEC 'match-ipsec"

### 問題
使用 Vyatta 5400 裝置時，容許下列防火牆規則：

set firewall name OUTSIDE_LOCAL rule 50 action 'accept'
set firewall name OUTSIDE_LOCAL rule 50 ipsec 'match-ipsec'

不過，使用 IBM Virtual Router Appliance 時，沒有 IPSec。

### 暫行解決方法
新增通訊協定 'ESP' 及 'AH'（分別為 IP 通訊協定 50 及 51）。

'action' 規則可以是 'accept' 或 'drop'，如下所示：

```
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> destination port 4500
set security firewall name <name> rule <rule-no> protocol udp
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol ah
set security firewall name <name> rule <rule-no> action accept
set security firewall name <name> rule <rule-no> protocol esp
```

## 網站對網站 IPSEC 與 DNAT

### 問題
IPSec（基於字首）無法與 DNAT 搭配使用。

```
伺服器 (10.71.68.245) -- vyatta 1 (11.0.0.1) 
===S-S-IPsec=== (12.0.0.1) 
vyatta 2 -- 用戶端 (10.103.0.1)
Tun50 172.16.1.245
```

上述 Snippet 是 IPSec 封包在 Vyatta 5400 中解密之後進行 DNAT 轉換的小型設定範例。在此範例中，有兩個 vyattas：'vyatta1 (11.0.0.1)' 及 'vyatta2 (12.0.0.1)'。會在 '11.0.0.1' 及 '12.0.0.1' 之間建立對等作業。在此情況下，用戶端從來源 '10.103.0.1' 端對端鎖定目標 '172.16.1.245'。此情境的預期行為是將目的地位址 '172.16.1.245' 轉換成封包標頭中的 '10.71.68.245'。
一開始，Vyatta 5400 裝置會對入埠 IPSec 執行 DNAT，使用連線追蹤表格終止介面及溫和地將資料流量傳回到 IPsec 通道。

在 Virtual Router Appliance 上，配置的運作方式不同。雖建立了階段作業，在連線追蹤表格反轉 DNAT 變更之後，返回資料流量卻略過 IPsec 通道。然後 VRA 透過線路傳送封包時沒有使用 IPsec 加密。上游裝置並未預期有此資料流量，很可能會捨棄它。端對端連線功能會中斷，這是預期的行為。  

### 暫行解決方法
為了配合上述網路情境，IBM 建立了 RFE。 

由於目前正在評量 RFE，我們建議下列暫行解決方法：

**介面配置指令**

```
set interfaces dataplane dp0p192p1 address '11.0.0.1/30'
set interfaces dataplane dp0p224p1 address '10.0.0.2/30'
set interfaces dataplane dp0p224p1 policy route pbr 'Backwards-DNAT'
set interfaces loopback lo address '169.254.1.1/24'
set interfaces tunnel tun50 address '169.254.240.1/32'
set interfaces tunnel tun50 encapsulation 'gre'
set interfaces tunnel tun50 local-ip '169.254.1.1'
set interfaces tunnel tun50 remote-ip '169.254.1.1'
```

**VPN 配置指令**

```
set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'
```

**NAT 配置指令**

```
set service nat destination rule 10 destination address '172.16.1.245'
set service nat destination rule 10 inbound-interface 'tun50'
set service nat destination rule 10 source address '10.103.0.1'
set service nat destination rule 10 translation address '10.71.68.245'
set service nat source rule 10 destination address '10.103.0.1'
set service nat source rule 10 'log'
set service nat source rule 10 outbound-interface 'tun50'
set service nat source rule 10 source address '10.71.68.245'
set service nat source rule 10 translation address '172.16.1.245'
```

**通訊協定配置指令**

```
set protocols static interface-route 172.16.1.245/32 next-hop-interface 'tun50'
set protocols static table 50 interface-route 0.0.0.0/0 next-hop-interface 'tun50'
```

**PBR 配置指令**

```
set policy route pbr Backwards-DNAT description 'Get return traffic back to tunnel for DNAT'
set policy route pbr Backwards-DNAT rule 10 action 'accept'
set policy route pbr Backwards-DNAT rule 10 address-family 'ipv4'
set policy route pbr Backwards-DNAT rule 10 destination address '10.103.0.0/24'
set policy route pbr Backwards-DNAT rule 10 source address '10.71.68.0/24'
set policy route pbr Backwards-DNAT rule 10 table '50'
```

## PPTP

### 問題
PPTP 在 Virtual Router Appliance 中不再受到支援。                                                                                                                             

### 暫行解決方法
改用 L2TP 通訊協定。

## 針對 IPSec 重新啟動的 Script

### 問題
每當 VRRP 虛擬位址新增至高可用性 VPN 的 IBM Virtual Router Appliance 時，您必須重新起始設定 IPsec 常駐程式。這是因為 IPsec 服務只會接聽對於起始設定 IKE 服務常駐程式時，VRA 上存在之位址的連線。

若為搭配 VRRP 的一對 VRA，待命路由器可能沒有起始設定期間裝置上存在之 VRRP 虛擬位址（如果主要路由器沒有該存在之位址）。因此，當 VRRP 狀態轉移發生時，若要重新起始設定 IPsec 常駐程式，則在主要路由器及備份路由器上執行下列指令：
```
interfaces dataplane interface-name vrrp vrrp-group group-id notify
```

## 最新計數及最新時間

### 問題

對於使用任何位址的 SSH，下列規則的意圖是要將 SSH 連線限制為每 30 秒 3 次：
```
set firewall name localGateway rule 300 action 'drop'
set firewall name localGateway rule 300 description 'Deter SSH brute force'
set firewall name localGateway rule 300 destination port '22'
set firewall name localGateway rule 300 protocol 'tcp'
set firewall name localGateway rule 300 recent count '3'
set firewall name localGateway rule 300 recent time '30'
set firewall name localGateway rule 300 state new 'enable'
```

在 IBM Virtual Router Appliance 上，此規則有下列問題：

* 最新計數及最新時間的選項已淘汰。

* 因為前一個問題，此規則無法如預期運作，並且將封鎖對已套用介面的所有 SSH 連線。

### 暫行解決方法
改用 CPP。

## 設定系統連線追蹤問題

### 問題
```
set system conntrack expect-table-size '8192'
set system conntrack hash-size '375000'
set system conntrack modules ftp 'disable'
set system conntrack modules 'gre'
set system conntrack modules h323 'disable'
set system conntrack modules nfs 'disable'
set system conntrack modules pptp 'disable'
set system conntrack modules sip 'disable'
set system conntrack modules sqlnet 'disable'
set system conntrack modules tftp 'disable'
set system conntrack table-size '3000000'
```

## 設定系統連線追蹤逾時

### 問題
```
set system conntrack timeout icmp '30'
set system conntrack timeout other '600'
set system conntrack timeout tcp close '10'
set system conntrack timeout tcp close-wait '60'
set system conntrack timeout tcp established '432000'
set system conntrack timeout tcp fin-wait '120'
set system conntrack timeout tcp last-ack '30'
set system conntrack timeout tcp syn-recv '60'
set system conntrack timeout tcp syn-sent '120'
set system conntrack timeout tcp time-wait '60'
```

## 時間型防火牆

### 問題
```
set firewall name PRIV_SERVICE_IN rule 58 action 'accept'
set firewall name PRIV_SERVICE_IN rule 58 description '586427 Acesso a base de dados ate 22-2-18'
set firewall name PRIV_SERVICE_IN rule 58 destination address '10.150.156.57'
set firewall name PRIV_SERVICE_IN rule 58 destination port '3306'
set firewall name PRIV_SERVICE_IN rule 58 protocol 'tcp'
set firewall name PRIV_SERVICE_IN rule 58 source address '10.150.156.104'
set firewall name PRIV_SERVICE_IN rule 58 time startdate '2017-08-22'
set firewall name PRIV_SERVICE_IN rule 58 time stopdate '2018-02-22'
```

## TCP-MSS

### 問題
```
set interfaces tunnel tun3 address '172.17.175.45/30'
set interfaces tunnel tun3 encapsulation 'gre'
set interfaces tunnel tun3 local-ip '169.55.223.76'
set interfaces tunnel tun3 mtu '1476'
set interfaces tunnel tun3 multicast 'disable'
set interfaces tunnel tun3 policy route 'change-mss'(in 18.x unable to apply tcp-mss using PBR only option is to set on interface directly which i believe is not equivalent to pbr .
set interfaces tunnel tun3 remote-ip '104.129.200.34'
```
```
set policy route change-mss rule 1 protocol 'tcp'
set policy route change-mss rule 1 set tcp-mss '1436'
set policy route change-mss rule 1 tcp flags 'SYN
```

## S-S Ipsec VPN 中特定的應用程式或埠中斷

### 問題

```
vyatta@v5600dallas09# set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote
可能完成：
   <Enter> 執行現行指令
   port    任何 TCP 或 UDP 埠
   prefix  遠端 IPv4 或 IPv6 字首                                                                                                                                     set security vpn ipsec esp-group ESP lifetime '30000'
set security vpn ipsec esp-group ESP proposal 1 encryption 'aes128'
set security vpn ipsec esp-group ESP proposal 1 hash 'sha1'
set security vpn ipsec ike-group IKE lifetime '60000'
set security vpn ipsec ike-group IKE proposal 1 encryption 'aes128'
set security vpn ipsec ike-group IKE proposal 1 hash 'sha1'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication mode 'pre-shared-secret'
set security vpn ipsec site-to-site peer 12.0.0.1 authentication pre-shared-secret 'thekey'
set security vpn ipsec site-to-site peer 12.0.0.1 default-esp-group 'ESP'
set security vpn ipsec site-to-site peer 12.0.0.1 ike-group 'IKE'
set security vpn ipsec site-to-site peer 12.0.0.1 local-address '11.0.0.1'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 local prefix '172.16.1.245/30'
set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote prefix '10.103.0.0/24'                                          set security vpn ipsec site-to-site peer 12.0.0.1 tunnel 1 remote port 21 (ftp)
```

## 記載行為的重大變更

### 問題
從每個階段作業到每個封包記載，Vyatta 5400 裝置與 IBM Virtual Router Appliance 之間的記載行為有重大變更。
* 階段作業記載：記錄有狀態的階段作業狀態轉移

* 封包記載：記錄符合規則的所有封包。由於封包記載是以「封包單位」記錄在日誌檔中，因此傳輸量及磁碟容量的壓力明顯減少。

* vRouter 的記載功能可用來擷取防火牆活動。和任何記載功能一樣，唯有當您在進行特定問題的疑難排解時才應啟用此功能，並且應儘快停用記載。

管理防火牆/NAT 階段作業的有狀態防火牆是以「階段作業單位」寫入。建議使用階段作業記載。每一個設定範例說明如下：

**階段作業 / 記載**

* `security firewall session-log <protocol>`
* `system syslog file <filename> facility <facility> level <level>`

**封包記載防火牆**

* `security firewall name <name> default-log <action>`
* `security firewall name <name> rule <rule-number> log`

**NAT**

* `service nat destination rule <rule-number> log`
* `service nat source rule <rule-number> log PBR`
* `policy route pbr <name> rule <rule-number> log`

**QoS**

* `policy qos name <policy-name> shaper class <class-id> match <rule-name>` 

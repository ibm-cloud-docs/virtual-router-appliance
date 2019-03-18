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
{:faq: data-hd-content-type='faq'}

# IBM Virtual Router Appliance 的常見技術問題
{: #technical-faqs-for-ibm-virtual-router-appliance}

下列常見問題可處理 IBM© Virtual Router Appliance (VRA) 的配置，以及從 Vyatta 5400 移轉至 VRA。

## 如何容許來自專用 VLAN 上主機的網際網路連結資料流量？
{:faq}

此資料流量需要取得公用來源 IP，因此「來源 NAT」需要使用 VRA 的公用 IP 來假冒專用 IP。

```
set service nat source rule 1000 description 'SNAT traffic from private VLANs to Internet'
set service nat source rule 1000 outbound-interface 'dp0bond1'
set service nat source rule 1000 source address '10.0.0.0/8'
set service nat source rule 1000 translation address masquerade
```

上述配置只會透過源自專用 `10.0.0.0/8` 網路中伺服器的資料流量來執行 SNAT。

這確保它不會干擾已有網際網路可遞送來源位址的封包。

## 如何過濾網際網路連結資料流量並且只容許特定通訊協定/目的地？
{:faq}

這是需要結合「來源 NAT」及防火牆時的一般問題。

設計規則集時，請記住 VRA 中的作業順序。

簡言之，請在 SNAT *之後* 套用防火牆規則。

若要在防火牆中封鎖所有送出的資料流量，但容許特定 SNAT 流程，您需要將過濾邏輯移至 SNAT。

例如，若只要容許主機的 HTTPS 網際網路連結資料流量，SNAT 規則會是：

```
set service nat source rule 10 description 'SNAT https traffic from server 10.1.2.3 to Internet'
set service nat source rule 10 destination port 443
set service nat source rule 10 outbound-interface 'dp0bond1'
set service nat source rule 10 protocol 'tcp'
set service nat source rule 10 source address '10.1.2.3'
set service nat source rule 10 translation address '150.1.2.3'
```

`150.1.2.3` 將是 VRA 的公用位址。 

強烈建議使用 VRA 的 VRRP 公用位址，以讓您區分主機與 VRA 公用資料流量。

假設 `150.1.2.3` 是 VRRP VRA 位址，而 `150.1.2.5` 是實際 dp0bond1 位址。

`dp0bond1 out` 上所套用的有狀態防火牆會是：

```
set security firewall name TO_INTERNET default-action drop
set security firewall name TO_INTERNET rule 10 action `accept`
set security firewall name TO_INTERNET rule 10 description 'Accept host traffic to Internet - SNAT to VRRP'
set security firewall name TO_INTERNET rule 10 source address '150.1.2.3'
set security firewall name TO_INTERNET rule 10 state 'enable'
set security firewall name TO_INTERNET rule 20 action `accept`
set security firewall name TO_INTERNET rule 20 description 'Accept VRA traffic to Internet'
set security firewall name TO_INTERNET rule 20 source address '150.1.2.5'
set security firewall name TO_INTERNET rule 20 state 'enable'
```

請注意，「來源 NAT」與防火牆的組合可以達到所需的設計目標。 

請確定規則適合您的設計，而且沒有其他規則容許應該封鎖的資料流量。 

## 如何使用以區域為基礎的防火牆來保護 VRA 本身？
{:faq}

VRA 沒有`當地時區`。

您可以改為利用 Control Plane Policing (CPP) 功能，因為它在迴圈上套用為 `local` 防火牆。

請注意，這是無狀態防火牆，而且您需要明確地容許源自 VRA 本身之出埠階段作業的傳回資料流量。

## 如何限制 SSH 並封鎖來自網際網路的連線？
{:faq}

最好不要容許來自網際網路的 SSH 連線，並使用另一種方式來存取專用位址（例如 SSL VPN）。

依預設，VRA 接受所有介面上的 SSH。
若只要接聽專用介面上的 SSH 連線，需要設定下列配置：

```
set service ssh listen-address '10.1.2.3'
```

請記住，IP 位址需要取代為屬於 VRA 的位址。

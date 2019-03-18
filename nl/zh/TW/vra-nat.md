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

# 搭配使用 NAT 與以字首為基礎的 IPsec
{: #using-nat-with-prefix-based-ipsec}

在[使用 IPsec 及區域防火牆配置 VFP 介面](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-configuring-a-vfp-interface-with-ipsec-and-zone-firewalls)主題中，我們已建立 VFP 介面，並將它設為與 IPsec 通道搭配使用。 

我們可以在 NAT 規則中使用相同的介面，以及入埠和出埠介面宣告，但有一個額外警告。 

以下是一些範例 NAT 規則：

```
set service nat destination rule 10 destination address '172.16.200.2'
set service nat destination rule 10 inbound-interface 'vfp0'
set service nat destination rule 10 translation address '10.177.137.251'
set service nat source rule 10 outbound-interface 'vfp0'
set service nat source rule 10 source address '10.177.137.251'
set service nat source rule 10 translation address '172.16.200.2'
```

前一個範例是相同 IP 的標準雙向一對一來源及目的地 NAT。但是，若要確保 NAT 資料流量適當地通過通道，您需要另一端的靜態路徑：

```
set protocols static interface-route 172.16.100.2/32 next-hop-interface 'vfp0'
```

使用靜態路徑的原因是因為 IPsec 常駐程式已建立遠端字首的核心路徑：

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
```

在來源 `10.177.137.251` 對 `172.16.100.2` 的連線測試，資料流量經過 `dp0bond1` 離開、無法傳輸通道，而且永遠無法正確地符合 NAT 規則。靜態路徑修正如下：

```
K    *> 172.16.100.0/24 via 169.63.66.49, dp0bond1
S    *> 172.16.100.2/32 [1/0] is directly connected, vfp0
```

這會針對要通過 `vfp0` 的資料流量建立更具體的路徑。 

此時，NAT 將依配置運作，而且資料流量會流經通道。 

**附註：**使用 NAT，您需要 CIDR 小於 IPsec 遠端字首的路徑（大小不能相同），而 IPsec 遠端字首指向透過 `vfp0` 虛擬介面的資料流量。

一切就緒之後，即可進行連線測試並驗證：

```
[root@acs-jmat-migserver ~]# ping 172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.2 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.3 ms
^C
--- 172.16.100.1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 44.247/44.431/44.727/0.272 ms
 
vyatta@acs-jmat-migsim01:~$ show nat source translations
Pre-NAT                 Post-NAT                Prot    Timeout
10.177.137.251:7553     172.16.200.2:7553       icmp    48
```

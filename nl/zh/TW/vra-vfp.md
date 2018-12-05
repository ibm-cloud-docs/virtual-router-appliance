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

# 使用 IPsec 及區域防火牆配置 VFP 介面
IPSec 資料包在到達時會透過防火牆規則處理，然後再解除封裝。出現的新資料包根本未與介面相關聯。一般而言，這不是問題，而且可以移至目的地介面，但是區域防火牆會防止資料包繼續進行。會捨棄未來自區域原則中介面的任何資料包。不過，VFP 介面會通知區域防火牆有關資料包確實來自介面，以容許套用規則。 

若要配置 VFP 介面來使用 IPsec 資料流量，請先定義具有單一 IP 的 VFP 來建立特性點：

```
set interfaces virtual-feature-point vfp0 address '192.168.123.123/32'
```

然後將 VFP 新增至通道：

```
set security vpn ipsec site-to-site peer 50.23.177.59 tunnel 1 uses 'vfp0'
```

接下來，將介面新增至區域（在此情況下，是具有 dp0bond1 的 INTERNET 區域）：

```
set security zone-policy zone INTERNET interface 'vfp0'
```

現在對伺服器進行連線測試：

```
[root@acs-jmat-migserver ~]# ping -c 5  172.16.100.1
PING 172.16.100.1 (172.16.100.1) 56(84) bytes of data.
64 bytes from 172.16.100.1: icmp_seq=1 ttl=63 time=44.9 ms
64 bytes from 172.16.100.1: icmp_seq=2 ttl=63 time=44.7 ms
64 bytes from 172.16.100.1: icmp_seq=3 ttl=63 time=44.6 ms
64 bytes from 172.16.100.1: icmp_seq=4 ttl=63 time=44.3 ms
64 bytes from 172.16.100.1: icmp_seq=5 ttl=63 time=44.8 ms

--- 172.16.100.1 ping statistics ---

5 packets transmitted, 5 received, 0% packet loss, time 4006ms

rtt min/avg/max/mdev = 44.377/44.728/44.996/0.243 ms
```

您可以在多個通道上使用相同的 VFP 介面，或為其他通道建立不同的 VFP，只需要確保任何其他 VFP 都在區域中，並且具有容許未封裝訊框之資料流量的規則。

您也可以將 VFP 新增至自己的區域。例如：

```
set security zone-policy zone SERVERS to TUNNEL firewall 'ALLOWALL'
set security zone-policy zone TUNNEL interface 'vfp0'
set security zone-policy zone TUNNEL to SERVERS firewall 'ALLOWALL'
```

雖然 VFP 介面是一個「實際」介面，但它可以使用 `tshark` 這類指令進行監視，而且可以直接遞送資料流量，這麼做並不實用。將會捨棄到達另一端的任何資料流量，而這端不符合通道原則。它不像 VTI 介面這麼多用途。

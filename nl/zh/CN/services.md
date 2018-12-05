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

# 配置服务
可以配置在虚拟路由器设备 (VRA) 上运行的各种服务，包括：

`vyatta@vrouter# set service`

可能的补全包括：

```
 > connsync        连接跟踪同步 (conn-sync) 服务
 > dhcp-relay      动态主机配置协议 (DHCP) 中继代理
 > dhcp-server     支持 DHCP 服务器的动态主机配置协议 (DHCP)
 > dhcpv6-relay    DHCPv6 中继代理参数
 > dhcpv6-server   支持 IPv6 的 DHCP (DHCPv6) 服务器
 > dns             域名服务器 (DNS) 参数
 > flow-monitoring 流监视流量监视配置
 > https           启用/禁用 Web 服务器
 > lldp            LLDP 设置
 > nat             网络地址转换 (NAT)
 > netconf         NETCONF (RFC 6241)
 > portmonitor     端口监视器配置
 > sflow           用于数据平面的 sflow 配置
 > snmp            简单网络管理协议 (SNMP)
 > ssh             安全 Shell (SSH) 协议
 > telnet          启用/禁用网络虚拟终端 (TELNET) 协议
 > twamp           双向主动度量协议
```

`nat` 作为服务的一部分进行配置，`https` 用于控制对 Web GUI 和 API 的访问。`connsync` 定义两个 VRA 可以如何共享连接跟踪同步，以同步有状态防火墙故障转移等内容。

---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 路由 VLAN
虚拟路由器设备能够通过同一网络接口（例如，`dp0bond0` 或 `dp0bond1`）路由多个 VLAN。实现方法是将交换机端口设置为中继方式，并在设备上配置虚拟接口 (VIF)。

例如：

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

以上命令在 `dp0bond0` 接口上创建两个虚拟接口。接口 `dp0bond0.1432` 用于处理 VLAN 1432 的流量，而接口 `dp0bond0.1693` 用于处理 VLAN 1693 的流量。

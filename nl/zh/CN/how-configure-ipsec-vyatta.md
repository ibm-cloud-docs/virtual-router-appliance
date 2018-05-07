---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 在 Vyatta 5400 上配置 IPSec

对于因特网安全协议 (IPSec) 隧道，Brocade 5400 vRouter (Vyatta) 设备将被当作“本地”设备。以下每个命令都将执行不同的功能来配置 IPSec 站点到站点连接。请注意，此 IPSec 站点到站点示例演示的是 SoftLayer 公用网络上的隧道；对于专用 IPSec 站点到站点连接，请使用 **bond0**。

1. 向隧道“指示”**interface bond1** 的用途：

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. 设置两阶段隧道的第一个阶段。以下命令将用于：

  * 创建名为 **test** 的新 **ike** 组，并使用 **dh-group** 作为密钥交换类型。
  * 指定要使用的加密类型；如果未设置此项，设备将使用 **aes128** 作为缺省值
  * 使用散列函数 **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. 设置两阶段隧道的第二个阶段。以下命令将用于：

  * 禁用完美前向保密 (PFS)，因为并非所有设备都可以使用此功能。（命令中的 esp 是加密的第二部分。）
  * 指定要使用的加密类型；如果未设置此项，设备将使用 **aes128** 作为缺省值
  * 使用散列函数 **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. 设置 IPSec 站点到站点加密参数。以下命令将用于：

  * 指定远程端 IP，以及 IPSec 将使用预共享私钥
  * 使用远程 IP 和私钥 TestPSK
  * 将隧道的缺省 **esp** 组设置为 TestESP
  * “指示”IPSec 使用先前所定义的 ike-group TestIKE<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. 为 IPSec 隧道创建映射。根据材料中的示例，以下命令将用于：

  * “指示”隧道将远程 IP 169.54.254.117 映射到 bond1 的本地 IP 地址 50.97.240.219
  * 仅将本地服务器接口上 10.54.9.152/29 子网的 IP 地址路由到远程服务器 169.54.254.117
  * 将隧道 1 远程流量 169.54.254.117 路由到远程子网 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

下一步是设置远程端设备，即 Brocade 5400 vRouter 6.6.5 R

  * 使用刚才配置的设备（在操作方式下配置的设备）输入 show configuration commands 命令。这将显示用于设置该设备的命令列表。
  * 将这些命令复制到文本编辑器。用来设置本地设备的命令将用于通过修改 IP，使其指向 SoftLayer 上的 Brocade 5400 vRouter 6.6.5R 设备来设置远程服务器。

下面是先前使用的远程端配置。本地端配置所需的更改以粗体显示。

远程端配置：

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret*（已交换站点到站点对等 IP）

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117***（已交换本地 IP 地址）

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29***（尚未交换本地前缀和远程前缀）

* 将新命令复制并粘贴到远程服务器（确保处于配置方式），输入 commit，然后保存。
* 输入 run show vpn ike sa 以查看隧道现在是否已建立。

下面是对已经完成的操作的概述：仅将本地接口（bond1 的 50.97.240.219）上“10.54.9.152/29”子网的 IP 地址路由到 IP 地址为 169.54.254.117 的接口中远程服务上的 192.168.1.2/32 子网。

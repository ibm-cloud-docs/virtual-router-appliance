---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: basics, vra, access, configure, gui

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# 访问并配置 IBM 虚拟路由器设备
{: #accessing-and-configuring-the-ibm-virtual-router-appliance}

VRA 可以使用远程控制台会话通过 SSH 进行配置，也可以通过登录到 Web GUI 进行配置。缺省情况下，无法通过公共因特网来访问 Web GUI。要启用 Web GUI，请首先通过 SSH 登录。

在 VRA 的 shell 和接口外部配置 VRA 可能会生成意外结果，因此建议不要这样做。
{: note}

## 使用 SSH 访问设备
{: #accessing-the-device-using-ssh}

大多数基于 UNIX 的操作系统（如 Linux、BSD 和 Mac OSX）在其缺省安装中包含 OpenSSH 客户机。Windows 用户可以下载 SSH 客户机，例如 PuTTy。

建议禁止通过 SSH 访问公共 IP，而只允许通过 SSH 访问专用 IP。要连接专用 IP，需要客户机连接到专用网络。您可以使用客户门户网站中提供的缺省 VPN 选项（PPTP、SSL-VPN 和 IPsec）或使用 VRA 上配置的定制 VPN 解决方案来登录。

使用**设备详细信息**页面中的 Vyatta 帐户通过 SSH 登录。此外，还提供有 root 用户密码，但出于安全原因，缺省情况下禁用了 root 用户登录。

`ssh vyatta@[IP address] `

建议使 root 用户登录保持禁用状态。请使用 Vyatta 帐户登录，并仅在需要时升级到 root 用户。
{: tip}

还可以在部署期间提供 SSH 密钥，这样无需 Vyatta 帐户即可登录。验证能否使用 SSH 密钥访问 VRA 后，可以通过运行以下命令来禁用标准用户/密码登录：

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## 使用 Web GUI 访问设备
{: #accessing-the-device-using-the-web-gui}

使用以上 SSH 指令登录到 VRA，然后运行以下命令来启用 HTTPS 服务：

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

完成这些命令后，在浏览器的地址栏中输入 `https://<ip.address>`，将 IP 地址替换为 VRA 的 IP 地址。系统可能会要求您接受 VRA 的自签发证书。请接受，然后在系统提示时使用您的 Vyatta 凭证登录到 Web 界面。

## 方式
{: #modes-1}

**配置方式：**此方式是使用 `configure` 命令调用的，在此方式下，将执行 VRA 系统的配置。

**操作方式：**登录到 VRA 系统时的初始方式。在此方式下，可以运行 `show` 命令来查询有关系统状态的信息。还可以在此方式下重新启动系统。

VRA 的 shell 是一个模态界面，有多种操作方式。主/缺省方式是**操作**，这将是登录时显示的方式。在**操作**方式下，可以查看信息并发出影响系统当前运行的命令，例如设置日期/时间或重新引导设备。

`configure` 命令会将用户置于**配置**方式，在此方式下，可以执行对配置的编辑。请注意，配置更改不会立即生效；必须落实并保存更改。命令输入后，会进入配置缓冲区。输入了所有必需的命令后，运行 `commit` 命令以使更改生效。

要永久保存命令，请在运行 `commit` 命令后，运行 `save` 命令。

可以通过启动带 `run` 的命令，在配置方式下运行操作方式命令。例如：


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

散列标记 (`#`) 指示配置方式。以 `run` 开头的命令向 VRA shell 指示显示的是操作命令。先前的示例还说明针对命令的输出执行“grep”的能力。

## 命令浏览表
{: #command-exploration}

VRA 命令 shell 包含通过 Tab 键补全的功能。如果您有兴趣了解可用的命令，请按 Tab 键以获取命令列表和简短说明。此功能在 shell 提示符下和输入命令期间都有效。例如：

```
$show log dns [按 Tab 键]
可能的补全：
  dynamic    显示动态 DNS 的日志
  forwarding 显示 DNS 转发的日志
```

## 样本配置
{: #sample-configuration}

配置以分层节点模式进行布置。请考虑以下静态路由块：

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

这将由以下命令生成：

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

此示例说明一个节点（静态）可以有多个子节点。要除去 `192.168.1.0/24` 的路由，将使用 `delete protocols static route 192.168.1.0/24` 命令。如果在命令中遗漏了 `192.168.1.0/24`，那么两个路由节点都将标记为删除。

请记住，在发出 `commit` 命令之前，不会实际更改配置。要将当前正在运行的配置与配置缓冲区中存在的任何更改进行比较，请使用 `compare` 命令。要清空配置缓冲区，请使用 `discard`。

## 用户和基于角色的访问控制 (RBAC)
{: #users-and-role-based-access-control-rbac-}

可以为用户帐户配置三个级别的访问权：

* 管理员
* 操作员
* 超级用户

操作员级别的用户可以运行 `show` 命令来查看系统的运行状态，并可发出 `reset` 命令来重新启动设备上的服务。操作员级别的许可权并不意味着只读访问权。

管理员级别的用户具有对设备的所有配置和操作的完全访问权。管理员用户可以查看正在运行的配置，更改设备的配置设置，重新引导设备以及关闭设备。

超级用户级别的用户除了具有管理员级别的特权外，还能够使用 root 用户特权通过 `sudo` 命令来执行命令。

可以配置用户使用密码和/或公用密钥认证样式。公用密钥认证与 SSH 一起使用，并允许用户使用自己系统上的密钥文件登录。要创建使用密码的操作员用户，请执行以下操作：

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

未指定任何级别时，用户被视为管理员级别。在这种情况下，用户密码在配置文件中会显示为已加密。
{: note}

基于角色的访问控制 (RBAC) 是针对授权用户限制部分配置的访问权的一种方法。RBAC 支持管理员为用户组定义规则，以限制用户组可以运行的命令。

执行 RBAC 的方法是创建分配给访问控制管理 (ACM) 规则集的组，将用户添加到该组，创建规则集以将该组与系统中的路径相匹配，然后配置系统以允许或拒绝应用于该组的路径。

缺省情况下，在虚拟路由器设备中未定义 ACM 规则集，并且 ACM 已禁用。如果要使用 RBAC 来提供细粒度访问控制，那么除了您自己定义的规则外，还必须启用 ACM 并添加以下缺省 ACM 规则：

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

在尝试启用 ACM 规则之前，请参阅[补充文档](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation)。不准确的 ACM 规则设置会导致设备访问拒绝或系统不可操作错误。

---

copyright:
  years: 2017
lastupdated: "2017-12-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 同步高可用性配置
采用高可用性 (HA) 对形式的两个虚拟路由器设备 (VRA) 的配置必须已充分同步，这样这两个设备才会表现出类似的行为。这将通过 `configuration sync-map` 来完成，并且您可以选择将同步配置的哪个部分。如果在一台机器上进行了更改，那么会将标记的配置推送到其他设备。

**注：**这会在远程设备上同步并保存本地设备的有效配置。但是，作为落实过程的一个步骤，这样做不会在本地设备上保存配置。 

对一个系统唯一的配置不应同步到另一个系统。例如，真实 IP 地址和 MAC 不应同步。`system config-sync` 配置节点本身和 `service` 节点完全无法同步。

以下示例说明 config-sync：

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

前两行创建实际的 sync-map 本身。在此，`security firewall` 的配置节将在 `sync-map` 中进行设置。因此，在 config 节点中进行的任何更改都将推送到远程设备。但是，不会发送对 `security user` 进行的更改，因为这些更改与规则不匹配。可以根据需要将 `sync-map` 设置为特定或常规。

接下来的三行指定远程路由器的 `config-sync` 用户和密码、IP 以及要推送的 sync-map。与 `TEST` 的规则相匹配的任何更改都将使用此登录信息转至 `remote-router 192.168.1.22`。请注意，发出了 `REST` 调用以使用 VRA API 来执行此操作，因此 HTTPS 服务器必须正在远程路由器上运行（并且已取消阻止）。

每当落实更改时都会发生 config-sync。因此请监视来自远程设备的错误消息。如果配置不同步，那么需要在远程设备上对其进行修复，以使其恢复正常运行。

您还可以使用 `show config-sync difference` 命令来查看配置差异。

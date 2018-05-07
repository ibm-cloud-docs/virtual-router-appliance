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

# 升级操作系统
可通过对支持团队上传的本地 ISO 文件运行 ``add system image`` 命令，从而升级 VRA 操作系统。可用 Vyatta 升级版本的列表可以使用 IBM 支持凭单系统来获取。

要开始升级过程，请在 IBM 支持凭单系统上开具凭单以请求升级并将新的 ISO 映像上传到您的系统。您将收到来自 IBM 支持人员的电子邮件，其中指示 ISO 文件的上传位置。在以下示例中，此文件位于目录 ``tmp`` 中。

**注：**下面说明的升级过程适用于单个 VRA。如果是在高可用性方式下使用 VRA，那么必须在两个系统上运行相同的升级命令。此外，建议您首先升级 `BACKUP` 机器，并验证它是否能正常运行。然后访问 `MASTER` 机器，并使用 `reset vrrpp` 命令对其执行故障转移。最后，在 `BACKUP` 获得控制权后，升级原始 `MASTER`。

要升级 VRA，请执行以下过程。

1. 运行命令 ``add system image <Local ISO File>``.
2. 点击 **Enter** 键以接受 ISO 映像的缺省名称，或者自行输入名称。
3. 选择是否保存当前配置目录和配置文件。
4. 选择是否保存当前配置中的 SSH 主机密钥。
5. 点击 **Enter** 键以接受缺省系统控制台，或者自行输入控制台。
6. 点击 **Enter** 键以接受缺省控制台速度，或者自行输入速度。
7. 输入 `reboot` 命令，然后输入 `Yes` 以重新引导机器。
8. 如果是在高可用性方式下使用 VRA，请在第二台机器上重复运行步骤 1 到步骤 7。

以下示例说明升级过程。

```
vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S3

vyatta@vra01:~$ add system image /tmp/vyatta-vrouter-5.2R5S4_amd64.iso
Welcome to the Brocade vRouter image installer.
Checking MD5 checksums of files on the ISO image...OK.
What would you like to name this image? [5.2R5S4.08091424]: 5.2R5S4
This image will be named: 5.2R5S4
Would you like to save the current configuration
directory and config file? (Yes/No) [Yes]:
Saving current configuration...
Would you like to save the SSH host keys from your
current configuration? (Yes/No) [Yes]:
Saving SSH keys...
Copying squashfs image...
Copying kernel and initrd images...
Copying saved configuration to config partition.
Copying saved SSH host keys.
Enter the desired system console [tty0]:
Enter the console speed [38400]:
Running post-install script...
Done.

vyatta@vra01:~$ show system image
The system currently has the following image(s) installed:

   1: 5.2R5S3.06301309 (running image)
   2: 5.2R5S4 (default boot)

vyatta@vra01:~$ reboot
Proceed with reboot? (Yes/No) [No] y
The system is going down for reboot NOW!

vyatta@vra01:~$ show version | grep Version
Version:      5.2R5S4
```

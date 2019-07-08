---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: upgrade, os

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# 升級 OS
{: #upgrading-the-os}

可以在我們的支援團隊所上傳的本端 ISO 檔上，使用指令 ``add system image`` 來執行 VRA 作業系統升級。可用的 Vyatta 升級版本清單可以利用「IBM© 支援問題單」系統來取得。

若要開始升級程序，請使用「IBM 支援問題單」系統來開立問題單，並要求升級以及將新的 ISO 映像檔上傳至您的系統。您會收到「IBM 支援中心」的電子郵件，指出上傳 ISO 檔案的位置。在下列範例中，它位於 ``tmp`` 目錄。

以下所說明的升級程序適用於單一 VRA。如果您在高可用性模式下使用 VRA，則必須在兩個系統上都執行相同的升級指令。此外，建議先升級 `BACKUP` 機器，並驗證其是否正常運作。然後存取 `MASTER` 機器，並使用 `reset vrrp` 指令進行失效接手。最後，在 `BACKUP` 取得控制權之後，升級原始的 `MASTER`。
{: important}

若要升級 VRA，請執行下列程序。

1. 執行指令 ``add system image <Local ISO File>``。
2. 按 **Enter** 鍵接受 ISO 映像檔的預設名稱，或自行輸入。
3. 選擇是否要儲存現行配置目錄及配置檔。
4. 選擇是否要從現行配置儲存 SSH 主機金鑰。
5. 按 **Enter** 鍵接受預設系統主控台，或自行輸入。
6. 按 **Enter** 鍵接受預設主控台速度，或自行輸入。
7. 輸入指令 `reboot`，然後輸入 `Yes` 重新開機。
8. 如果您在高可用性模式下使用 VRA，請在第二部機器上重新執行步驟 1 到步驟 7。

下列範例說明升級程序。

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

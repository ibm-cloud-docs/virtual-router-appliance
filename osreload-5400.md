---

copyright:
  years: 2017
lastupdated: "2019-11-14"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# Reloading the OS
{: #reloading-the-os}

The OS reload process, when initiated in the Cloud Catalog, will wipe away all configurations present on the device and restore it to its original configuration. Any changes made since the last load of the system will be erased. As a result, if you have made changes (for example to the VRRP group on another machine), the reloaded machine will likely have a conflict when it comes back online.
{: shortdesc}

To reload your OS, perform the following procedure:

1. From your browser, open [https://cloud.ibm.com ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com){:new_window} and log into your account.
2. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the top left, then click **Classic Infrastructure**.
3. Choose **Network > Gateway Appliances**.
4. Click on the server you want to reload.
5. Click the server name in the Hardware section.
4. Select **OS Reload** from the **Actions** dropdown menu on the top right of the page.
5. In the OS Reload screen, click **Edit** for the Category that requires an update. Select **AT&T** as the Vendor, and the OS version you wish to reload.
6. Click the **Reload Above Configuration** button to proceed to the **Review** pop-up. Click **Cancel** to cancel the changes to the device and exit the screen.
7. Verify that all details in the New Configuration section are correct. Click **Next** to advance to the Confirm pop-up.
8. Click the **Confirm OS Reload** button to confirm and initiate the OS Reload. Click **Cancel** to cancel the action.

After initiating the OS Reload process, the device will be taken offline and the OS Reload process will begin. The time it takes an OS reload to complete varies based on the current and new configuration of the device. Throughout the configuration process, the minimum time for the OS Reload displays on each screen. The timeframe is an estimate made by the system. If the reload takes longer than 24 hours, please contact our Support team. When the device returns online, it will function as specified in the New Configuration for the OS Reload.

All data previously saved to the device will be lost, but can be restored if a backup was made of the device prior to its reload. If data was not backed up, it cannot be retrieved.
{: note}

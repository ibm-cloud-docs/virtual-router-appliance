---

copyright:
  years: 2017
lastupdated: "2017-10-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Reload OS
The OS reload process, when initiated in the Customer Portal, will wipe away all configurations present on the device and restore it to its original configuration. Any changes made since the last load of the system will be erased. As a result, if you have made changes, for example to the VRRP group on another machine, the reloaded machine will likely have a conflict when it comes back online.

To reload your OS, perform the following procedure:

1. Log in to the [Customer Portal ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){: new_window} using your unique credentials.
2. Select **Device List** from the Devices dropdown list.
3. Click on the server you want to reload.
4. Select **OS Reload** from the **Actions** dropdown menu on the top left of the page.
5. In the OS Reload screen, choose any features and options you would like to apply as part of the reload. 
6. Click the **Reload Above Configuration** button to proceed to the **Review** pop-up. Click **Cancel** to cancel the changes to the device and exit the screen.
7. Verify that all details in the New Configuration section are correct. Click **Next** to advance to the Confirm pop-up.
8. Click the **Confirm OS Reload** button to confirm and initiate the OS Reload. Click **Cancel** to cancel the action.

After initiating the OS Reload process, the device will be taken offline and the OS Reload process will begin. The time it takes an OS reload to complete varies based on the current and new configuration of the device. Throughout the configuration process, the minimum time for the OS Reload displays on each screen. The timeframe is an estimate made by the system. If the reload takes longer than 24 hours, please contact our Support team. When the device returns online, it will function as specified in the New Configuration for the OS Reload. 

**NOTE:** All data previously saved to the device will be lost, but can be restored if a backup was made of the device prior to its reload. If data was not backed up, it cannot be retrieved.

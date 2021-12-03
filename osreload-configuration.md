---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-14"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Reloading the OS
{: #reloading-the-os}
{: help}
{: support}

The OS reload process, when initiated in the IBM Cloud catalog, wipes away all configurations present on the device and restores  it to its original configuration. Any changes made since the last load of the system are erased. As a result, if you have made changes (for example to the VRRP group on another machine), the reloaded machine might have a conflict when it comes back online.
{: shortdesc}

To reload your OS, follow these steps:

1. From your browser, open [https://cloud.ibm.com](https://cloud.ibm.com){: external} and log in to your account.
2. Select the Menu icon ![Menu icon](../../icons/icon_hamburger.svg) from the upper left of the page, then click **Classic Infrastructure**.
3. Choose **Network > Gateway Appliances**.
4. Click the server you want to reload.
5. Click the server name in the Hardware section.
6. Select **OS Reload** from the **Actions** menu on the upper right of the page.
7. On the OS Reload page, click **Edit** for the Category that requires an update. Select **AT&T** as the Vendor, and the OS version that you want to reload.
8. Click the **Reload Above Configuration** button to proceed to the Review window. Click **Cancel** to cancel the changes to the device and exit the page.
9. Verify that all details in the New Configuration section are correct. Click **Next** to advance to the Confirm window.
10. Click the **Confirm OS Reload** button to confirm and initiate the OS Reload. Click **Cancel** to cancel the action.

After initiating the OS Reload process, the device is taken offline and the OS Reload process begins. The time it takes an OS reload to complete varies based on the current and new configuration of the device. Throughout the configuration process, the minimum time for the OS Reload displays on each page. The timeframe is an estimate made by the system. If the reload takes longer than 24 hours, contact IBM Support. When the device returns online, it functions as specified in the New Configuration for the OS Reload.

All data previously saved to the device is lost. You can restore your data if you made a backup of the device prior to its reload. If you did not back up your data, it cannot be retrieved.
{: note}

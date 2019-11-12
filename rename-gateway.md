---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: gateway, rename

subcollection: virtual-router-appliance

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

# Renaming a VRA
{: #renaming-a-vra}

Network Gateways are given unique names that assist users in their identification. At any time, a Gateway name may be changed. It is recommended that you use a consistent naming convention to more easily identify Gateways.

Perform the following procedure to rename a Network Gateway:

1. [Access the Gateway Appliance Details screen](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details) in the IBM Cloud console.
2. Click the **Actions** dropdown menu and select **Rename Gateway**.
3. Enter the new Gateway Name in the **Gateway Name** field.
4. Click **OK** to save the change.

After changing a Gateway Appliance's name, the name will immediately change at the top of the Gateway Appliance Details screen. The Gateway name may be changed again at any time by repeating the steps above.

Changing the name of the VRA in the console does not automatically change the hostname within the {{site.data.keyword.vra_full}} or any DNS entries you may have. This will require manual intervention if needed.
{: note}

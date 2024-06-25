---
copyright:
  years: 1994, 2019
lastupdated: "2019-11-14"

keywords: nat, setup, 5400

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Configuring Vyatta 5400
{: #basic-configuration-of-vyatta-5400}

You can configure your Vyatta 5400 using the procedure here.
{: shortdesc}

You must configure the public virtual LAN (VLAN) 1224, which is now associated and routed, before it can pass traffic. Two pieces of information are needed to complete the configuration:

* The public-side bond of the Brocade 5400 vRouter appliance
* The default gateway of 1224

It is the public-side VLAN where the compute option is located, not the Brocade 5400 vRouter appliance's public VLAN.
{: note}

## In the SoftLayer portal
{: #in-the-softlayer-portal}

Follow these steps:

1. Within the SoftLayer web portal, in **Devices**, locate the bond on the **Configuration** tab for the Brocade 5400 vRouter device. Assume that `eth1=bond1` on your device.

2. Select **Network > IP Management > VLANs** from the web portal to find the default gateway for VLAN 1224.

3. Click your VLAN in the list, then click the **Subnet** where you see your gateway. Make a note of the mask Classless Inter-Domain Routing (CIDR) details located after the slash.

## Configuring the private VLAN using the CLI
{: #configure-the-private-vlan-using-the-cli}

There are two command modes in the command-line interface (CLI): operational and configuration.

Operational mode provides access to operational commands for:

* Displaying and clearing information
* Enabling and disabling debugging
* Configuring terminal settings
* Loading and saving configuration
* Restarting the system

Configuration provides access to commands for:

* Creating
* Modifying
* Deleting
* Committing
* Showing configuration information
* Navigating through the configuration hierarchy 

When you log on to the system, the system is in operational mode. You must switch to configuration mode for the commands.

You can tell which mode that you are in based on the prompt. You are in operational mode if the prompt is #, and configuration mode if the prompt is $.
{: note}

Follow these steps to configure the private VLAN by using the CLI. Remember the values needed to configure the VLAN are:

* VLAN name of the VLAN to be routed through Brocade 5400 vRouter device (`2254`)
* Gateway and mask (CIDR format) of the VLAN to be routed (`10.52.69.201/29`)
* Private bond name of the Brocade 5400 vRouter Device (**bond0**)

1. SSH into your Brocade 5400 vRouter (public or private IP address) by using **vyatta** as the **Username**. Supply the password when prompted.

   You should create a new user within Brocade 5400 vRouter and disable the default initial user **vyatta**.
   {: note}

2. Configure the vif:

   * Type *configure* at the command prompt to enter configuration mode.
   * Type *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* at the command prompt to set the vif.
   * Type *commit* at the command prompt to commit the settings.
   * Type *save* to save the settings.
   * Type *exit* to switch back to operation mode.

3. Type *show interfaces* to check the settings that you committed.

4. Route any remaining VLANs through the Brocade 5400 vRouter device.

---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Basic Configuration of Vyatta 5400

Perform the following procedure to configure your Vyatta 5400.

The public virtual LAN (VLAN) 1224, which is now associated and routed, needs to be configured before it can pass traffic. Two pieces of information are needed to complete the configuration:

  * The public-side bond of the Brocade 5400 vRouter appliance
  * The default gateway of 1224

Note that it is the public side VLAN where the compute option is located - not the Brocade 5400 vRouter appliance's public VLAN.

## In the Softlayer Portal

1\. Within the SoftLayer Web Portal, under **Devices**, locate the bond on the **Configuration** tab for the Brocade 5400 vRouter device. Assume that <span style="text-decoration: underline">eth1=bond1</span> on your device.

2\. Select **Network > IP Management > VLANs** from the Web Portal to find the default gateway for VLAN 1224.

3\. Click your VLAN in the list, and click the **Subnet** where you see your Gateway. Make a note of the mask Classless Inter-Domain Routing (CIDR) details located after the slash. 

## In the Vyatta GUI

4\. From the dashboard, click the **Configuration** tab.

5\. Expand **Interfaces > Bonding > bond1** on the left side of the screen; highlight **vif**.

6\. Enter the VLANs "name" in the **vif** field (1224 for our purposes) and click the **Create** button. The process will take a few seconds to complete.

7\. Select the newly created **vif 1224** under the **vif** twisty.

8\. Check the box under **dhcp** and type the default gateway IP address and the mask CIDR notation of the VLAN you want to pass through the Brocade 5400 vRouter (for example, VLAN 1224).

9\. Click the **Set** button and click **Commit**.

10\. Click **Save** in the middle menu bar otherwise the configuration rolls back to its' default the next time the system is rebooted.

**NOTE:** The configuration rollback can be a very useful feature if you break your configuration while testing changes. As long as the changes are not saved you can reboot the server from the Web Portal and restore your previous configuration.

11\. Click on the Statistics tab and open the new interface to verify and monitor traffic.

## Configure the private VLAN using the CLI

There are two command modes in the CLI (Command Line Interface): operational and configuration. 

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
  * Navigating through the configuraiton heirarchy

When you log onto the system, the system is in operational mode; you will need to switch to configuration for the commands.

**NOTE:** You can tell which mode you are in, operational or configuration, based on the prompt. You are in operational mode if the prompt is #, and configuration mode if the prompt is $.

Use the following steps to configure the private VLAN using the CLI. Remember the values needed to configure the VLAN are:

  * VLAN name of the VLAN to be routed through Brocade 5400 vRouter device (2254)
  * Gateway and mask (CIDR format) of the VLAN to be routed (10.52.69.201/29)
  * Private bond name of the Brocade 5400 vRouter Device (bond0)

1. SSH into your Brocade 5400 vRouter (public or private IP address) using **vyatta** as the **Username**; supply the password when prompted.

   **NOTE:** You should create a new user within Brocade 5400 vRouter and disable the default initial user `vyatta`.

2. Configure the vif:

  * Type *configure* at the command prompt to enter configuration mode.
  * Type *set interfaces bonding bond0 vif 2254 address 10.52.69.201/29* at the command prompt to set the vif.
  * Type *commit* at the command prompt to commit the settings.
  * Type *save* to save the settings.
  * Type *exit* to switch back to operation mode.

3\. Type *show interfaces* to check the settings you just committed.

4\. Route any remaining VLANs through the Brocade 5400 vRouter device.
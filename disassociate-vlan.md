---

copyright:
  years: 2017
lastupdated: "2017-10-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Disassociate a VLAN from a Gateway Appliance

VLANs may be linked to one Gateway Appliance at a time through [association](associate-vlan.html). Association allows the VLAN to be routed to the Gateway Appliance at any time. If a VLAN should be associated to another Gateway Appliance, or if the VLAN should no longer be associated to its Gateway, disassociation is required. Disassociation removes the "link" from the VLAN to the Gateway Appliance, allowing it to be associated to another Gateway, if necessary. 

Perform the following procedure to disassociate a VLAN from a Gateway Appliance:

1. Access the [Gateway Details screen](access-gateway-screen.html) in the Customer Portal. 
2. Locate the desired VLAN in the Associated VLANs section.
3. Select **Disassociate** from the Actions drop down menu. 
4. Click **Yes** to disassociate the VLAN. Click **No** to cancel the action.

After disassociating a VLAN from a Gateway Appliance, the VLAN may be associated to another Gateway. The VLAN may also be associated back to the Gateway Appliance at any time. After disassociating a VLAN from a Gateway Appliance, the VLAN's traffic cannot be routed through the Gateway. VLANs must be associated to a Gateway Appliance in order to utilize Gateway routing.
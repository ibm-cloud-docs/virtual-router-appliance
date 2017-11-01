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

# Route an Associated VLAN

Associated VLANs are linked to a Gateway Appliance, but traffic in and out of the VLAN does not hit the Gateway until the VLAN has been routed. After an associated VLAN has been routed, all front and back-end traffic is routed through the Gateway Appliance as opposed to customer routers. 

Perform the following procedure to route an associated VLAN.

1. Access the [Gateway Appliance Details screen](access-gateway-details.html) in the Customer Portal. 
2. Associate the VLAN to the Gateway Appliance, if the desired VLAN is not already associated. Refer to [Associate a VLAN to a Gateway Appliance](associate-vlan.html).
3. Locate the desired VLAN in the Associated VLANs section.
4. Select **Route VLAN** from the Actions drop down menu.
5. Click **Yes** to route the VLAN. Click **No** to cancel the action.

After routing a VLAN, all front and back-end traffic moves from the customer routers to the Network Gateway. Additional controls related to traffic and the Gateway Appliance itself may be taken by accessing the Gateway's management tool. Routing through the Network Gateway may be discontinued at any time by [bypassing the Gateway Appliance](bypass-gateway.html).
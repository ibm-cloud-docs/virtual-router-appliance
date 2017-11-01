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

# Bypass Gateway Appliance Routing for a VLAN

After a VLAN has been routed, all front and back-end traffic travels through the Network Gateway. At any time, the Gateway Appliance may be bypassed so that traffic will return to the front and back-end customer routers (FCR and BCR). 

Bypassing a VLAN allows the VLAN to remain associated to the Network Gateway. If the VLAN should no longer be associated with the Gateway Appliance, refer to [Disassociate a VLAN from a Network Gateway](disassociate-vlan.html). 

Perform the following procedure to bypass Gateway routing for a VLAN:

1. Access the [Gateway Appliance Details screen](access-gateway-screen.html) in the Customer Portal. 
2. Locate the desired VLAN in the Associated VLANs section.
3. Select **Bypass VLAN** from the Actions drop down menu.
4. Click **Yes** to bypass the Gateway. Click **No** to cancel the action.

After bypassing the Network Gateway, all front and back-end traffic routes through the FCR and BCR associated with the VLAN. The VLAN will remain associated with the Gateway Appliance and may be routed back to the Gateway Appliance at any time.
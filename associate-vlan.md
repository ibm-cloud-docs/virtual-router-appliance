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

# Associate a VLAN to a Gateway Appliance

VLAN association is the linking of an eligible VLAN to a Network Gateway so that it may be routed to the Gateway Appliance in the future. The process of association does not automatically route a VLAN to a Gateway Appliance; the VLAN continues to use front-end and back-end customer routers until it is manually routed to the Gateway. 

VLANs may be associated to only one Gateway at a time and must not have a firewall. Perform the following procedure to associate a VLAN to a Network Gateway.

1. Access the [Gateway Appliance Details screen](access-gateway-screen) in the Customer Portal. 
2. Select the desired VLAN from the **Associate a VLAN** drop down list.
3. Click the **Associate** button to associate the VLAN.

After associating a VLAN to the Gateway Appliance, it appears in the Associated VLANS section of the Gateway Appliance Details screen. From this section, the VLAN may be routed to the Gateway or may be disassociated from the Gateway. Additional eligible VLANs may be associated to a Gateway Appliance at any time by repeating the steps above.
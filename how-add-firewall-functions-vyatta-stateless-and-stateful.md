---

copyright:
  years: 2024
lastupdated: "2024-08-02"

keywords:  

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Adding firewall functions to Vyatta 5400 (stateless and stateful)
{: #adding-firewall-functions-to-vyatta-5400-stateless-and-stateful-}

Applying firewall rulesets to each interface is one firewall method when you use Brocade 5400 vRouter (Vyatta) devices. Each interface has three possible firewall instances - In, Out, and Local - and each instance has rules that can be applied to it. The default action is Drop, with rules that allow specific traffic being applied in a fashion from rule 1 to N. As soon as a match is made, the firewall applies the specific action of the matching rule.
{: shortdesc}

For any of the following firewall instances, **only one** can be applied.

* **IN** - The firewall filters packets that enter the interface and traverse the Brocade system. You must permit certain SoftLayer IP ranges to have access to the front-end and back-end for management (ping, monitoring, and so on).
* **OUT** - The firewall filters packets leaving the interface. These packets can be traversing the Brocade system or originating on the system.
* **LOCAL** - The firewall filters packets that are destined for the Brocade vRouter system through the system interface. You should establish restrictions on access ports coming into the Brocade vRouter device from external IP addresses that are not restricted.

Follow these steps to set an example firewall rule to turn off Internet Control Message Protocol (ICMP) *(ping - IPv4 echo reply message)* to your Brocade 5400 vRouter's interfaces (this is a stateless action; a stateful action is reviewed later):

1. Type *show configuration* commands in the command prompt to see which configurations are set. You see a list of all the commands that you set on your device (which can be handy if you decide to migrate and want to see all your configurations). Notice the command *set firewall all-ping 'enable'*, which indicates that ICMP is still enabled for your device.

2. Type *configure*.

3. Type *set firewall all-ping 'disable'*.

4. Type *commit*.

If you now try to ping your Brocade 5400 vRouter device, no response is received.

To assign firewall rules to routed traffic, you must apply rules to the Brocade 5400 vRouter's **interfaces** or **create zones**, then apply to the zones.

For this example, zones are created for the VLANs that were used thus far.

bond1 = dmz

bond1.2007 = prod (for production)

bond0.2254 = private (for development)

bond1.1280 = reserved for future use

bond1.1894 = reserved for future use

## Creating firewall rules
{: #create-firewall-rules}

Before the zones are created, it is a good idea to create the firewall rules that are to be applied to the zones. Creating the rules before the zones allows you to apply them immediately, versus creating the zone, then creating the rules, and having to go back to the zone for rule application.

**NOTE:** Zones and rulesets both have a default action statement. When using Zone-Policies, the default action is set by the zone-policy statement and is represented by rule 10,000. It is also important to remember that the default action of a firewall ruleset is to **drop** all traffic.

The following commands:

* Create a firewall rule named **dmz2private** with the default action to drop any packet.
* Enable logging of accepted and denied traffic for the rule named **dmz2private**.

1. Type *configure* in the command prompt.

2. Enter the following commands in the prompt:

   * *set firewall name dmz2private default-action drop*
   * *set firewall name dmz2private enable-default-log*

   The next set of commands is to enable **iptables** to allow traffic for an established session to return. By default, **iptables** does not    allow this, which is why an explicit rule is required.
   
      * *set firewall name dmz2private rule 1 action accept*
      * *set firewall name dmz2private rule 1 state established enable*
      * *set firewall name dmz2private rule 1 state related enable*
   
   The third set of commands allows TCP/UDP to get through port 22, the default for SSH.
   
      * *set firewall name dmz2private rule 2 action accept*
      * *set firewall name dmz2private rule 2 protocol tcp_udp*
      * *set firewall name dmz2private rule 2 destination port 22*
   
   The final set of commands allows TCP/UDP to get through port 80, the default for HTTP.
   
      * *set firewall name dmz2private rule 3 action accept*
      * *set firewall name dmz2private rule 3 protocol tcp_udp*
      * *set firewall name dmz2private rule 3 destination port 80*

3. Type *commit* to ensure that all rules are taken when finished.

4. View your configuration by typing *show firewall name dmz2private* in the command prompt.

The next firewall rule is applied to our **dmz** zone. This rule is named **public**. Enter the following commands:

* *set firewall name public default-action drop*
* *set firewall name public enable-default-log*
* *set firewall name public rule 1 action accept*
* *set firewall name public rule 1 state established enable*
* *set firewall name public rule 1 state related enable*

Firewall rules need to flow outbound through **prod** to **dmz**. Use the following command to help troubleshoot network flow: *sudo tcpdump -i any host 10.52.69.202*.
{: note}

## Creating zones
{: #create-zones}

Zones are logical representation of an interface. The following commands:

* Create a zone and a policy named **dmz** with a default action to drop packets that are destined for this zone.
* Set the **dmz** policy to use the **bond1** interface.
* Set the **prod** policy to use the **bond1.2007** interface.
* Create a zone policy named **private** with a default action to drop packets that are destined for this zone.
* Set the policy named **private** to use the **bond0.2254** interface.

1. Enter the Following commands in the prompt:

   * *configure*
   * *set zone policy zone dmz default-action drop*
   * *set zone-policy zone dmz interface bond1*
   * *set zone-policy zone prod default-action drop*
   * *set zone-policy zone prod interface bond1.2007*
   * *set zone-policy zone private default-action drop*
   * *set zone-policy zone private interface bond0.2254*

2. Use the following commands to set the firewall policy for the zones:

   * *set zone-policy zone private from dmz firewall name dmz2private*
   * *set zone-policy zone prod from dmz firewall name dmz2private*
   * *set zone-policy zone dmz from prod firewall name public4*
   * *commit*

Note that you can apply a firewall rule to a specific interface if you do not want to apply it to a zone policy. Use the following commands to apply a rule to an interface.

* *set interfaces bonding bond1 firewall local name public*
* *commit*

## Stateful firewalls
{: #stateful-firewalls}

A stateful firewall keeps a table of previously seen flows, and packets can be accepted or dropped according to their relation with previous packets. Generally, stateful firewalls are preferred where application traffic is prevalent.

The Brocade 5400 vRouter does not track the state of the connections with default configuration. The firewall is stateless until one of the following conditions is met:

* Configuration of a firewall and any rule has a state parameter set
* Configuration of a firewall state-policy
* Configuration of NAT
* Enablement of the transparent web proxy service
* Enablement of a WAN load-balancing configuration

## Stateless firewalls
{: #stateless-firewalls}

A stateless firewall considers every packet in isolation. Packets can be accepted or dropped according to only basic access control list (ACL) criteria, such as the source and destination fields in the IP or Transmission Control Protocols/User Datagram Protocol (TCP/UDP) headers. A stateless Brocade 5400 vRouter does not store connection information and has no requirement to look up every packet's relation to previous flows, both of which consume small amounts of memory and CPU time. Therefore, raw forwarding performance is best on a stateless system. Brocade recommends keeping the router stateless for best performance if you do not require the features specific to statefulness.

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

# Setup NAT Rules on Vyatta
This topic contains examples of the Network Addres Translation (NAT) rules used on a Vyatta.

## One-to-many NAT rule (masquerade)

Enter the following commands in the prompt:

~~~
set nat source rule 1000 description 'pass traffic to the internet'
set nat source rule 1000 outbound-interface 'bond1'
set nat source rule 1000 protocol 'tcp_udp'
set nat source rule 1000 source address '10.125.49.128/26'
set nat source rule 1000 translation address 'masquerade'
commit
~~~

Connection request from machines in the `10.xxx.xxx.xxx` network are mapped to the IP on bond1 and receive an associated ephemeral port when going outbound. An idea is to assign one-to-many masquerade rule numbers higher so that they do not conflict with lower NAT rules that you may have.

**NOTE:** You must configure the server to pass its internet traffic through the Brocade 5400 vRouter so that its default gateway is the Private IP address of the managed virtual LAN (VLAN). For example, for `bond0.2254` the gateway is `10.52.69.201`. This should be the gateway address for the server passing Internet traffic.

**NOTE:** Use the following command to help troubleshoot NAT: run show nat source translations detail.

## One-to-one NAT rule

The commands below show how to setup a one-to-one NAT rule. Notice the rule numbers are set up to be lower than the masquerade rule. This is so that the one-to-one rules will take precedence over the one-to-many rules.

**NOTE:** IP addresses that are mapped one-to-one cannot be masqueraded. If you translate an IP inbound, you must translate that IP outbound in order for traffic to go both ways.

The following commands are for a source and destination rule. Type `show nat` in configuration mode to see the NAT rule type.

**NOTE:** Use the following command to help troubleshoot NAT: `run show nat source translations detail`. 

Enter the following commands in the prompt after ensuring you are in configuration mode:

~~~
set nat source rule 9 outbound-interface 'bond1'
set nat source rule 9 protocol 'all'
set nat source rule 9 source address '10.52.69.202'
set nat source rule 9 translation address '50.97.203.227'
set nat destination rule 9 destination address '50.97.203.227'
set nat destination rule 9 inbound-interface 'bond1'
set nat destination rule 9 protocol 'all'
set nat destination rule 9 translation address '10.52.69.202'
commit
~~~

If traffic comes in on IP `50.97.203.227` on bond1, that IP will be mapped to IP `10.52.69.202` (on any interface defined). If traffic goes outbound with the IP of `10.52.69.202` (on any interface defined), it will get translated to IP `50.97.203.227` and proceed out bound on interface bond1.

**NOTE:** Use the following command to help troubleshoot NAT: `run show nat source translations detail`.

## Adding IP ranges through your Brocade 5400 vRouter (Vyatta)

Depending on your Brocade 5400 vRouter configuration, you may want to accept specific SoftLayer IP addresses. 

Newer Brocade 5400 vRouter deployments come with IBM Cloud's services network IP addresses defined in a firewall rule called `SERVICE-ALLOW`.

The following is an example of `SERVICE-ALLOW`. This is not a complete private IP rule set.

~~~
set firewall name SERVICE-ALLOW rule 1 action 'accept'
set firewall name SERVICE-ALLOW rule 1 destination address '10.0.64.0/19'
set firewall name SERVICE-ALLOW rule 2 action 'accept'
set firewall name SERVICE-ALLOW rule 2 destination address '10.1.128.0/19'
set firewall name SERVICE-ALLOW rule 3 action 'accept'
set firewall name SERVICE-ALLOW rule 3 destination address '10.0.86.0/24'
~~~

Once you have deinfed the firewall rules, you may now assign them as you see fit. Two examples are listed below. 

Applying to a zone:

`set zone-policy zone private from dmz firewall name SERVICE-ALLOW`

Applying to a bond interface:

`set interfaces bonding bond0 firewall local name SERVICE-ALLOW`

Notes:



2IP addresses that are mapped one-to-one cannot also be masqueraded. If you translate۝ an IP inbound, you must translate۝ that IP outbound in order for traffic to go both ways.

3Use the following command to help troubleshoot NAT: run show nat source translations detail.

4Use the following command to help troubleshoot NAT: run show nat source translations detail.

---

copyright:
  years: 2017
lastupdated: "2017-10-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# High Availability and VRRP
The Virtual Router Appliance (VRA) supports Virtual Router Redundancy Protocol (VRRP) as a high availability protocol. The deployment of devices is done in an active/passive manner, where one machine is master and the other is the backup. All interfaces on both machines will be a member of the same "sync-group", so if one interface experiences a fault, the other interfaces in the same group will also fault, and the device will stop being master. The current backup will detect that the master is no longer broadcasting keepalive/heartbeat messages, and assume control of the VRRP virtual IPs and become the master.

VRRP is the most important part of the configuration when provisioning Gateways. High availability functionality depends on the heartbeat messages, so making sure they are not blocked is critical.

## VRRP Virtual IP (VIP) Addresses
The VRRP virtual IP, or VIP, is the floating IP address changed from master to backup device when failover happens. When a VRA deploys, it will have a public and private bonded network connection and real IPs assigned on each interface. A VIP is assigned on both interfaces as well, whether or not the device is a standalone or in an HA pair. Traffic that has a destination IP in subnets in VLANs associated with the VRA will be sent directly to these VRRP VIPs.

VRRP virtual IP addresses for any gateway group should not be changed, nor should the VRRP interface be disabled. These IP addresses are the method by which traffic is routed to the gateway when a VLAN is associated. If the IP address is not present, then the traffic cannot be forwarded from softLayer BCR/FCR to the gateway itself.

## VRRP Group
A VRRP group consists of a cluster of interfaces or virtual interfaces that provide redundancy for a primary, or “master,” interface in the group. Each interface in the group is typically on a separate router. Redundancy is managed by the VRRP process on each system. The VRRP group has a unique numeric identifier and can be assigned up to 20 virtual IP addresses.  

The VRRP group ID is assigned by IBM Cloud, and should not be changed. When a new gateway group is provisioned behind a Front Customer Router (FCR) or Backend Customer Router (BCR) for the first time, it will receive a VRRP group of 1. Subsequent gateway group provisions will increment this value to prevent conflicts, the next group will have group 2, then group 3, and so on. It is then calculated and assigned by the provisioning process. Altering this value risks collision with other active groups, and then master/master contention, which will likely cause an outage on both gateway groups.

If you migrate from a previous configuration, it is recommended that you double check your configuration code to make sure the VRRP group ID is not statically assigned.

The VRRP group ID is persisted in the IBM Cloud database, so the same group ID value will be used during an OS reload or upgrade. Any modified VRRP group ID will be overwritten with the system assigned value during an OS reload.  

## VRRP Priority
The first machine in a gateway group will have a priority of 254, and this value is decremented for the next device provisioned. You should never set a priority to 255, as this defines the VIP "address owner" and can result in unintended behavior when the machine is brought online with a configuration that differs from the running active master.

## VRRP Preemption
Preemption should always be disabled on all VRRP interfaces, so that a new device (or one that is going through the reloading of its OS) does not try to take over the cluster.

## VRRP Advertise Interval
To signal that it is still in service, the master interface or VIF sends “heartbeat” packets called “advertisements” to the LAN segment, using the IANA assigned multicast addresses for the VRRP (`224.0.0.18` for IPv4 and `FF02:0:0:0:0:0:0:12` for IPv6).

By default, the interval is set as 1. This value can be increased, but it is not recommended you set a value greater than 5. On a busy network, it may take much longer than a second for all the VRRP notices to arrive on the backup device from the master, so a value of 2 should be used for high traffic networks.

## VRRP Synchronization (sync-group)
Interfaces in a VRRP synchronization group (“sync group”) are synchronized such that, if one of the interfaces in the group fails over to backup, all interfaces in the group fail over to backup. For example, in many cases, if one interface on a master router fails, the whole router fails over to a backup router.

This value is different than the VRRP group, as it defines what interfaces on a device will fail over when an interface in that group registers a fault. It is recommended that all interfaces belong to the same sync-group, otherwise, some interfaces will be master and have gateway IPs, and others will be backup, and traffic will not forward properly anymore. Active/active configurations are not supported.

## VRRP RFC-Compatibility
VRRP rfc-compatibility enables or disables RFC 3768 MAC addresses for VRRP on an interface. This should be enabled on the native interfaces, but not enabled on any configured VIFs. Some virtual switches (mostly vmware) have problems with this being enabled, and will cause traffic to be dropped and not sent to the gateway IP from the hypervisor host machine. Leave this setting alone, and do not configure the setting for any VIFs.

## VRRP Authentication
If a password is set for VRRP authentication, the authentication type must also be defined. If the password is set and the authentication type is not defined, the system will generate an error when you try to commit the configuration.

Similarly, you cannot delete the VRRP password without also deleting the VRRP authentication type. If you do, the system generates an error when you try to commit the configuration. If you delete both the VRRP authentication password and authentication type, VRRP authentication is disabled.

The IETF determined that authentication is not to be used for VRRP version 3. For more information, refer to RFC 5798 VRRP.

## Version 2/3 support
The VRA supports both VRRP version 2 (default) and version 3 protocols. In version 2, you cannot have IPv4 and IPv6 together. But in Version 3, you can have IPv4 and IPv6 at the same time.

## High Availability VPN with VRRP
The VRA provides the ability to maintain connectivity through one IPsec tunnel by using a pair of Virtual Router Appliances with VRRP. When one router fails or is brought down for maintenance, the new VRRP master router restores IPsec connectivity between the local and remote networks.

When configuring High Availability VPN with VRRP, whenever a VRRP virtual address is added to a VRA interface, you must reinitialize the IPsec daemon because the IPsec service listens only for connections to the addresses that are present on the VRA when the Internet Key Exchange (IKE) service daemon is initialized.

Run the following command on the master and backup routers, so that when failover happens, IPsec daemons will be restarted on a new master device after VIP transiting in, as shown in the following example:

```
vyatta@vrouter# set interfaces bonding dp0bond1 vrrp vrrp-group 1 notify ipsec
```

## High Availability Firewalls with VRRP
When two devices are in an HA pair, care must be taken on your master device that you do not block access from the other device. Port 443 must be allowed between both devices for config-sync to work, and VRRP must be allowed to be sent and received, including the multicast range of 224.0.0.0/24.

## Associated VLAN Subnets with VRRP
For any VIF configuration with VRRP you will need a virtual IP address (the first usable IP in the subnet, the gateway IP to which everything in that subnet will route) and also a real interface IP address for the VIF on both devices. To conserve usable IPs, it is recommended that the real interface IPs use an out of band range such as 192.168.0.0/30, where one device has the .1 and the other device has the .2 address. You can have multiple VRRP virtual IPs, but you only need one real address on each VIF.

## Connection Synchronization
When two VRA devices are in an HA pair, it may be useful to track stateful connections between the two devices, so that if a failover happens, the current state of all connections that are on the failed device are replicated to the backup device. This means it isn't necessary for any current active sessions (like SSL connections) to be rebuilt from scratch, which can result in a disrupted user experience.

This is called connection tracking synchronization.

To configure it, you must declare what the failover method is, which interface you will use to send the connection tracking information, and the IP of the remote peer:

```
set service connsync failover-mechanism vrrp sync-group SYNC1
set service connsync interface dp0bond0
set service connsync remote-peer 10.124.10.4
```

The other VRA will have the same configuration, but a different `remote-peer`.

Note that this can saturate a link if there is a high number of connections coming in on other interfaces, and will compete with other traffic on the declared link.
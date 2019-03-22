---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: ha, high availability, sync, synchronize, config-sync

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# Synchronizing High Availability Configurations
{: #synchronizing-high-availability-configurations}

Two Virtual Router Appliances (VRA) in a High Availability (HA) pair must have their configurations synchronized sufficiently so that both devices behave in a similar manner. This is done through `configuration sync-maps` and you can choose which portion of the configuration will be synchronized. If you make a change on one machine, it will push the marked config over to the other device.

This synchronizes and saves the running configuration of the local device on the remote device. However, as a step of the commit process it would not save the configuration on the local device.
{: note}

Configurations that are unique to one system should not be synced to the other. Real IP addresses and MACs should not be synchronized, for instance. The `system config-sync` configuration node itself and the `service https` node cannot be synchronized at all.

The following example illustrates config-sync:

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

The first two lines create the actual sync-map itself. Here, the configuration stanza for `security firewall` will be set in the `sync-map`. As a result, any changes made inside the config node will be pushed to the remote device. However, changes made to `security user` would not be sent, as that does not match the rule. You can make the `sync-map` as specific or as general as you want.

The next three lines designate the remote router's `config-sync` user and password, IP, and which sync-map to push. Any changes that match the rules for `TEST`, will go to the `remote-router 192.168.1.22`, using this login information. Note that a `REST` call is made to perform this using the VRA API, so the HTTPS server must be running (and unblocked) on the remote router.

Config-sync happens whenever you commit a change. Watch for error messages that come from the remote device. If the configuration is out of sync, you will need to fix it on the remote device to make it operational again.

You can also see configuration differences using the command `show config-sync difference`.

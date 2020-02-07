---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-07"

keywords: basics, vra, access, configure, gui

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}

# Accessing and configuring the IBM Cloud Virtual Router Appliance
{: #accessing-and-configuring-the-ibm-virtual-router-appliance}

The {{site.data.keyword.vra_full}} (VRA) can be configured using a remote console session through SSH or by logging into the web GUI. By default, the web GUI is not available from the public internet. To enable the web GUI, log in through SSH first.
{: shortdesc}

Configuring the VRA outside of its shell and interface may produce unexpected results and is not recommended.
{: note}

## Accessing the device using SSH
{: #accessing-the-device-using-ssh}

Most UNIX-based operating systems, such as Linux, BSD, and Mac OSX, have OpenSSH clients included with their default installations. Windows users can download an SSH client, such as PuTTy.

It is recommended that SSH to the public IP be disabled and only SSH to the private IP be allowed. Connections to private IPs requires your client to be connected private network. You can log in using one of the default VPN options (SSL and IPsec) offered in the IBM Cloud console, or using a custom VPN solution configured on the VRA.

Use the Vyatta account from the **Device Details** page to login through SSH. The root password is also provided, but root login is disabled by default for security reasons.

`ssh vyatta@[IP address] `

It is recommended to keep root logins disabled. Log in using the Vyatta account and elevate to root only when needed.
{: tip}

SSH keys can also be supplied during deployment to avoid needing the Vyatta account to log in. After you verify your ability to access your VRA using your SSH key, you can disable standard user/password logins by running the following commands:

```
$ configure
# set service ssh disable-password-authentication
# commit
```

## Accessing the device using the web GUI
{: #accessing-the-device-using-the-web-gui}

Log into the VRA using the SSH instructions above, then run the following commands to enable the HTTPS service:

```
$ configure
# set service https listen-address 169.45.21.2
# commit
```

After these commands complete, enter `https://<ip.address>` in your browser's address bar, replacing the IP address with your VRA's. You may be asked to accept the VRA's self-issued certificate. Do so, then log into the Web interface with your Vyatta credentials when prompted.

## Modes
{: #modes-1}

**Configuration mode:** Invoked with the use of the `configure` command, this mode is where configuration of the VRA system is performed.

**Operational mode:** The initial mode upon logging into a VRA system. In this mode, `show` commands can be run to query information on the state of the system. The system can also be restarted from this mode.

The VRA's shell is a modal interface, with several modes of operation. The primary/default mode is **Operational**, and this will be the mode presented upon login. In **Operational** mode, you can view information and issue commands which impact the current run of the system, such as setting the date/time or rebooting the device.

The command `configure` places the user into **Configuration** mode, where edits to the configuration can be performed. Note that configuration changes do not take place immediately; they must be committed. As commands are entered, they go into a configuration buffer. Once all the necessary commands are entered, run the `commit` command to make the changes take effect.

Operational mode commands can be run from Configuration mode by starting the command with `run`. For example:


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

The hash mark (`#`) indicates Configuration mode. Beginning a command with `run` indicates to the VRA shell that an operational command is being presented. The previous example also illustrates the ability to "grep" against the output of commands.

## Command exploration
{: #command-exploration}

The VRA command shell includes tab completion capabilities. If you are curious about which commands are available, press the Tab key for a list and a short explanation. This works both at the shell prompt and while typing a command. For instance: 

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## Sample configuration
{: #sample-configuration}

Configurations are laid out in a hierarchical pattern of nodes. Consider this static route block:

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

This would be generated by the commands:

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

This example illustrates that it is possible for a node (static) to have multiple child nodes. To remove the route for `192.168.1.0/24` the command `delete protocols static route 192.168.1.0/24` would be used. If `192.168.1.0/24` was left off the command, then both route nodes are marked for deletion.

Remember that the configuration is not actually changed until the `commit` command is issued. To compare the current running configuration to any changes that are present in the configuration buffer, use the `compare` command. To flush the configuration buffer, use `discard`.

## Users and Role-based Access Control (RBAC)
{: #users-and-role-based-access-control-rbac-}

User accounts can be configured with three levels of access:

* Admin
* Operator
* Superuser

Operator level users can run `show` commands to view the running status of the system and issue `reset` commands to restart services on the device. Operator level permissions do not imply read-only access.

Admin level users have full access to all configurations and operations for the device. Admin users can view running configurations, change configuration settings for the device, reboot the device, and shutdown the device.

Superuser level users are able to execute commands with root privileges through the `sudo` command in addition to having admin level privileges.

Users can be configured for password or public key authentication styles or both. Public key authentication is used with SSH and allows users to login using a key file on their system. To create an operator user with a password:

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

When no level is specified, a user is considered admin level. The user passwords in this case display as encrypted in the configuration file.
{: note}

Role-based Access Control (RBAC) is a method of restricting access to part of the configuration to authorized users. RBAC allows admins to define rules for a user group that restrict the commands they can run.

RBAC is performed by creating a group assigned to the Access Control Management (ACM) rule set, adding a user to the group, creating a rule set to match the group to the paths in the system, then configuring the system to allow or deny those paths that are applied to the group.

By default, there is no ACM rule set defined in {{site.data.keyword.vra_full}}, and ACM is disabled. If you want use RBAC to provide granular access control, you must enable the ACM and add following default ACM rules in addition to your own defined rules:

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

Refer to the [supplemental documentation](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-supplemental-vra-documentation) before attempting to enable ACM rules. Inaccurate ACM rule settings can cause device access denials or system inoperability errors.

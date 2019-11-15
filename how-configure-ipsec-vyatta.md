---
copyright:
  years: 1994, 2017
lastupdated: "2019-11-14"

keywords: configure, configuring, vyatta, 5400, ipsec
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank_"}
{:note: .note}
{:important: .important}
{:tip: .tip}

# Configuring IPSec on Vyatta 5400
{: #configuring-ipsec-on-vyatta-5400}

The Brocade 5400 vRouter (Vyatta) device will be referred to as "local" in regards to the Internet Security Protocol (IPSec) tunnel. Each of the following commands will perform different functions to configure IPSec site-to-site. Note that this example of IPSec site-to-site demonstrates the tunnel on SoftLayer's public network; use **bond0** for private IPSec site-to-site connections.
{: shortdesc}

1. "Tell" the tunnel the purpose of **interface bond1:**

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Setup the first phase of the two phase tunnel. The following commands will:

  * Create a new **ike** group called **test** and use **dh-group** as the key exchange type.
  * Specify the type of encryption to use; if this is not setup, the device will use **aes128** as a default
  * Use the hash function **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Setup the second phase of the two phase tunnel. The following commands will:

  * Disable perfect forward secrecy (PFS) because not all device can use it. (The esp in the command is the second part of the encryption.)
  * Specify the type of encryption to use; if this is not setup, the device will use **aes128** as a default
  * Use the `has` function **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Setup the IPSec site-to-site encryption parameters. The following commands will:

  * Specify the remote side IP and that the IPSec will be using pre-shared secret
  * Use the remote IP and the secret key TestPSK
  * Set the default **esp** group for the tunnel to TestESP
  * "Tell" the IPSec to use ike-group TestIKE, which was defined earlier<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Create the mapping for the IPSec tunnel. The following commands will, based on the example in the material:

  * "Tell" the tunnel to map to the remote IP of 169.54.254.117 to the local IP address of bond1, 50.97.240.219
  * Route only IP addresses with the subnet of 10.54.9.152/29 that are on the local server interface to the remote server 169.54.254.117
  * Route tunnel 1 remote traffic 169.54.254.117 to remote subnet of 192.168.1.2/32<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

The next step is to set up the remote-side device, which is a Brocade 5400 vRouter 6.6.5 R.

  * Use the just-configured device (that was configured in operation mode) to enter the command show configuration commands. A list of commands used to set up the device will be presented.
  * Copy the commands to a text editor. The commands use to set up the local device will be used to set up the remote server with modifications to the IP to point the Brocade 5400 vRouter 6.6.5R device on SoftLayer.

The remote-side configuration used earlier is below. The changes needed for the local-side configuration are in bold.

Remote side configuration:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (The site-to-site peer IP has been swapped)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (The local IP address has been swapped)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (The local prefix and the remote prefix have not been swapped)

* Copy and paste the new commands into the remote server (be sure to be in configuration mode), type commit, then save.
* Type `run show vpn ike sa` to see if the tunnel is now established.

Here is a recap of what has been done: Only route IP addresses with the subnet of '10.54.9.152/29' that reside on the local interface (bond1, 50.97.240.219) to only 192.168.1.2/32 subnets on the remote service residing in the interface with the IP address of 169.54.254.117.

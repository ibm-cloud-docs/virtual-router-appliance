---

copyright:
  years: 2024
lastupdated: "2024-08-02"

keywords: 

subcollection: virtual-router-appliance

---

{{site.data.keyword.attribute-definition-list}}

# Configuring IPsec on Vyatta 5400
{: #configuring-ipsec-on-vyatta-5400}

The Brocade 5400 vRouter (Vyatta) device is referred to as "local" with regards to the IPsec tunnel. Each of the following commands performs different functions to configure IPsec site-to-site. Note that the following example demonstrates the tunnel on the SoftLayer public network. Use **bond0** for private IPsec site-to-site connections.
{: shortdesc}

1. "Tell" the tunnel the purpose of **interface bond1:**

   * *set vpn ipsec ipsec-interfaces interface bond1*

2. Set up the first phase of the two-phase tunnel. The following commands:

   * Create a new **ike** group named **test** and use **dh-group** as the key exchange type.
   * Specify the type of encryption to use; if this is not set up, the device uses **aes128** as a default.
   * Use the hash function **sha-1** 
  
   `set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'`
   `set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'`
   `set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'`

3. Set up the second phase of the two-phase tunnel. The following commands:

   * Disable perfect forward secrecy (PFS) because not all devices can use it. (The esp in the command is the second part of the encryption.)
   * Specify the type of encryption to use; if this is not set up, the device uses **aes128** as a default.
   * Use the `has` function **sha-1**.
  
   `set vpn ipsec esp-group TestESP pfs disabl۪`
   `set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪`
   `set vpn ipsec esp-group TestESP proposal 1 hash sha1۪`

4. Set up the IPsec site-to-site encryption parameters. The following commands:

   * Specify the remote side IP and that the IPsec is using pre-shared secret.
   * Use the remote IP and the secret key TestPSK.
   * Set the default **esp** group for the tunnel to TestESP.
   * "Tell" the IPsec to use ike-group TestIKE, which was defined earlier
   
   `set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪`
   `set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪`
   `set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪`
   `set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪`

5. Create the mapping for the IPsec tunnel. Based on the example in the material, the following commands:

   * "Tell" the tunnel to map to the remote IP of 169.54.254.117 to the local IP address of **bond1**, `50.97.240.219`.
   * Route only IP addresses with the subnet of 10.54.9.152/29 that are on the local server interface to the remote server 169.54.254.117.
   * Route tunnel 1 remote traffic `169.54.254.117` to remote subnet of `192.168.1.2/32`.
  
   `set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219`
   `set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29`
   `set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32`

The next step is to set up the remote-side device, which is a Brocade 5400 vRouter 6.6.5 R.

* Use the just-configured device (that was configured in operation mode) to enter the command show configuration commands. A list of commands used to set up the device is presented.
* Copy the commands to a text editor. The commands used to set up the local device are used to set up the remote server with modifications to the IP to point the Brocade 5400 vRouter 6.6.5R device on SoftLayer.

The remote-side configuration used earlier is as follows. The changes that are needed for the local-side configuration are in bold.

Remote side configuration:

set vpn ipsec ipsec-interfaces interface eth1

set vpn ipsec esp-group TestESP pfs disable۪

set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪

set vpn ipsec esp-group TestESP proposal 1 hash sha1۪

set vpn ipsec ike-group TestIKE proposal 1 dh-group 2

set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪

set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪

set vpn ipsec ipsec-interfaces interface eth1۪

set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (The site-to-site peer IP was swapped)

set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪

set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪

set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪

set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117** (The local IP address was swapped.)

set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32

set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29** (The local prefix and the remote prefix have not been swapped.)

* Copy and paste the new commands into the remote server (be sure to be in configuration mode), type commit, then save.
* Type `run show vpn ike sa` to see whether the tunnel is now established.

Here is a recap of what was done: Route IP addresses only with the subnet of `10.54.9.152/29` that reside on the local interface (**bond1**, `50.97.240.219`) to only `192.168.1.2/32` subnets on the remote service residing in the interface with the IP address of `169.54.254.117`.

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

# How to configure IPSec on Vyatta
Perform the following procedure to configure IPSec site-to-site. 

**NOTE:** This example of IPSec site-to-site demonstrates the tunnel on IBM Cloud's public network; use `bond0` for private IPSec site-to-site connections.

1. Tell the tunnel the purpose of inteface bond1:

	`set vpn ipsec ipsec-interfaces interface bond1`

2. Set up the first phase of the two phase tunnel.

	Create a new 'ike' group called test and use dh-group as the key exchange type:
	
	`set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'`

	Specify the type of encryption to use; if this is not set up, the device will use 'aes128' as a default:

	`set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'`

	Use the hash function sha-1:
	
	`set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'`
	
3. Setup the second phase of the two phase tunnel. 

	Disable perfect forward secrecy (PFS), because not all devices can use it. The 'esp' in the command is the second part of the encryption:
	
	`set vpn ipsec esp-group TestESP pfs disabl`

	Specify the type of encryption to use; if this is not set up, the device will use 'aes128' as a default:

	`set vpn ipsec esp-group TestESP proposal 1 encryption aes128`

	Use the 'hash' function sha-1:

	`set vpn ipsec esp-group TestESP proposal 1 hash sha1۪`

4. Setup the IPSec site-to-site encryption parameters.

	Specify that both the remote side IP and the IPSec will be using 'pre-shared secret':

	`set vpn ipsec site-to-site peer 169.54.254.117 	authentication mode pre-shared-secret`
	
	Use the remote IP and the secret key 'TestPSK':

	`set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK`
	
	Set the default esp group for the tunnel to 'TestESP':

	`set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP`
		
	Tell the IPSec to use ike-group 'TestIKE', which you defined previously:
	
	`set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE`

5. Create the mapping for the IPSec tunnel. 

	Tell the tunnel to map the remote IP of 169.54.254.117 to the local IP address of bond1, 50.97.240.219:

	`set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219`
	
	Route only IP addresses with the subnet of 10.54.9.152/29 that are on the local server interface to the remote server 169.54.254.117:

	`set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29`
		
	Route tunnel 1 remote traffic 169.54.254.117 to remote subnet of 192.168.1.2/32:
	
	`set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32`

6. Set up the remote-side device, which is a Brocade 5400 vRouter 6.6.5 R.

	Use the newly configured device to enter the command `show configuration commands`. A list of commands used to set up the device will display.

	Copy the commands to a text editor. The commands used to set up the local device will be used to set up the remote server with modifications to the IP to point the Brocade 5400 vRouter 6.6.5R device on IBM Cloud.

	The remote-side configuration outlied before is detailed below in its entirety.

	~~~
	set vpn ipsec ipsec-interfaces interface eth1
	
	set vpn ipsec esp-group TestESP pfs disable۪
	
	set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪
	
	set vpn ipsec esp-group TestESP proposal 1 hash sha1۪
	
	set vpn ipsec ike-group TestIKE proposal 1 dh-group 2۪
	
	set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪
	
	set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪
	
	set vpn ipsec ipsec-interfaces interface eth1۪
	
	set vpn ipsec site-to-site peer 50.97.240.219 authentication mode pre-shared-secret (The site-to-site peer IP has been swapped)
	
	set vpn ipsec site-to-site peer 50.97.240.219 authentication pre-shared-secret TestPSK۪
	
	set vpn ipsec site-to-site peer 50.97.240.219 default-esp-group TestESP۪
	
	set vpn ipsec site-to-site peer 50.97.240.219 ike-group TestIKE۪
	
	set vpn ipsec site-to-site peer 50.97.240.219 local-address 169.54.254.117۪ 
	
	set vpn ipsec site-to-site peer 50.97.240.219 tunnel 1 local prefix 192.168.1.2/32۪
	
	set vpn ipsec site-to-site peer 50.97.240.219 tunnel 1 remote prefix 10.54.9.152/29۪ 
	~~~

7. Copy and paste the new commands into the remote server (be sure you are in configuration mode). Enter the command `commit` then enter `save`.

8. Enter the command `run show vpn ike sa` to see if the tunnel is now established.
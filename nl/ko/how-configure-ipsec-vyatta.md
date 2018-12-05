---
copyright:
  years: 1994, 2017
lastupdated: "2018-11-10"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta 5400에서 IPSec 구성

Brocade 5400 vRouter(Vyatta) 디바이스는 IPSec(Internet Security Protocol) 터널에 대해 "로컬"이라고도 합니다. 다음 각 명령은 서로 다른 기능을 수행하여 IPsec 사이트-투-사이트를 구성합니다. IPsec 사이트-투-사이트의 이 예제는 SoftLayer의 공용 네트워크의 터널을 설명하며, 사설 IPSec 사이트-투-사이트 연결에 **bond0**을 사용합니다.

1. 터널에 **interface bond1:**의 목적을 "알리십시오."

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. 2단계 터널 중 첫 번째 단계를 설정하십시오. 명령은 다음을 수행합니다.

  * **test**라고 하는 새 **ike** 그룹을 작성하고 키 교환 유형으로 **dh-group**을 사용합니다.
  * 사용할 암호화 유형을 지정합니다. 설정되지 않으면 디바이스에서는 기본값으로 **aes128**을 사용합니다.
  * **sha-1** 해시 기능을 사용합니다.<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. 2단계 터널 중 두 번째 단계를 설정하십시오. 명령은 다음을 수행합니다.

  * 모든 디바이스가 PFS(Perfect Forward Secrecy)를 사용할 수 없으므로 이를 사용 안함으로 설정합니다. (명령의 esp는 암호화의 두 번째 파트입니다.)
  * 사용할 암호화 유형을 지정합니다. 설정되지 않으면 디바이스에서는 기본값으로 **aes128**을 사용합니다.
  * **sha-1** 해시 기능을 사용합니다.<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. IPSec 사이트-투-사이트 암호화 매개변수를 설정하십시오. 명령은 다음을 수행합니다.

  * 원격 측 IP를 지정하면 IPSec은 사전 공유 시크릿을 사용합니다.
  * 원격 IP 및 시크릿 키 TestPSK를 사용합니다.
  * 터널의 기본 **esp** 그룹을 TestESP로 설정합니다.
  * IPSec에 이전에 정의된 ike-group TestIKE를 사용할 것을 알립니다.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. IPSec 터널에 대한 맵핑을 작성하십시오. 명령은 자료의 예제에 따라 다음을 수행합니다.

  * 터널에 bond1, 50.97.240.219의 로컬 IP 주소에 대한 169.54.254.117의 원격 IP 주소로 맵핑할 것을 알립니다.
  * 로컬 서버 인터페이스에 있는 IP 주소(10.54.9.152/29의 서브넷이 포함)만 원격 서버 169.54.254.117에 라우팅합니다.
  * 터널 1 원격 트래픽 169.54.254.117을 192.168.1.2/32의 원격 서브넷으로 라우팅합니다.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

다음 단계는 Brocade 5400 vRouter 6.6.5 R인 원격 측 디바이스를 설정하는 것입니다.

  * 방금 구성된 디바이스(운영 모드에서 구성됨)를 사용하여 show configuration commands 명령을 입력하십시오. 디바이스를 설정하는 데 사용된 명령의 목록이 표시됩니다.
  * 명령을 텍스트 편집기에 복사하십시오. 로컬 디바이스를 설정하기 위한 명령 사용은 SoftLayer의 Brocade 5400 vRouter 6.6.5R 디바이스를 가리키기 위해 IP를 수정하여 원격 서버를 설정하는 데 사용됩니다.

이전에 사용된 원격 측 구성은 다음과 같습니다. 로컬 측 구성에 필요한 변경사항은 굵게 표시됩니다.

원격 측 구성:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret(사이트-투-사이트 피어 IP가 스왑됨)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117***(로컬 IP 주소가 스왑됨)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29***(로컬 접두부 및 원격 접두부가 스왑됨)

* 새 명령을 원격 서버로 복사하여 붙여넣고(구성 모드여야 함) commit를 입력한 후 저장하십시오.
* run show vpn ike sa를 입력하여 터널이 현재 확립되었는지 확인하십시오.

요약하면 다음과 같습니다. 169.54.254.117의 IP 주소로 인터페이스에 상주하는 원격 서비스에 있는 192.168.1.2/32 서브넷에 대해서만 로컬 인터페이스(bond1, 50.97.240.219)에 상주하는 10.54.9.152/29'의 서브넷이 포함된 IP 주소만 라우팅합니다.

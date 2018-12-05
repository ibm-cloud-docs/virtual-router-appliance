---

copyright:
  years: 2017
lastupdated: "2018-11-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# VFP 인터페이스 문제점 해결
이 주제에는 VFP 인터페이스에 대한 문제점 해결 정보가 포함되어 있습니다. 

* VFP 인터페이스는 `dp0bond0`(심지어 VIF 또는 TUN)과 같은 "실제" 인터페이스가 아닙니다. 트래픽을 적절히 처리할 수 있도록 인터페이스를 연결하기 위한 방화벽 및 NAT 프로세스에 대한 플레이스홀더입니다. 일반 인터페이스처럼 VFP에서 계속 트래픽을 라우팅할 수 있지만, `thsark` 및 기타 모니터 명령에서는 트래픽을 표시하지 않습니다. 
* NAT를 사용하면, VFP로 라우팅되는 트래픽을 얻기 위해 IPsec에서 작성하는 커널 라우트 대신 더 구체적인 서브넷 범위를 사용해야 합니다. 정적 라우트가 설정되지 않은 경우에는 커널 라우트를 따릅니다. `show ip route x.x.x.x`를 사용하여 테스트할 수 있습니다.
* VFP에서 나오는 DNAT를 적절하게 처리해야 하지만, 리턴되는 트래픽에는 여전히 정적 라우트 세트가 필요합니다. IPsec 인터페이스, `dp0bond1` 또는 `dp0bond0`(또는 IPsec 트래픽을 사용하는 인터페이스)에서 나오는 비NAT 트래픽을 찾으십시오.
* VFP에서 라우팅 프로토콜 사용은 테스트되지 않았습니다.  
* VFP에서 GRE 터널 사용도 테스트되지 않았습니다. 

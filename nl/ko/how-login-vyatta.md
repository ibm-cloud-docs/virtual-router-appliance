---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta에 로그인하는 방법

웹 GUI를 사용하거나 SSH 로그인을 사용하여 직접 Brocade NFV(Network Functions Virtualization)에 액세스할 수 있습니다.

## SoftLayer의 사설 네트워크에 대한 인증

1. **세부사항** 화면(**네트워크 > 게이트웨이 어플라이언스** 메뉴)의 사설 IP 주소를 https:// 접두부를 사용하여(예: https://10.54.207.131/) 브라우저에 입력하십시오. 디바이스가 공용 인증서를 사용하지 않으므로 인증서 오류가 발생할 수 있습니다. 오류를 승인하여 웹 페이지로 이동하십시오.

2. **vyatta** 사용자의 웹 포털에 있는 **디바이스** 메뉴의 **비밀번호** 탭에서 인증서를 입력하십시오. Brocade 대시보드가 열립니다.

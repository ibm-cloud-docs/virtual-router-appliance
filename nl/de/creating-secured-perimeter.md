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

# Einen geschützten Bereich erstellen
Ein grundlegender Aspekt der Netzisolation ist die Einrichtung eines geschützten Bereichs. Ein geschützter Bereich steuert den Datenverkehr zwischen dem öffentlichen Internet zu den Kundenanlagen, die in der IBM Cloud gehostet sind. Der geschützte Bereich ermöglicht ferner die direkte Anbindung des Kundenunternehmens mithilfe von VPN-Tunneln (VPN, Virtual Private Network) und IBM Cloud Direct Link.

Ein geschützter Bereich verwendet ein Secure Perimeter Segment (SPS), um Netze von Assets innerhalb des geschützten Bereichs zu trennen. Diese Trennung hat mehrere Vorteile, wozu unter anderem Zugriffssteuerung und Isolation des Servicedatenverkehrs zwischen Segmenten gehören. Ein Secure Perimeter Segment (SPS) besteht aus zwei VLANs (Virtual Local Area Networks), nämlich einem Front-End-VLAN und einem Back-End-VLAN, und einer VRA, die mit den VLANs verbunden ist, um den Datenverkehr in das SPS und aus dem SPS zu verwalten. Ein sicherer Bereich kann mehrere SPS (Secure Perimeter Segments) umfassen, was beispielsweise für die Hochverfügbarkeit zweckmäßig ist. 

## Geschützten Bereich einrichten

Führen Sie die folgenden Schritte aus, um einen geschützten Bereich einzurichten. Eine detaillierte Beschreibung dieser Schritte finden Sie unter [Set up a Secure Perimeter in IBM Cloud](https://developer.ibm.com/dwblog/2018/ibm-cloud-vyatta-set-up-secure-perimeter).

1. Konfigurieren Sie die Berechtigungen in der IBM Cloud und in der Infrastruktur, die für den Zugriff auf die Cloud-Services und die Infrastrukturkomponenten, die zum geschützten Bereich gehören, erforderlich sind. 
2. Erstellen Sie im öffentlichen Internet durch ein öffentliches VLAN und eine Firewall eine äußere Begrenzung. Diese äußere Begrenzung wird zur Isolierung des SPS verwendet.
3. Ordnen Sie das SPS innerhalb dieser Begrenzung an.
4. Richten Sie ein Gateway ein, um eine Brücke zwischen das öffentlichem VLAN und dem SPS zu erhalten. 
5. Erstellen Sie eine VRA (Virtual Router Appliance).
6. Erstellen und konfigurieren Sie ein oder mehrere Secure Perimeter Segments (SPS).
7. Erstellen Sie ein Front-End-VLAN
8. Erstellen Sie ein Back-End-VLAN
9. Erstellen Sie ein Vyatta-Paar
10. Konfigurieren Sie Vyatta.
11. Ordnen Sie die VLANs einem Gateway zu. 
12. Konfigurieren Sie die Gateway-Schnittstelle für öffentliches VLAN. 
13. Konfigurieren Sie das Gateway auf dem Vyatta-Master.
14. Konfigurieren Sie das Gateway auf dem Vyatta-Backup. 
15. Konfigurieren Sie die Gateway-Schnittstelle für das private VLAN.
16. Konfigurieren Sie das Gateway auf dem Vyatta-Master.
17. Konfigurieren Sie das Gateway auf dem Vyatta-Backup. 
18. Aktivieren Sie die Netzoptimierung. 
19. Erstellen Sie die Firewallregeln. 
20. Konfigurieren Sie die Kubernetes-Cluster.
21. Konfigurieren Sie die vom Benutzer bereitgestellten IPs (optional).
22. Stellen Sie die Kubernetes-Cluster bereit.

## Dedizierte Lösungen in der IBM Cloud
Ein geschützter Bereich trägt zusammen mit Rechnerisolation und Datenverschlüsselung zu einer vollständigen, dedizierten Lösung in der öffentlichen IBM Cloud bei. Eine Beschreibung dedizierter Cloudlösungsmuster finden Sie unter [Implementing a Dedicated Solution Pattern](https://developer.ibm.com/dwblog/2018/ibm-cloud-dedicated-cloud-solution-patterns/).

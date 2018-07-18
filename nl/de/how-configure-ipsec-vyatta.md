---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-25"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vorgehensweise zum Konfigurieren von IPSec auf Vyatta

Der Brocade 5400 vRouter (Vyatta) wird als "local" bezüglich des IPSec-Tunnels (Internet Security Protocol) bezeichnet. Jeder der folgenden Befehle führt unterschiedliche Funktionen aus, um IPSec site-to-site zu konfigurieren. Beachten Sie, dass dieses Beispiel mit IPSec site-to-site den Tunnel im öffentlich Netz von SoftLayer zeigt. Verwenden Sie **bond0** für private IPSec site-to-site-Verbindungen.

1. "Informieren" Sie den Tunnel über den Zweck von **interface bond1:**

  * *set vpn ipsec ipsec-interfaces interface bond1*

2. Definieren Sie die erste der beiden Phasen des Tunnels. Die folgenden Befehle dienen folgenden Zwecken:

  * Erstellen Sie eine neue **ike**-Gruppe mit dem Namen **test** und verwenden Sie **dh-group** als Schlüsselaustauschtyp.
  * Geben Sie den Typ der zu verwendenden Verschlüsselung an. Falls keine Verschlüsselung definiert ist, wird standardmäßig **aes128** verwendet.
  * Verwenden Sie die Hashfunktion **sha-1**<br/><br/>
  1\. *set vpn ipsec ike-group TestIKE proposal 1 dh-group '2'*<br/>
  2\. *set vpn ipsec ike-group TestIKE proposal 1 encryption 'aes128'*<br/>
  3\. *set vpn ipsec ike-group TestIKE proposal 1 hash 'sha1'*<br/>

3. Definieren Sie die zweite Phase der beiden Phasen des Tunnels. Die folgenden Befehle dienen folgenden Zwecken:

  * Inaktivieren Sie PFS (Perfect Forward Secrecy), da nicht alle Einheiten PFS verwenden können. (Die Angabe 'esp' im Befehl ist der zweite Teil der Verschlüsselung.)
  * Geben Sie den Typ der zu verwendenden Verschlüsselung an. Falls keine Verschlüsselung definiert ist, wird standardmäßig **aes128** verwendet.
  * Verwenden Sie die Funktion **sha-1**<br/><br/>
  1\. *set vpn ipsec esp-group TestESP pfs disabl۪*<br/>
  2\. *set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*<br/>
  3\. *set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*<br/>

4. Definieren Sie die IPSec site-to-site-Verschlüsselungsparameter. Die folgenden Befehle dienen folgenden Zwecken:

  * Geben Sie die IP der fernen Site an und geben Sie an, dass IPSec einen bereits zuvor gemeinsam verwendeten geheimen Schlüssel verwenden wird.
  * Verwenden Sie die ferne IP und den geheimen Schlüssel TestPSK
  * Definieren Sie die **esp**-Standardgruppe für den Tunnel zu TestESP
  * "Informieren" Sie IPSec über die Verwendung der ike-group TestIKE, die zuvor definiert wurde.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication mode pre-shared-secret۪*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 authentication pre-shared-secret TestPSK۪*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 default-esp-group TestESP۪*<br/>
  4\. *set vpn ipsec site-to-site peer 169.54.254.117 ike-group TestIKE۪*<br/>

5. Erstellen Sie die Zuordnung für den IPSec-Tunnel. Die folgenden Befehle werden auf Grundlage des Beispiels verwendet:

  * "Informieren" Sie den Tunnel, dass eine Zuordnung der fernen IP 169.54.254.117 zur lokalen IP-Adresse bond1, 50.97.240.219 erfolgen soll.
  * Leiten Sie nur IP-Adressen mit dem Teilnetz 10.54.9.152/29, die sich in der Schnittstelle des lokalen Servers befinden, an den fernen Server 169.54.254.117 weiter.
  * Leiten Sie den fernen Verkehr von Tunnel 1 von 169.54.254.117 an das ferne Teilnetz an 192.168.1.2/32 weiter.<br/><br/>
  1\. *set vpn ipsec site-to-site peer 169.54.254.117 local-address ۪50.97.240.219*<br/>
  2\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 local prefix 10.54.9.152/29*<br/>
  3\. *set vpn ipsec site-to-site peer 169.54.254.117 tunnel 1 remote prefix 192.168.1.2/32*<br/>

Der nächste Schritte ist die Installation der fernen Einheit, dem Brocade 5400 vRouter 6.6.5 R

  * Verwenden Sie die gerade eben konfigurierte Einheit (das im Betriebsmodus konfiguriert wurde), um den Befehl zum Anzeigen der Konfigurationsbefehle einzugeben. Eine Liste der Befehle für die Konfiguration der Einheit wird angezeigt.
  * Kopieren Sie die Befehle in einen Texteditor. Die Befehle zur Konfiguration der lokalen Einheit werden verwendet, um den fernen Server mit Änderungen an der IP zu konfigurieren und so auf den Brocade 5400 vRouter 6.6.5R im SoftLayer zu verweisen.

Die ferne Konfiguration, die zuvor verwendet wurde, finden Sie unten. Die für die lokale Konfiguration erforderlichen Änderungen sind fett hervorgehoben.

Ferne Konfiguration:

*set vpn ipsec ipsec-interfaces interface eth1*

*set vpn ipsec esp-group TestESP pfs disable۪*

*set vpn ipsec esp-group TestESP proposal 1 encryption aes128۪*

*set vpn ipsec esp-group TestESP proposal 1 hash sha1۪*

*set vpn ipsec ike-group TestIKE proposal 1 dh-group 2*

*set vpn ipsec ike-group TestIKE proposal 1 encryption aes128۪*

*set vpn ipsec ike-group TestIKE proposal 1 hash sha1۪*

*set vpn ipsec ipsec-interfaces interface eth1۪*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication mode pre-shared-secret (Die site-to-site Peer-IP wurde ausgetauscht.)*

*set vpn ipsec site-to-site peer **50.97.240.219** authentication pre-shared-secret TestPSK۪*

*set vpn ipsec site-to-site peer **50.97.240.219** default-esp-group TestESP۪*

*set vpn ipsec site-to-site peer **50.97.240.219** ike-group TestIKE۪*

*set vpn ipsec site-to-site peer **50.97.240.219** local-address **169.54.254.117*** (Die lokale IP-Adresse wurde ausgetauscht.)

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 local prefix 192.168.1.2/32*

*set vpn ipsec site-to-site peer **50.97.240.219** tunnel 1 remote prefix **10.54.9.152/29*** (Das lokale Präfix und das ferne Präfix wurden nicht ausgetauscht.)

* Kopieren Sie die neuen Befehle und fügen Sie sie im fernen Server ein. Stellen Sie sicher, dass Sie dabei den Konfigurationsmodus verwenden. Geben Sie 'commit' ein und speichern Sie dann.
* Geben Sie 'run show vpn ike sa' ein, um zu sehen, ob der Tunnel eingerichtet wurde.

Hier finden Sie eine kleine Zusammenfassung der durchgeführten Aktionen: Nur IP-Adressen mit dem Teilnetz '10.54.9.152/29', die sich auf der lokalen Schnittstelle (bond1, 50.97.240.219) befinden, werden ausschließlich an 192.168.1.2/32-Teilnetze auf dem fernen Service weitergeleitet, der sich in der Schnittstelle mit der IP-Adresse 169.54.254.117 befindet.

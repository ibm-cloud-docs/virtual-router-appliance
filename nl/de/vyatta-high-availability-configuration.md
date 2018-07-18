---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta-Hochverfügbarkeitskonfiguration

Die Hochverfügbarkeit von Vyatta wird durch die Verwendung von VRRP (Virtual Routing Redundancy Protocol) unterstützt. Jede Gateway-Gruppe verfügt über zwei primäre VRRP IP-Adressen. Eine für die private und eine für die öffentliche Seite des Netzes. 

**HINWEIS:** Rein private Vyattas verfügen nur über die private VRRP. 

Diese IP-Adressen sind die Ziel-IPs für die Softlayer-Netzinfrastruktur, mit der alle Teilnetze in VLANs weitergeleitet werden, die zu den Gateway-Membern gehören. Nur bei einer Vyatta zur Zeit werden diese VRRP IPs ausgeführt. Die anderen Member der Gatewaygruppe haben diese währenddessen administrativ unterdrückt.

Die Konfiguration kann zwischen den beiden Vyattas mit den "config-sync"-Konfigurationsbefehlen synchronisiert werden. Diese Konfiguration ermöglicht es einem Member, eine Push-Operation für die Konfiguration bestimmter Optionen an die andere Vyatta in einer Gruppe durchzuführen und zwar selektiv. Sie können nur für die Firewallregel eine Push-Operation durchführen oder nur für die IPsec-Konfiguration oder für eine Kombination von Regelsätzen. 

Es wird empfohlen, keine Push-Operation für IP-Adressen oder andere Netzkonfigurationen durchzuführen, da config-sync für Ihre Änderungen sofort ein Commit auf anderen Vyattas durchführen und diese Schnittstellen online bringen würde. Wenn Sie Schnittstellen und Services dynamisch aktivieren möchten, sollten Sie die Transitionsscripts verwenden, um diese Aktion bei einem Failover durchzuführen. Es wird ferner empfohlen, dass Sie mehr VRRP IP-Adressen für Ihre Gateway IPs in den zugehörigen VLANs verwenden. Dies erleichtert die Verwaltung eines Failover.

Ihre VRRP-Basiskonfiguration auf beiden Maschinen sieht ähnlich wie diese Beispielkonfiguration aus:

    interfaces {
    bonding bond0 {
    address 10.28.94.213/26
    duplex auto
    hw-id 06:d6:f8:f0:fb:ee
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 192.168.10.1/24
    }
    }
    }
    bonding bond1 {
    address 50.23.184.5/26
    duplex auto
    hw-id 06:05:09:41:fb:cb
    smp_affinity auto
    speed auto
    vrrp {
    vrrp-group 1 {
    advertise-interval 1
    }
    preempt false
    priority 254
    sync-group vgroup10
    rfc3768-compatibility
    virtual-address 50.23.184.27/26
    }
    }
    }
    loopback lo {
    }
    }

VRRP erfordert, dass eine tatsächliche IP-Adresse zuerst an eine virtuelle Schnittstelle gebunden wird, bevor VRRP-Mitteilungen gesendet werden können. In vielen Fällen können Sie einfach eine IP aus dem primären Teilnetz hinzufügen. Dies kann jedoch zu Konflikten mit künftigen Einrichtungen führen. Möglicherweise möchten Sie auch ein VLAN weiterleiten, bei dem bereits alle primären Teilnetz-IPs einem Server zugeordnet sind. Um diese Situation zu vermeiden, können Sie eine IP-Adresse außerhalb des Bereichs auf beiden Vyattas verwenden, die diese für die Kommunikation miteinander verwenden können. Beispiel:

Auf der ersten Vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.10/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 200
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

Auf der zweiten Vyatta:

    set interfaces bonding bond0 vif 1000 address 172.16.0.11/29
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 sync-group vgroup10
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 priority 199
    set interfaces bonding bond0 vif 1000 vrrp vrrp-group 10 virtual-address 192.168.10.1/24

In diesem Fall haben beide Vyattas ihre eigene IP, die nicht mit dem zugeordneten Teilnetz in Konflikt gerät. Sie können fast jeden beliebigen privaten Bereich auswählen. Suchen Sie jedoch ein kleines Teilnetz aus, das nicht mit anderen Routen, über die Sie möglicherweise verfügen, in Konflikt gerät. Beispiele wären Teilnetzbereiche über einem IPsec-Tunnel oder eine 10.0.0.0/8-Adresse, die mit Softlayer in Konflikt steht. Wenn Sie dies beachten, sollten keine Probleme auftreten.

Sie möchten auch einen "sync-group"-Namen hinzufügen. Alle VRRP-Adressen sollten ein Teil derselben sync-group sein. Wenn ein Failure bei einer Schnittstelle auftritt, führt dies dazu, dass alle Schnittstellen in derselben sync-group ebenfalls ausfallen. Andernfalls kann es dazu kommen, dass einige Schnittstellen als MASTER und andere als BACKUP fungieren. Verwenden Sie denselben Namen in der nativen VLAN-Konfiguration bond0 und bond1.

HINWEIS: Die VRRP-Konfigurationen bond0 und bond1 können eine Zeile für rfc3768-compatibility enthalten. Dies ist nicht für VRRP in einer virtuellen Schnittstelle erforderlich, sondern nur für ein natives VLAN bei bond0 und bond1.

Auf einem neu bereitgestellten Gatewaypaar, wird config-sync nur über eine Minimalkonfiguration verfügen:


    config-sync {
    remote-router 10.28.94.195 {
    password ****************
    sync-map syncme
    username vyatta
    }

Sie müssen Regeln hinzufügen, um festzulegen, welche Konfigurationszweige migriert werden sollen. Wenn Sie zum Beispiel für die Firewall und die ipsec-Konfiguration eine Push-Operation durchführen möchten, fügen Sie diese Befehle hinzu:


    set system config-sync sync-map syncme rule 1 action include
    set system config-sync sync-map syncme rule 1 location firewall
    set system config-sync sync-map syncme rule 2 action include
    set system config-sync sync-map syncme rule 2 location "vpn ipsec"

Sobald für diese Befehle ein Commit durchgeführt wurde, wird für alle Änderungen an der Firewall oder der ipsec-Konfiguration eine Push-Operation auf die andere Vyatta durchgeführt,

**HINWEIS:** sync-group und sync-map sind zwei unterschiedliche Dinge. Die Konfiguration sync-map ist für Regeln konzipiert, mit denen Konfigurationsänderungen mit einer Push-Operation auf eine andere Vyatta übertragen werden. Die andere Konfiguration, sync-group, ist für VRRP IPs konzipiert, mit denen diese als Gruppe und nicht einzeln übernommen werden. Die Verwendung einer Konfiguration hat keine Auswirkung auf die andere.

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

# API-Referenz für IBM Virtual Router Appliance
{: #api-reference-for-ibm-virtual-router-appliance}

Die SoftLayer® Application Programming Interface (SLAPI) ist eine Entwicklungsschnittstelle für direkte Interaktionen von Entwicklern und Systemadministratoren mit dem IBM© Cloud-Back-End-System.

Die SLAPI bildet die Grundlage für viele Funktionen im Kundenportal. Mit anderen Worten: Wenn eine Interaktion im Kundenportal möglich ist, kann sie auch in der API ausgeführt werden. Da die API programmgesteuerte Interaktionen mit allen Bereichen des Kundenportals ermöglicht, können Sie mit der API Tasks automatisieren.

Die SLAPI ist ein System für ferne Prozeduraufrufe (Remote Procedure Calls, RPCs). Bei jedem Aufruf werden Daten an einen API-Endpunkt gesendet und strukturierte Daten als Antwort empfangen. Das Format zum Senden und Empfangen von Daten mit der SLAPI hängt davon ab, welche API-Implementierung Sie auswählen. Die Datenübertragung der SLAPI erfolgt gegenwärtig mit SOAP, XML-RPC oder REST.

Weitere Informationen zur SLAPI mit Bezug auf die IBM Virtual Router Appliance-APIs finden Sie in den folgenden Ressourcen im SoftLayer Development Network (SLDN):

- [SoftLayer_Network_Gateway - API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway){: new_window} 
- [SoftLayer_Network_Gateway_Member - API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Member){: new_window} 
- [SoftLayer_Network_Gateway_Vlan - API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan){: new_window} 
- [SoftLayer_Network_Gateway_Status - API ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Status){: new_window} 
- [SoftLayer-API - Python-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://softlayer-api-python-client.readthedocs.io/en/latest/api/client/){: new_window} 
- [Weitere SoftLayer-API-Beispiele für verschiedene Sprachen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer.github.io/python/){: new_window} 


### Beispiel - Gateway-Appliance bestellen

```
import SoftLayer
from pprint import pprint


def main():

    client = SoftLayer.create_client_from_env(username=<eigener_benutzername>,
                                              api_key=<eigener_api-schlüssel>,
                                              endpoint_url="https://api.softlayer.com/xmlrpc/v3")

    productOrder = {
        'orderContainers': [
            {
              "complexType": ('SoftLayer_Container_Product_Order_Hardware' +
                                '_Server_Gateway_Appliance'),
                "prices": [
                    {"id", 205533},  # Preis für Betriebssystem
                    {"id", 177619},  # Preis für Server
                    {"id", 17188},   # Preis für RAM
                    {"id", 63077},   # Preis für Platte
                    {"id", 52527},   # Preis für Netzgeschwindigkeit
                    {"id", 418},
                    {"id": 198769},
                    {"id": 75007},
                    {"id": 21},
                    {"id": 906},
                    {"id": 792},
                    {"id": 56},
                    {"id": 57},
                    {"id": 59},
                    {"id": 420}
                ]
                "hardware": [{
                    "hostname": eigener_gw-name,
                    "domain": eigene_domäne.com
                }],
                "location": "DAL09",
                "quantity": 1,
                "packageId": 174
            }
        ]
    }
    if bool(verify_order_only):
        order = client['Product_Order'].verifyOrder(productOrder)
    else:
        order = client['Product_Order'].placeOrder(productOrder)
    pprint(order)


if __name__ == '__main__':
    main()
```
{: codeblock}

### Beispiel - VLANs für Gateway-Appliance zuordnen, weiterleiten, umgehen und die Zuordnung aufheben

```
import SoftLayer
from pprint import pprint

class Gateway:
    def __init__(self, client, gateway_id):
        self.__gateway_id = gateway_id
        self.__client = client
        self.__inside_vlans = None

    def getNetworkVlanId(self, vlanNum):
        """
        Fordert das VLAN-Objekt (VLAN-Objekt-ID) anhand der 'vlanNumber'
        über einen SoftLayer-API-Aufruf an
        """
        nwfilter = {'networkVlans': {'vlanNumber': {'operation': vlanNum}}}
        mask = 'mask[id]'
        vlan = self.__client['SoftLayer_Account'].getNetworkVlans(filter=nwfilter,
                                           mask=mask)
        if vlan:
            return vlan[0]['id']
        else:
            pprint("VLAN-Objekt nicht vorhanden")
            return 0

    def getInsideVlans(self):
        """
        Ruft die internen VLANs über einen SoftLayer-API-Aufruf ab
        """
        self.__inside_vlans = self.__client['Network_Gateway'].getInsideVlans(
                    id=self.__gateway_id,
                    mask='mask[id, networkVlanId, networkVlan.vlanNumber]')
        return self.__inside_vlans

    def __findVlanAssocId(self, vlanNum):
        for vlan in self.__inside_vlans:
            if int(vlan['networkVlan']['vlanNumber']) == int(vlanNum):
                return vlan['id']
        return None

    def associateVlan(self, vlanNum):
        vlanId = self.getNetworkVlanId(vlanNum)
        if vlanId:
            vlanObj = {"bypassFlag": "false",
                       "networkGatewayId": self.__gateway_id,
                       "networkVlanId": vlanId}
            obj = self.__client['Network_Gateway_Vlan'].createObject(vlanObj)
            return True
        else:
            pprint("Ungültiges VLAN")
            return False

    def disassociateVlan(self, vlanNum):
        vlanId = self.getNetworkVlanId(vlanNum)
        self.__client['Network_Gateway_Vlan'].deleteObject(id=vlanId)
        return True

    def bypassAllVlans(self):
        self.__client['Network_Gateway'].bypassAllVlans(id=self.__gateway_id)

    def routeAllVlans(self):
        self.__client['Network_Gateway'].unbypassAllVlans(id=self.__gateway_id)

    def bypassSingleVlan(self, vlan):
        vlanAssocId = self.__findVlanAssocId(vlan)
        if vlanAssocId:
            vlanAssocObj = [{'id': vlanAssocId}]
            self.__client['Network_Gateway'].bypassVlans(vlanAssocObj,
                                                         id=self.__gateway_id)
        else:
            pprint("Umgehen von VLAN nicht möglich; %s nicht zugeordnet" % vlan)

    def routeSingleVlan(self, vlan):
        vlanAssocId = self.__findVlanAssocId(vlan)
        if vlanAssocId:
            vlanAssocObj = [{'id': vlanAssocId}]
            self.__client['Network_Gateway'].unbypassVlans(vlanAssocObj,
                                                           id=self.__gateway_id)
        else:
            pprint("Weiterleitung über VLAN nicht möglich; %s nicht zugeordnet" % vlan)
```
{: codeblock}

### Beispiel für das Abbrechen einer Gateway-Appliance

```
import SoftLayer
from pprint import pprint


def getGatewayMembers(client, appliance_id):
    """
        Ruft Gateway_Member ab
        Argumente:
            client:
                Berechtigungsnachweise des SL-Clients
            appliance_id:
                ID der Gateway-Appliance, die zurückgefordert wird
        Rückgaben:
            Gateway-Member
    """
    mask = 'mask[hardwareId]'
    return client['Network_Gateway'].getMembers(id=appliance_id, mask=mask)


def cancelGateway(client, appliance_id):
    """
        Standalone-Gateway oder HA-Paar-Gateway abbrechen
        Argumente:
            client:
                Berechtigungsnachweise des SL-Clients
            appliance_id:
                ID der Gateway-Appliance, die zurückgefordert wird
        Rückgaben:
            'True', wenn alle Member der Appliance erfolgreich zurückgefordert wurden;
            sonst 'False'
    """
    members = getGatewayMembers(client, appliance_id)

    for hardware in members:
        billingID = client['Hardware'].getBillingItem(id=hardware['hardwareId'])
        result = client['Billing_Item'].cancelItem(False,
                                                   False,
                                                   "No longer needed",
                                                   "", id=billingID['id'])


def main():

    client = SoftLayer.create_client_from_env(username=<eigener_benutzername>,
                                              api_key=<eigener_api-schlüssel>,
                                              endpoint_url="https://api.softlayer.com/xmlrpc/v3")

    cancelGateway(client, eigene_gateway-id)

if __name__ == '__main__':
    main()
```
{: codeblock}

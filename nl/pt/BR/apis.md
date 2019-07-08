---

copyright:
  years: 2017
lastupdated: "2018-11-10"

keywords: apis, api

subcollection: virtual-router-appliance

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:note: .note}
{:important: .important}

# Referência de API para o IBM Virtual Router Appliance
{: #api-reference-for-ibm-virtual-router-appliance}

A SoftLayer® Application Programming Interface (SLAPI) é a interface de desenvolvimento que fornece aos desenvolvedores e administradores do sistema a interação direta com o sistema back-end do IBM© Cloud.

O SLAPI empodera muitos dos recursos no Portal do Cliente, o que geralmente significa que, se uma interação é possível no Portal do Cliente, ela também pode ser executada na API. Como você pode interagir programaticamente com todas as partes do ambiente do Portal do Cliente dentro da API, é possível usar a API para automatizar tarefas.

O SLAPI é um sistema de Chamada de Procedimento Remoto (RPC). Cada chamada envolve enviar dados para um terminal de API e receber dados estruturados em retorno. O formato usado para enviar e receber dados com a SLAPI depende de qual implementação da API você escolhe. A
SLAPI usa atualmente SOAP, XML-RPC ou REST para transmissão de dados.

Para obter mais informações sobre o SLAPI com relação às APIs do IBM Virtual Router Appliance, consulte os recursos a seguir no SoftLayer Development Network (SLDN):

- [API SoftLayer_Network_Gateway ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway){: new_window}
- [API SoftLayer_Network_Gateway_Member ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Member){: new_window}
- [API SoftLayer_Network_Gateway_Vlan ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan){: new_window}
- [API SoftLayer_Network_Gateway_Status ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Status){: new_window}
- [SoftLayer API Python Client ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://softlayer-api-python-client.readthedocs.io/en/latest/api/client/){: new_window}
- [Mais Exemplos de API do SoftLayer com diferentes idiomas ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer.github.io/python/){: new_window}


### Exemplo - Solicitando um Dispositivo de Gateway
{: #example-ordering-a-gateway-appliance}

```
import SoftLayer
from pprint import pprint


def main():

    client = SoftLayer.create_client_from_env(username=<your_username>,
                                              api_key=<your_api_key>,
                                              endpoint_url="https://api.softlayer.com/xmlrpc/v3")

    productOrder = {
        'orderContainers': [
            {
              "complexType": ('SoftLayer_Container_Product_Order_Hardware' +
                                '_Server_Gateway_Appliance'),
                "prices": [
                    {"id", 205533},  # price for OS
                    {"id", 177619},  # price for Server
                    {"id", 17188},   # price for RAM
                    {"id", 63077},   # price for DISK
                    {"id", 52527},   # price for Network speed
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
                    "hostname": your_gwname,
                    "domain": your_domain.com
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

### Exemplo - Associando, roteando, efetuando bypass e desassociando VLANs para o Dispositivo de Gateway
{: #example-associating-routing-bypassing-and-disassociating-vlans-to-the-gateway-appliance}

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
        obtains the VLAN Object (Vlan Object id) given the vlanNumber
        via SoftLayer API call
        """
        nwfilter = {'networkVlans': {'vlanNumber': {'operation': vlanNum}}}
        mask = 'mask[id]'
        vlan = self.__client['SoftLayer_Account'].getNetworkVlans(filter=nwfilter,
                                           mask=mask)
        if vlan:
            return vlan[0]['id']
        else:
            pprint("Vlan Object not present")
            return 0

    def getInsideVlans(self):
        """
        obtains the inside VLANs via SoftLayer API call
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
            pprint("Invalid Vlan")
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
            pprint("Cannot bypass VLAN %s is not associated" % vlan)

    def routeSingleVlan(self, vlan):
        vlanAssocId = self.__findVlanAssocId(vlan)
        if vlanAssocId:
            vlanAssocObj = [{'id': vlanAssocId}]
            self.__client['Network_Gateway'].unbypassVlans(vlanAssocObj,
                                                           id=self.__gateway_id)
        else:
            pprint("Can't route thru VLAN %s is not associated" % vlan)
```
{: codeblock}

### Exemplo de cancelamento de um Dispositivo de Gateway

```
import SoftLayer
from pprint import pprint


def getGatewayMembers(client, appliance_id):
    """
        Retrieves the gateway's member(s)
        args:
            client:
                SL client credentials
            appliance_id:
                ID of the gateway appliance to be reclaimed
        returns:
            gateway members
    """
    mask = 'mask[hardwareId]'
    return client['Network_Gateway'].getMembers(id=appliance_id, mask=mask)


def cancelGateway(client, appliance_id):
    """
        Cancels a standalone or HA pair gateway
        args:
            client:
                SL client credentials
            appliance_id:
                ID of the gateway appliance to be reclaimed
        returns:
            True if all members of the appliance are successfully reclaimed
            False otherwise
    """
    members = getGatewayMembers(client, appliance_id)

    for hardware in members:
        billingID = client['Hardware'].getBillingItem(id=hardware['hardwareId'])
        result = client['Billing_Item'].cancelItem(False,
                                                   False,
                                                   "No longer needed",
                                                   "", id=billingID['id'])


def main():

    client = SoftLayer.create_client_from_env(username=<your_username>,
                                              api_key=<your_api_key>,
                                              endpoint_url="https://api.softlayer.com/xmlrpc/v3")

    cancelGateway(client, your_gateway_id)

if __name__ == '__main__':
    main()
```
{: codeblock}

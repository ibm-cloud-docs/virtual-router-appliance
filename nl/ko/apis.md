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

# API 참조

SLAPI(SoftLayer® Application Programming Interface)는 개발자와 시스템 관리자가 IBM Cloud 백엔드 시스템과 직접 상호작용하는 개발 인터페이스입니다.

SLAPI는 고객 포털에서 많은 기능을 제공하며, 일반적으로 고객 포털에서 상호작용이 가능한 경우 API에서도 실행할 수 있음을 의미합니다. API 내에서 고객 포털 환경의 모든 부분과 프로그래밍 방식으로 상호작용할 수 있기 때문에 API를 사용하여 태스크를 자동화할 수 있습니다.

SLAPI는 원격 프로시저 호출(RPC) 시스템입니다. 각 호출에는 API 엔드포인트로 데이터를 송신하고 구조화된 데이터를 수신하는 과정이 포함됩니다. SLAPI를 통해 데이터를 송수신할 때 사용되는 형식은 사용자가 선택한 API 구현에 따라 다릅니다. 현재 SLAPI에서는 데이터 전송에 SOAP, XML-RPC 또는 REST가 사용됩니다.

IBM Virtual Router Appliance API를 참조할 때 SLAPI에 대한 자세한 정보는 SLDN(SoftLayer Development Network)에서 다음 리소스를 참조하십시오.

- [SoftLayer API 개요 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/article/softlayer-api-overview?cm_mc_uid=71397258840814855542946&cm_mc_sid_50200000=1508536023){: new_window} 
- [SoftLayer_Network_Gateway API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway){: new_window} 
- [SoftLayer_Network_Gateway_Member API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Member){: new_window} 
- [SoftLayer_Network_Gateway_Vlan API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan){: new_window} 
- [SoftLayer_Network_Gateway_Status API ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Status){: new_window} 
- [SoftLayer API Python 클라이언트 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://softlayer-api-python-client.readthedocs.io/en/latest/api/client/){: new_window} 
- [다른 언어로 된 추가 SoftLayer API 예 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://softlayer.github.io/python/){: new_window} 


### 예 - 게이트웨이 어플라이언스 순서 지정

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

### 예 - 게이트웨이 어플라이언스에 대한 VLAN 연결, 라우팅, 우회 및 연결 해제

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

### 예 - 게이트웨이 어플라이언스 취소

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

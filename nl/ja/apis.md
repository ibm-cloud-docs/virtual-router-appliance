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

# API リファレンス

SoftLayer® アプリケーション・プログラミング・インターフェース (SLAPI) は、開発者やシステム管理者が IBM Cloud のバックエンド・システムと直接対話できるようにする開発用インターフェースです。

SLAPI は、カスタマー・ポータルの多くの機能を備えています。つまり、一般に、カスタマー・ポータルで可能な対話をこの API でも実行できます。 API 内でカスタマー・ポータル環境のすべての部分とプログラムで対話できるので、API を使用してタスクを自動化できます。

SLAPI は、リモート・プロシージャー・コール (RPC) システムです。 各呼び出しには、API エンドポイントへのデータの送信と、それと引き換えに構造化データの受信が含まれます。 SLAPI でのデータの送受信に使用される形式は、選択した API の実装によって異なります。 SLAPI は現在、データ伝送に SOAP、XML-RPC、または REST を使用しています。

SLAPI は IBM 仮想ルーター・アプライアンス API を参照するため、SLAPI について詳しくは、SoftLayer Development Network (SLDN) にある次のリソースを参照してください。

- [SoftLayer API Overview![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/article/softlayer-api-overview?cm_mc_uid=71397258840814855542946&cm_mc_sid_50200000=1508536023){: new_window} 
- [SoftLayer_Network_Gateway API![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway){: new_window} 
- [SoftLayer_Network_Gateway_Member API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Member){: new_window} 
- [SoftLayer_Network_Gateway_Vlan API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan){: new_window} 
- [SoftLayer_Network_Gateway_Status API ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Status){: new_window} 
- [SoftLayer API Python Client ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://softlayer-api-python-client.readthedocs.io/en/latest/api/client/){: new_window} 
- [各種言語による SoftLayer API のその他の例![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer.github.io/python/){: new_window} 


### 例 - ゲートウェイ・アプライアンスの注文

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

### 例 - VLAN とゲートウェイ・アプライアンスの関連付け、ルーティング、バイパス、および関連付け解除

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

### ゲートウェイ・アプライアンスのキャンセル例

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
    ｘmembers = getGatewayMembers(client, appliance_id)

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

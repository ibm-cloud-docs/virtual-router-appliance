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

# IBM 虚拟路由器设备的 API 参考
{: #api-reference-for-ibm-virtual-router-appliance}

SoftLayer® 应用程序编程接口 (SLAPI) 是开发者和系统管理员与 IBM© Cloud 后端系统进行直接交互的开发接口。

SLAPI 支持客户门户网站中的许多功能，这通常意味着如果某个交互可在客户门户网站中执行，那么也可以在该 API 中运行。由于在 API 中可以通过编程方式与客户门户网站环境的所有部分进行交互，因此可以使用该 API 来自动执行任务。

SLAPI 是一个远程过程调用 (RPC) 系统。每个调用都涉及向 API 端点发送数据以及接收所返回的结构化数据。通过 SLAPI 发送和接收数据时所使用的格式将取决于您所选择的 API 实现。SLAPI 当前使用 SOAP、XML-RPC 或 REST 进行数据传输。

有关 SLAPI（现在称为“IBM 虚拟路由器设备 API”）的更多信息，请参阅 SoftLayer 开发网络 (SLDN) 中的以下资源：

- [SoftLayer_Network_Gateway API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway){: new_window}
- [SoftLayer_Network_Gateway_Member API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Member){: new_window}
- [SoftLayer_Network_Gateway_Vlan API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Vlan){: new_window}
- [SoftLayer_Network_Gateway_Status API ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://sldn.softlayer.com/reference/services/SoftLayer_Network_Gateway_Status){: new_window}
- [SoftLayer API Python 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://softlayer-api-python-client.readthedocs.io/en/latest/api/client/){: new_window}
- [使用不同语言的更多 SoftLayer API 示例 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer.github.io/python/){: new_window}


### 示例 - 订购网关设备
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
                    {"id", 205533},  # 操作系统的价格
                    {"id", 177619},  # 服务器的价格
                    {"id", 17188},   # RAM 的价格
                    {"id", 63077},   # 磁盘的价格
                    {"id", 52527},   # 网络速度的价格
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

### 示例 - 将 VLAN 关联到网关设备、将 VLAN 路由到网关设备、使 VLAN 绕过网关设备以及解除 VLAN 与网关设备的关联
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
        通过 SoftLayer API 调用根据给定的 vlanNumber
        获取 VLAN 对象（Vlan 对象标识）
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
        通过 SoftLayer API 调用获取内部 VLAN
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

### 取消网关设备的示例

```
import SoftLayer
from pprint import pprint


def getGatewayMembers(client, appliance_id):
    """
        检索网关成员
        自变量：
            client：
                SL 客户机凭证
            appliance_id：
                要回收的网关设备的标识
        返回：
            网关成员
    """
    mask = 'mask[hardwareId]'
    return client['Network_Gateway'].getMembers(id=appliance_id, mask=mask)


def cancelGateway(client, appliance_id):
    """
        取消独立网关或 HA 对网关
        自变量：
            client：
                SL 客户机凭证
            appliance_id：
                要回收的网关设备的标识
        返回：
            如果成功回收了设备的所有成员，那么为 True
            否则为 False
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

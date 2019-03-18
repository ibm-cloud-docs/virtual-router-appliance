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

# VLAN の管理
{: #managing-your-vlans}

[「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面 ](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)からさまざまなアクションを実行できます。

## ゲートウェイ・アプライアンスへの VLAN の関連付け

VLAN を経路指定するには、その前にゲートウェイ・アプライアンスに VLAN を関連付ける必要があります。 VLAN の関連付けは、後に、VLAN をゲートウェイ・アプライアンスに経路指定できるように、適格な VLAN をネットワーク・ゲートウェイにリンクすることです。 関連付けのプロセスにより、VLAN がゲートウェイ・アプライアンスに自動的に経路指定されることはありません。ゲートウェイに経路指定されるまで、VLAN はフロントエンドとバックエンドのお客様のルーターを使用し続けます。 

VLAN は、一度に 1 つのゲートウェイにのみ関連付けられ、ファイアウォールを持つことはできません。 以下の手順に従って VLAN をネットワーク・ゲートウェイに関連付けます。

1. カスタマー・ポータルの[「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面にアクセス](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)します。 
2. **「VLAN の関連付け」**ドロップダウン・リストの中から任意の VLAN を選択します。
3. **「関連付け」**ボタンをクリックして、VLAN を関連付けます。

VLAN をゲートウェイ・アプライアンスに関連付けた後、その VLAN は「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面の「関連付けられた VLAN」セクションに表示されます。 このセクションで、VLAN をゲートウェイに経路指定したり、ゲートウェイから関連付けを解除したりすることができます。 上記のステップを繰り返すことで、適格な VLAN をいつでもゲートウェイ・アプライアンスにさらに関連付けることができます。

## 関連付けられた VLAN の経路指定

関連付けられた VLAN はゲートウェイ・アプライアンスにリンクされますが、 VLAN が経路指定されるまで、VLAN との間の両方向のトラフィックはゲートウェイに到達しません。 関連付けられた VLAN が経路指定された後、フロントエンドとバックエンドのすべてのトラフィックは、お客様のルーターではなく、ゲートウェイ・アプライアンスを通るように経路指定されます。 

関連付けられた VLAN を経路指定するには、次の手順を実行します。

1. カスタマー・ポータルの[「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面にアクセス](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)します。 
2. 「関連付けられた VLAN」セクションで目的の VLANを見つけます。
3. 「アクション」ドロップダウン・メニューから**「VLAN の経路指定」**を選択します。
4. **「はい」**をクリックして、VLAN を経路指定します。 

VLAN を経路指定した後、フロントエンドとバックエンドのすべてのトラフィックは、お客様のルーターからネットワーク・ゲートウェイに移動します。 トラフィックとゲートウェイ・アプライアンス自体に関連した制御をさらに行うには、ゲートウェイの管理ツールにアクセスすることで可能です。 ネットワーク・ゲートウェイを通る経路指定は、[ゲートウェイ・アプライアンスをバイパスする](#bypass-gateway-appliance-routing-for-a-vlan)ことによっていつでも中止できます。

## VLAN のゲートウェイ・アプライアンスの経路指定のバイパス

VLAN が経路指定された後、フロントエンドとバックエンドのすべてのトラフィックは、ネットワーク・ゲートウェイを通って移動します。 ゲートウェイ・アプライアンスは、トラフィックがフロントエンドとバックエンドのお客様のルーター (FCR と BCR) に戻れるように、いつでもバイパスすることができます。 

VLAN をバイパスすると、VLAN をネットワーク・ゲートウェイに関連付けたままにすることができます。 VLAN をゲートウェイ・アプライアンスと関連付ける必要がなくなった場合は、[ゲートウェイ・アプライアンスからの VLAN の関連付け解除](#disassociate-a-vlan-from-a-gateway-appliance)を参照してください。 

以下の手順を実行して、VLAN のゲートウェイ経路指定をバイパスします。

1. カスタマー・ポータルの[「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面にアクセス](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)します。 
2. 「関連付けられた VLAN」セクションで目的の VLANを見つけます。
3. 「アクション」ドロップダウン・メニューから**「VLAN のバイパス」**を選択します。
4. ゲートウェイをバイパスするには、**「はい」**をクリックします。 

ネットワーク・ゲートウェイをバイパスした後、フロントエンドとバックエンドのすべてのトラフィックは、VLAN に関連付けられた FCR と BCR を通過するようになります。 VLAN は、ゲートウェイ・アプライアンスに関連付けされたままになり、いつでもゲートウェイ・アプライアンスに経路を戻すことができます。

## ゲートウェイ・アプライアンスからの VLAN の関連付け解除

VLAN は、[関連付け](#associate-a-vlan-to-a-gateway-appliance)を介して、一度に 1 つのゲートウェイ・アプライアンスにリンクできます。 関連付けにより、VLAN のゲートウェイ・アプライアンスへの経路指定はいつでも可能です。 VLAN を別のゲートウェイ・アプライアンスに関連付ける必要がある場合、または VLAN をそのゲートウェイに関連付ける必要がなくなった場合は、関連付けの解除が必要です。 関連付け解除により、VLAN からゲートウェイ・アプライアンスへの「リンク」が削除され、必要に応じて別のゲートウェイに関連付けることができます。 

以下の手順に従って VLAN からゲートウェイ・アプライアンスへの関連付けを解除します。

1. カスタマー・ポータルの[「ゲートウェイ・アプライアンスの詳細 (Gateway Appliance Details)」画面にアクセス](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-view-vra-details)します。 
2. 「関連付けられた VLAN」セクションで目的の VLANを見つけます。
3. **「アクション」**ドロップダウン・メニューから**「関連付け解除」**を選択します。 
4. **「はい」**をクリックして、VLAN の関連付けを解除します。 

ゲートウェイ・アプライアンスから VLAN を関連付け解除した後、VLAN を別のゲートウェイに関連付けることができます。 また、いつでも、VLAN を元のゲートウェイ・アプライアンスに再び関連付けることができます。 ゲートウェイ・アプライアンスから VLAN の関連付けを解除した後、VLAN のトラフィックをゲートウェイ経由で経路指定することはできません。 VLAN を経路指定するには、その前にゲートウェイ・アプライアンスに関連付ける必要があります。

## 同じネットワーク・インターフェース上で複数の VLAN を経路指定する
仮想ルーター・アプライアンスは、同じネットワーク・インターフェース (例えば、`dp0bond0` または `dp0bond1`) 上に複数の VLAN を経路指定することができます。 これを行うには、スイッチ・ポートをトランク・モードに設定し、デバイス上で仮想インターフェース (VIF) を構成します。

以下に例を示します。 

```
set interfaces bonding dp0bond0 vif 1432 address 10.0.10.1/24
set interfaces bonding dp0bond0 vif 1693 address 10.0.20.1/24
```

上記のコマンドは、`dp0bond0`  インターフェース上に 2 つの仮想インターフェースを作成します。 インターフェース `dp0bond0.1432` は VLAN 1432 のトラフィックを処理し、インターフェース `dp0bond0.1693` は VLAN 1693 のトラフィックを処理します。

## 複数のサブネットを単一の VLAN に追加する

以下の構成例の最後には、パブリック VLAN 用の追加のサンプル・サブネット (`159.8.67.96/28`) が含まれています (1451)。各 VIF (VLAN Interface) のアドレスは、BCR (バックエンド・カスタマー・ルーター) や FCR (フロントエンド・カスタマー・ルーター) では経路指定されません。これは 2 つの Vyattas 間での VRRP/High Availability 通信にのみ使用されます。 

サブネットは、未使用のプライベート IP スペースから選択できます。そのため、ここでは通常は `10.0.0.0/8` が除外されます。以下の例では `192.168.0.0/16` からのサブネットが選択されていますが、`172.16.0.0/12` からのサブネットも使用できます。 

`virtual-address` は、新しいサブネットを構成する場所です。ほとんどの場合、サブネットのゲートウェイ IP アドレスを構成する必要があります。その後、VIF にバインドされたゲートウェイ ID は、VRA の背後にある新しいサブネットにセットアップされたベアメタル・サーバーまたは仮想サーバーのための、次のゲートウェイ・アドレスとして使用されます。 

以下の例は、`159.8.67.97/28` が VIF にバインドされているので、`159.8.67.98/28` サブネットのすべてのトラフィックは VRA によって管理可能であることを示しています。

```
set interfaces bonding dp0bond0 vif 1623 address '192.168.10.2/30'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1623 vrrp vrrp-group 2 virtual-address '10.127.132.129/26'
set interfaces bonding dp0bond0 vif 1750 address '192.168.20.2/30'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond0 vif 1750 vrrp vrrp-group 2 virtual-address '10.126.19.129/26'
set interfaces bonding dp0bond1 vif 788 address '192.168.150.2/30'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 788 vrrp vrrp-group 2 virtual-address '159.8.106.129/28'
set interfaces bonding dp0bond1 vif 1451 address '192.168.200.2/30'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 sync-group 'vgroup2'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.67.97/28'
set interfaces bonding dp0bond1 vif 1451 vrrp vrrp-group 2 virtual-address '159.8.86.49/29'
```

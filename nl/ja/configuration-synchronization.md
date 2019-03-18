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

# 高可用性構成の同期化
{: #synchronizing-high-availability-configurations}

高可用性 (HA) ペアの 2 つの仮想ルーター・アプライアンス (VRA) では、両方のデバイスが同様の動作をするように、構成を十分に同期させる必要があります。 これは、`configuration sync-maps` を使用して行われ、構成のどの部分を同期化するかを選択できます。 1 つのマシンで変更を行うと、マークされた config が他のデバイスにプッシュされます。

**注:** これにより、ローカル・デバイスの実行中の構成が同期化され、リモート・デバイス上に保存されます。 ただし、コミット・プロセスのステップとして、構成はローカル・デバイスに保存されません。 

一方のシステムに固有の構成は、もう一方のシステムに同期化しないでください。 例えば、真の IP アドレスと MAC は同期化されるべきではありません。 `system config-sync` 構成ノード自体と `service https` ノードをすべて同期化することはできません。

以下の例は、config-sync を示しています。

```
set system config-sync sync-map TEST rule 2 action include
set system config-sync sync-map TEST rule 2 location security firewall
set system config-sync remote-router 192.168.1.22 username vyatta
set system config-sync remote-router 192.168.1.22 password xxxxxx
set system config-sync remote-router 192.168.1.22 sync-map TEST
```

最初の 2 行は、実際の sync-map 自体を作成します。 ここで、`security firewall` の構成スタンザは、`sync-map` に設定されます。 結果として、config ノードの内部で行われた変更は、リモート・デバイスにプッシュされます。 ただし、`security user` に対して行われた変更は、ルールに一致しないため送信されません。 `sync-map` は、必要に応じて特定のものまたは一般として行うことができます。

次の 3 行は、リモート・ルーターの `config-sync` ユーザーとパスワード IP、およびプッシュする sync-map を指定します。 `TEST` のルールに一致する変更は、このログイン情報を使用して `remote-router 192.168.1.22` に送られます。 なお、VRA API を使用してこれを実行するために、`REST` 呼び出しが行われます。したがって、HTTPS サーバーがリモート・ルーター上で実行中 (非ブロック化) でなければなりません。

config-sync は、変更をコミットするたびに発生します。 リモート・デバイスからエラー・メッセージが送信されているか注視してください。 構成が同期されていない場合は、再び作動可能になるようにリモート・デバイス上で修正する必要があります。

また、コマンド `show config-sync difference` を使用して構成の相違点を確認することもできます。

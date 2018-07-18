---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Vyatta へのログイン方法

Brocade NFV (Network Functions Virtualization) は、Web GUI を使用するか、SSH ログインで直接アクセスすることができます。

## SoftLayer のプライベート・ネットワークの認証

1. **「詳細」**画面 (**「ネットワーク」>「ゲートウェイ・アプライアンス」**メニュー) からご使用のブラウザーにプライベート IP アドレスを入力し、 https:// の接頭部を付けます (例 https://10.54.207.131­/)。デバイスがパブリック証明書を使用しないことが原因で、証明書エラーを受信する可能性があることにご注意ください。エラーを受け入れて、Web ページに移動します。

2. **vyatta** ユーザーの Web ポータルにある**「デバイス」**メニューで、**「パスワード」**タブから資格情報を入力します。Brocade ダッシュボードに移動します。

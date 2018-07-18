---
copyright:
  years: 1994, 2017
lastupdated: "2017-07-26"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 如何登入 Vyatta

可以使用 Web GUI 或直接透過 SSH 登入來存取 Brocade NFV（網路功能虛擬化）。

## 向 SoftLayer 專用網路進行鑑別

1. 從**詳細資料**畫面（**網路 > 閘道應用裝置**功能表），輸入一個含有 https:// 字首的專用 IP 位址（例如 `https://10.54.207.131/`）到瀏覽器中。請注意，因為裝置不使用公用憑證，所以您可能會接收到憑證錯誤。請接受此錯誤而移至該網頁。

2. 從 Web 入口網站的**裝置**功能表下的**密碼**標籤，為 **vyatta** 使用者輸入認證。您將看到 Brocade 儀表板。

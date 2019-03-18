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

# OS 18.01 版的升級考量

此檔案包含當您將 VRA (Vyatta 5600) 從 Brocade 5.X 作業系統升級至 18.01 作業系統（這包括 Spectre 安全侵害的補救）時，要知道的事項清單。執行此升級時，需要注意幾個問題。

## 誰需要閱讀本主題

* 具有未執行 1801 作業系統之現行 VRA 的任何人。（現行版本是 18.01g）

* 要安裝新的 Virtual Router Appliance 18.01f 版的任何人（例如，從 17.2X 升級的任何人）。

* 如果您有 Vyatta 5400，則此資訊不適用於您。

## 為什麼需要此資訊

VRA 的 1801f 版包含 Spectre 漏洞的安全修正程式，不過，必須先變更安裝程式本身，才能安裝修補程式。需要安裝 1801C 版的中間步驟。

## 一般升級程序
若要升級至 18.01f 版，您必須先下載並安裝 18.01C 修補程式，才能更新 VRA 安裝程式：

1. 從此位置下載 1801c 修補程式，然後遵循[一般程序](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)進行安裝。

2. 重新開機之後，請從此位置下載 18.01f 修補程式，並使用[一般程序](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)進行安裝。

現在，您應該執行已修正 Spectre 安全限制的 18.01f 版。

## 完整重新載入程序
作為替代方案，您也可以在 18.01f 層次執行 VRA 的完整重新載入：

*警告：*此程序可讓您略過下載及安裝兩個修補程式的中間步驟，但在使用 ISO 進行完整重新載入期間，您將遺失所有資料。

1. 從此位置取得 18.01f ISO。
2. 遵循[一般程序](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-upgrading-the-os)來進行安裝，然後重新開機。

現在，您應該執行已修正 Spectre 安全限制的 1801f 版。

## 其他指引

因為 Vyatta 5400 與 Virtual Router Appliance 之間的功能沒有簡單的一對一對映，所以建立 VRA 的基準線配置會有所幫助。IBM© 合作夥伴 WanClouds 可協助您完成此處理程序，並提供有關建立與您 VRA 上 Vyatta 5400 類似之功能的指引。

如需在此升級處理程序中所遇到之一般問題的相關資訊，請參閱[其他文件](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-vyatta-5400-common-migration-issues)。

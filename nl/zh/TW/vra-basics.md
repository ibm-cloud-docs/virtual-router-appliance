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

# VRA 基本觀念
VRA 可以透過 SSH 使用遠端主控台階段作業來配置，或透過登入 Web GUI 來配置。依預設，無法從公用網際網路取得 Web GUI。若要啟用 Web GUI，請先透過 SSH 登入。

**附註：**在 Shell 和介面外配置 VRA 可能會產生非預期的結果，因此不建議這樣做。

## 使用 SSH 存取裝置
大部分 UNIX 型作業系統（例如 Linux、BSD 及 Mac OSX）的預設安裝都會包含 OpenSSH 用戶端。Windows 使用者可以下載 SSH 用戶端，例如 PuTTy。

建議將公用 IP 的 SSH 停用，且只容許專用 IP 的 SSH。與專用 IP 的連線需要您的用戶端連接專用網路。您可以使用下列方式登入：使用客戶入口網站中所提供的其中一個預設 VPN 選項（PPTP、SSL-VPN 及 IPsec），或使用 VRA 上所配置的自訂 VPN 解決方案。

請從**裝置詳細資料**頁面使用 Vyatta 帳戶，透過 SSH 登入。也提供 root 密碼，但基於安全考量，依預設會停用 root 登入。

`ssh vyatta@[IP address] `

**附註：**建議您維持停用 root 登入。請使用 Vyatta 帳戶登入，並在必要時提升至 root。

SSH 金鑰也可以在部署期間提供，以避免需要 Vyatta 帳戶登入。在您驗證能夠使用 SSH 金鑰存取 VRA 之後，可以執行下列指令來停用標準使用者/密碼登入：

```
$ configure
# set service ssh disable-password-authentication
# commit
# save
```

## 使用 Web GUI 存取裝置

使用上述 SSH 指示登入 VRA，然後執行下列指令以啟用 HTTPS 服務：

```
$ configure
# set service https listen-address 169.45.21.2
# commit
# save
```

完成這些指令之後，請在瀏覽器的位址列中輸入 `https://<ip.address>`，並將 IP 位址取代為 VRA 的 IP 位址。可能會要求您接受 VRA 的自簽憑證。請這樣做，然後登入 Web 介面，並在系統提示時使用 Vyatta 認證。

## 模式
**配置模式：**此模式使用 `configure` 指令呼叫，是執行 VRA 系統配置之處。

**作業模式：**登入 VRA 系統時的起始模式。在此模式中，可以執行 `show` 指令，以查詢系統狀態的相關資訊。系統也可以從這個模式重新啟動。

VRA 的 Shell 是一種限制模式介面，具有數種作業模式。主要/預設模式是**作業模式**，且這將是登入時呈現的模式。在**作業**模式中，您可以檢視資訊，並發出會影響系統現行執行的指令，例如，設定日期/時間或重新啟動裝置。

`configure` 指令會讓使用者進入**配置**模式，以便在其中執行配置的編輯。請注意，配置變更不會立即進行；必須加以確定及儲存。輸入指令時，它們會進入配置緩衝區。輸入所有必要的指令之後，請執行 `commit` 指令，讓變更生效。

若要永久地儲存指令，請在 `commit` 指令之後執行 `save` 指令。

透過使用 `run` 來啟動指令，可以從「配置」模式執行作業模式指令。例如：


```
# run show configuration commands | grep name-server
set system name-server '10.0.80.11'
set system name-server '10.0.80.12'
```

井字號 (`#`) 表示配置模式。使用 `run` 作為指令的開頭，會告知 VRA Shell 正在呈現作業指令。上一個範例也說明了可以對指令輸出執行 "grep"。

## 指令探索

VRA 指令 Shell 包含 tab 完成功能。如果您好奇有哪些指令可用，請按 Tab 鍵取得清單及簡要說明。這在 Shell 提示及鍵入指令時都適用。例如：

```
$show log dns [Press tab]
Possible completions:
  dynamic    Show log for Dynamic DNS
  forwarding Show log for DNS Forwarding
```

## 配置範例
配置是以階層式的節點來佈置。請考量這個靜態路由區塊：

```
#show protocols static

protocols {
     static {
         route 10.0.0.0/8 {
             next-hop 10.60.63.193 {
             }
         }
         route 192.168.1.0/24 {
             next-hop 10.0.0.1 {
             }
         }
     }
 }
```

這將由下列指令產生：

```
set protocols static route 10.0.0.0/8 next-hop 10.60.63.193
set protocols static route 192.168.1.0/24 next-hop 10.0.0.1
```

此範例說明節點（靜態）可能有多個子節點。若要移除 `192.168.1.0/24` 的路徑，會使用指令 `delete protocols static route 192.168.1.0/24`。如果指令不包含 `192.168.1.0/24`，則兩個路徑節點都會標示為刪除。

請記得，在發出 `commit` 指令之前，不會實際變更配置。如果要比較現行執行中配置與配置緩衝區中具有的任何變更，請使用 `compare` 指令。如果要清除配置緩衝區，請使用 `discard`。

## 使用者和角色型存取控制 (RBAC)
可以使用三個層次的存取權來配置使用者帳戶：

* 管理者
* 操作員
* 超級使用者

操作員層次的使用者可以執行 `show` 指令，以檢視系統的執行狀態，也可以發出 `reset` 指令，以重新啟動裝置上的服務。操作員層次許可權並不默示唯讀存取。

管理者層次的使用者對裝置的所有配置及作業具有完整存取權。管理使用者可以檢視執行中配置、變更裝置的配置設定、重新啟動裝置，以及關閉裝置。

除了具有管理者層次專用權之外，超級使用者層次的使用者還可以透過 `sudo` 指令，以 root 專用權執行指令。

可以針對密碼及/或公開金鑰鑑別樣式來配置使用者。公開金鑰鑑別與 SSH 一起使用，可讓使用者利用系統上的金鑰檔來登入。若要建立具有密碼的操作員使用者，請執行下列動作：

```
set system login user [account] authentication plaintext-password [password]
set system login user [account] level operator
commit
```

**附註：**如果未指定層次，則會將使用者視為管理者層次。在此情況下，使用者密碼會在配置檔中顯示為已加密。

「角色型存取控制 (RBAC)」是將部分配置的存取限制給已獲授權之使用者的一種方法。RBAC 可讓管理者為使用者群組定義規則，以限制他們可以執行的指令。

要執行 RBAC，可以建立一個指派給「存取控制管理 (ACM)」規則集的群組、將使用者新增至群組、建立規則集以將群組對應到系統中的路徑，然後配置系統以容許或拒絕這些已套用至群組的路徑。

依預設，在 Virtual Router Appliance 中沒有定義 ACM 規則集，且已停用 ACM。如果您要使用 RBAC 來提供精細的存取控制，除了自己的已定義規則之外，您還必須啟用 ACM 並新增下列預設 ACM 規則：

```
set system acm 'enable'
set system acm operational-ruleset rule 9977 action 'allow'
set system acm operational-ruleset rule 9977 command '/show/tech-support/save'
set system acm operational-ruleset rule 9977 group 'vyattaop'
set system acm operational-ruleset rule 9978 action 'deny'
set system acm operational-ruleset rule 9978 command '/show/tech-support/save/*'
set system acm operational-ruleset rule 9978 group 'vyattaop'
set system acm operational-ruleset rule 9979 action 'allow'
set system acm operational-ruleset rule 9979 command '/show/tech-support/save-uncompressed'
set system acm operational-ruleset rule 9979 group 'vyattaop'
set system acm operational-ruleset rule 9980 action 'deny'
set system acm operational-ruleset rule 9980 command '/show/tech-support/save-uncompressed/*'
set system acm operational-ruleset rule 9980 group 'vyattaop'
set system acm operational-ruleset rule 9981 action 'allow'
set system acm operational-ruleset rule 9981 command '/show/tech-support/brief/save'
set system acm operational-ruleset rule 9981 group 'vyattaop'
set system acm operational-ruleset rule 9982 action 'deny'
set system acm operational-ruleset rule 9982 command '/show/tech-support/brief/save/*'
set system acm operational-ruleset rule 9982 group 'vyattaop'
set system acm operational-ruleset rule 9983 action 'allow'
set system acm operational-ruleset rule 9983 command '/show/tech-support/brief/save-uncompressed'
set system acm operational-ruleset rule 9983 group 'vyattaop'
set system acm operational-ruleset rule 9984 action 'deny'
set system acm operational-ruleset rule 9984 command '/show/tech-support/brief/save-uncompressed/*'
set system acm operational-ruleset rule 9984 group 'vyattaop'
set system acm operational-ruleset rule 9985 action 'allow'
set system acm operational-ruleset rule 9985 command '/show/tech-support/brief/'
set system acm operational-ruleset rule 9985 group 'vyattaop'
set system acm operational-ruleset rule 9986 action 'deny'
set system acm operational-ruleset rule 9986 command '/show/tech-support/brief'
set system acm operational-ruleset rule 9986 group 'vyattaop'
set system acm operational-ruleset rule 9987 action 'deny'
set system acm operational-ruleset rule 9987 command '/show/tech-support'
set system acm operational-ruleset rule 9987 group 'vyattaop'
set system acm operational-ruleset rule 9988 action 'deny'
set system acm operational-ruleset rule 9988 command '/show/configuration'
set system acm operational-ruleset rule 9988 group 'vyattaop'
set system acm operational-ruleset rule 9989 action 'allow'
set system acm operational-ruleset rule 9989 command '/clear/*'
set system acm operational-ruleset rule 9989 group 'vyattaop'
set system acm operational-ruleset rule 9990 action 'allow'
set system acm operational-ruleset rule 9990 command '/show/*'
set system acm operational-ruleset rule 9990 group 'vyattaop'
set system acm operational-ruleset rule 9991 action 'allow'
set system acm operational-ruleset rule 9991 command '/monitor/*'
set system acm operational-ruleset rule 9991 group 'vyattaop'
set system acm operational-ruleset rule 9992 action 'allow'
set system acm operational-ruleset rule 9992 command '/ping/*'
set system acm operational-ruleset rule 9992 group 'vyattaop'
set system acm operational-ruleset rule 9993 action 'allow'
set system acm operational-ruleset rule 9993 command '/reset/*'
set system acm operational-ruleset rule 9993 group 'vyattaop'
set system acm operational-ruleset rule 9994 action 'allow'
set system acm operational-ruleset rule 9994 command '/release/*'
set system acm operational-ruleset rule 9994 group 'vyattaop'
set system acm operational-ruleset rule 9995 action 'allow'
set system acm operational-ruleset rule 9995 command '/renew/*'
set system acm operational-ruleset rule 9995 group 'vyattaop'
set system acm operational-ruleset rule 9996 action 'allow'
set system acm operational-ruleset rule 9996 command '/telnet/*'
set system acm operational-ruleset rule 9996 group 'vyattaop'
set system acm operational-ruleset rule 9997 action 'allow'
set system acm operational-ruleset rule 9997 command '/traceroute/*'
set system acm operational-ruleset rule 9997 group 'vyattaop'
set system acm operational-ruleset rule 9998 action 'allow'
set system acm operational-ruleset rule 9998 command '/update/*'
set system acm operational-ruleset rule 9998 group 'vyattaop'
set system acm operational-ruleset rule 9999 action 'deny'
set system acm operational-ruleset rule 9999 command '*'
set system acm operational-ruleset rule 9999 group 'vyattaop'
set system acm ruleset rule 9999 action 'allow'
set system acm ruleset rule 9999 group 'vyattacfg'
set system acm ruleset rule 9999 operation '*'
set system acm ruleset rule 9999 path '*'
```

嘗試啟用 ACM 規則之前，請參閱補充[文件](vra-docs.html)。不正確的 ACM 規則設定可能導致裝置存取拒絕或系統無法作業的錯誤。

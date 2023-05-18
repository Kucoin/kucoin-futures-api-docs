# 基本介紹
## 簡介
歡迎使用KuCoin合約(KuCoin Futures)開發者文檔。 此文檔概述了交易功能、市場行情和其他應用開發接口。


KuCoin Futures API分爲兩部分：**REST API 和 Websocket 實時數據流**

 -  REST API 包含三個類別：**用戶（私有）、交易（私有）、市場數據（公共）**
 -  Websocket包含兩類：**公共頻道和私人頻道**


<!-- <aside class="notice">合約API文檔現已升級，您可以閱讀參考以下鏈接閱讀最新文檔內容: <code>https://docs.kucoin.com/futures/new/cn</code>，目前新文檔相關接口暫未開放使用，上線時間另行通知，如有任何疑問請郵箱聯繫<code>newapi@kucoin.plus</code></aside> -->

## 更新預告

**爲了進一步提升API安全性，KuCoin已經升級到了V2版本的API-KEY，驗籤邏輯也發生了一些變化，建議到[API管理頁面](https://futures.kucoin.com/api)添加並更換到新的API-KEY。KuCoin將繼續支持使用老的API-KEY到2021年05月01日。請查看“消息簽名”，瞭解更多詳情**

#### 2023.04.02

* 【修改】`GET /api/v1/transfer-list`接口，新增入參字段，queryStatus、queryStatus、pageSize
* 【修改】`GET /api/v1/orders`接口，新增入參字段，currentPage、pageSize
* 【修改】`GET /api/v1/stopOrders`接口，新增入參字段，currentPage、pageSize
* 【修改】`GET /api/v1/fills`接口，新增入參字段：currentPage、pageSize，新增出參字段：openFeePay、closeFeePay
* 【修改】`GET /api/v1/recentFills`接口，新增入參字段：currentPage、pageSize，新增出參字段：openFeePay、closeFeePay
* 【修改】`GET /api/v1/position`接口，新增出參字段：userId
* 【修改】`GET /api/v1/positions`接口，新增出參字段：userId
* 【修改】`POST /api/v1/position/margin/deposit-margin`接口，新增出參字段：userId
* 【修改】`GET /api/v1/premium/query`接口，刪除出參字段：predictedValue
* 【修改】`GET /api/v1/recentDoneOrders`接口，新增入參字段：symbol
* 【修改】`GET /api/v1/positions`接口，新增入參字段：currency
* 【廢棄】`GET /api/v1/deposit-address`请参考现货接口
* 【廢棄】`GET /api/v1/deposit-list`请参考现货接口
* 【廢棄】`GET /api/v1/withdrawals/quotas`请参考现货接口
* 【廢棄】`GET /api/v1/withdrawal-list`请参考现货接口
* 【廢棄】`GET /api/v1/withdrawals`请参考现货接口
* 【廢棄】`GET /api/v1/transfer-out`
* 【廢棄】`DELETE /api/v1/cancel/transfer-out`


#### 2022.11.01

* 【廢棄】`DELETE /api/v1/withdrawals/{withdrawalId}`取消提現接口

#### 2022.10.18

* 【新增】子賬號相關接口: `GET /api/v1/sub/api-key`、`POST /api/v1/sub/api-key/update`、`DELETE /api/v1/sub/api-key`

#### 2022.09.22

* 【新增】子賬號相關接口: `POST /api/v1/sub/api-key`

#### 2022.07.21
* 【廢棄】`POST /api/v1/withdrawals`接口

<!-- #### 2022.04.30
* 合約API文檔已升級，最新地址請參考：<code>https://docs.kucoin.com/futures/new/cn</code> -->

#### 2022.03.24
* 【廢棄】了`GET /api/v1/level2/message/query`接口
* 【新增】接口返回值描述
* 【新增】`POST /api/v3/transfer-out`接口
* 【新增】`POST /api/v1/transfer-in`接口

#### 2022.02.07
* 【新增】`GET /api/v1/position`接口返回字段：maintainMargin、riskLimitLevel.


## 客戶端開發庫

使用客戶端開發庫可快速集成到KuCoin Futures API。

**官方軟件開發工具包（SDK）**

- [PHP SDK](https://github.com/Kucoin/kucoin-futures-php-sdk)

- [Java SDK](https://github.com/Kucoin/kucoin-futures-java-sdk)

- [Node.js SDK](https://github.com/Kucoin/kucoin-futures-node-sdk)

- [Python SDK](https://github.com/Kucoin/kucoin-futures-python-sdk)

- [Go SDK](https://github.com/Kucoin/kucoin-futures-go-sdk)

  

## 沙盒測試環境
沙盒是測試環境，用於測試API連接和Web交易，並提供交易的所有功能。在沙盒中，您可以使用虛假資金來測試交易功能。
沙盒環境中的登錄會話和API密鑰與生產環境完全分離。您可以使用沙盒環境中的Web界面來創建API密鑰。

**注意:**在沙盒環境中註冊後，您將收到系統在您的帳戶中自動充值的一定數量的測試資金（XBT）。如果您想交易，請將資產從儲蓄賬戶轉移到交易賬戶。這些資金僅用於測試目的，不能提現。

**用於API測試的沙盒URL：**
* 網址：https://sandbox-futures.kucoin.com 
* REST API 連接地址: **https://api-sandbox-futures.kucoin.com** (https://sandbox-api.kumex.com has been Deprecated)


## 請求頻率限制

### REST API

對需要校驗API權限的私有接口，限制賬號userid。不需要檢驗權限API，則限制IP。目前Kucoin一共有三種限頻，分別如下

1、error code：1015，根據IP限制頻率，是cloudflare基於ip的限制，所有的接口共用該限頻，目前是500/10s,後臺可能會微調，block 30s。Cloudfeare沒有ip白名單的配置，所以無法特殊調整，但是這個問題是可以避免的，比如使用Websocket接口代替Rest接口(如果接口支持的話)。也可以用一臺伺服器綁定多個ip地址（ipv4或者ipv6），或者不同的子帳號使用不同的ip。

2、error code：200002，kucoin每個私有接口的限頻，是基於用戶的uid+介面模式的限制，block10s。比如某個接口調用頻率過高，就可能遇到這個問題，建議降低那個接口的使用頻率

3、error code：429000，kucoin單機容量限制。可以理解為伺服器超載了。

<aside class="notice">接口有特定請求頻率限制說明，以特定說明爲準。</aside>

### WEBSOCKET

**連接數量**
- 每個用戶ID同時建立的連接數：**≤ 50個**

**連接次數**

 - 連接請求次數限制：**每分鐘 30次 請求**

**上行消息條數** 

 - 向服務器發送指令條數限制：**每10秒 100條**

**訂閱topic數量** 

 - 每個連接最大可訂閱topic數量限制：**100個topics**

## 做市激勵計劃
KuCoin Futures爲專業做市商提供做市激勵計劃。 參與該計劃，可以獲得以下激勵：

 - 做市商返傭
 - 每月嘉獎前十名錶現優良的做市團隊，高額回饋最佳做市商
 - 直接市場接入（提供private link接入）

具有良好做市策略和大交易量的用戶歡迎參與此長期做市激勵計劃。如果您的賬戶在過去30天內的交易量超過5000 BTC，請提供以下信息以發送電子郵件至**mm@kucoin.com**，郵件主題爲“Futures Market Maker Application”：

 - 提供平臺帳戶（需要電子郵件，無需推薦關係）
 - 過去30天內在任何交易所交易的交易量證明或VIP級別的證明
 - 請簡要說明做市的方法（不需要詳細說明）以及估算做市訂單量的百分比。

## VIP快速通道
具有良好做市策略和大交易量的用戶歡迎參與長期做市商計劃。 如果您的帳戶資產大於10BTC，請提供以下信息以發送電子郵件至：**vip_futures@kucoin.com**進行做市商申請;

 - 提供平臺帳戶（需要電子郵件，無需推薦關係）;
 - 提供其他交易平臺做市商交易量的截圖（例如30天內的交易量，或VIP級別等）; 
 - 請簡要說明做市的方法，不需要詳細說明；

---
# REST API
## 請求說明

### API服務器地址

REST API對用戶、交易及市場數據均提供了接口。

基本URL： **https://api-futures.kucoin.com** (**https://api.kumex.com** 已過期不推薦使用)

<aside class="notice">爲了遵守當地法律要求，使用中國IP的用戶不允許訪問以上URL。</aside>

請求URL由基本URL和指定接口端點組成。


## 接口端點

每個接口都提供了對應的端點，可在**HTTP請求**模塊下獲取。


對於**GET請求**，只需將請求參數拼接在請求路徑後面。

例如：對於“倉位” 接口，其默認端點爲/api/v1/position。請求“合約”參數（XBTUSDM）時，該端點將變爲：**/api/v1/position?symbol=XBTUSDM**。因此，您最終請求的URL應爲：**https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM**。

## 請求
所有的請求和響應的內容類型都是application/json。  


除非另行說明，所有的時間戳參數均以Unix時間戳毫秒計算。如：**1544657947759**

## 參數

對於**GET**和**DELETE**請求，需將參數拼接在請求URL中（如：**/api/v1/position?symbol=XBTUSDM**）。

對於**POST**和**PUT**請求，需將參數以JSON格式拼接在請求主體中（如： {"side":"buy"}）。

**注意：不要在JSON字符串中添加空格。**


### 錯誤返回

系統會返回HTTP錯誤代碼或系統錯誤代碼。您可根據返回的參數消息排查錯誤原因。


#### HTTP錯誤碼

```json
  {
    "code": "400100",
    "msg": "Invalid Parameter."
  }
```

代碼 | 含義
---------- | -------
400 | Bad Request -- 請求格式不正確
401 | Unauthorized -- 無效API Key
403 | Forbidden -- 請求被禁止
404 | Not Found -- 找不到指定資源
405 | Method Not Allowed -- 您請求資源的方法不正確
415 | Content-Type -- 請求類型必須爲application/json類型
429 | Too Many Requests -- 請求頻率超出限制
500 | Internal Server Error -- 服務器出錯，請稍後再試
503 | Service Unavailable -- 服務器維護中，請稍後再試



#### 系統錯誤碼

代碼 | 含義
---------- | -------
1015 | cloudflare frequency limit according to IP, block 30s--超頻錯誤:基於ip的cloudflare限制,觸發限制時間30s
40010 | Unavailable to place orders. Your identity information/IP/phone number shows you're at a country/region that is restricted from this service. -- 您當前屬於限制國家，委託無法生效。
100001 | There are invalid parameters -- 無效參數
100002 | systemConfigError -- 系統配置有誤
100003 | Contract parameter invalid -- 沒有此合約
100004 | Order is in not cancelable state -- 訂單不存在或者訂單不可撤銷
100005 | contractRiskLimitNotExist -- 合約風險限額不存在
200001 | The query scope for Level 2 cannot exceed xxx -- level2查詢範圍必須小於等於xxx　
200002 | Too many requests in a short period of time, please retry later--超頻錯誤:基於接口業務層面的限制, 觸發限制時間10s
200002 | The query scope for Level 3 cannot exceed xxx -- level3查詢範圍必須小於等於xxx
200003 | The symbol parameter is invalid. -- 參數symbol無效
300000 | request parameter illegal -- 請求參數非法
300001 | Active order quantity limit exceeded (limit: xxx, current: xxx) -- 超過活動委託數量限制，最大可提交 xxx 個活動委託，當前活動委託數: xxx
300002 | Order placement/cancellation suspended, please try again later. -- 系統暫停下單/撤單操作，將自動恢復，請稍後再試
300003 | Balance not enough, please first deposit at least 2 USDT before you start the battle -- 可用餘額不足，委託成本爲 xxx
300004 | Stop order quantity limit exceeded (limit: xxx, current: xxx) -- 超過條件委託數量限制，最大可提交 xxx 個條件委託，當前條件委託數: xxx
300005 | xxx risk limit exceeded -- 委託會導致超過最大風險限額 xxx
300006 | The close price shall be greater than the bankruptcy price. Current bankruptcy price: xxx. -- 平倉委託價格不能劣於倉位的破產價格，當前破產價格：xxx
300007 | priceWorseThanLiquidationPrice -- 加倉委託價格不能劣於倉位的強平價格，當前強平價格：xxx
300008 | Unavailable to place the order, there's no contra order in the market. -- 市場無訂單，市價單提交失敗
300009 | Current position size: 0, unable to close the position. -- 當前倉位爲0，無法平倉
300010 | Failed to close the position -- 平倉失敗
300011 | Order price cannot be higher than xxx -- 委託價格不能高於xxx
300012 | Order price cannot be lower than xxx -- 委託價格不能低於xxx
300013 | Unable to proceed the operation, there's no contra order in order book. -- 委託訂單沒有合法可撮合價格，訂單提交失敗
300014 | The position is being liquidated, unable to place/cancel the order. Please try again later. -- 強制平倉中，系統暫停下單/撤單，將自動恢復，請稍後重試
300015 | The order placing/cancellation is currently not available. The Contract/Funding is under the settlement process. When the process is completed, the function will be restored automatically. Please wait patiently and try again later. -- 合約交割/資金費用結算中，系統暫停下單/撤單，將自動恢復，請稍後重試
300016 | The leverage cannot be greater than xxx. -- 槓桿數不能大於xxx
300017 | Unavailable to proceed the operation, this position is for Futures Brawl -- 該倉位爲亂鬥倉位，不支持操作
300018 | clientOid parameter repeated -- clientOid參數重複
400001 | Any of KC-API-KEY, KC-API-SIGN, KC-API-TIMESTAMP, KC-API-PASSPHRASE is missing in your request header -- 請求頭中缺少KC-API-KEY、KC-API-SIGN、KC-API-TIMESTAMP和KC-API-PASSPHRASE參數
400002 | KC-API-TIMESTAMP Invalid -- 請求時間與服務器時差超過5秒
400003 | KC-API-KEY not exists -- API-KEY不存在
400004 | KC-API-PASSPHRASE error -- API-PASSPHRAE不正確
400005 | Signature error -- 簽名錯誤，請檢查您的簽名
400006 | The requested ip address is not in the api whitelist -- 請求IP不在API白名單中
400007 | Access Denied -- API權限不足，無法訪問該URI目標地址。
400100 | Parameter Error -- 請求參數不合法
404000 | Url Not Found -- 找不到請求資源
411100 | User are frozen -- 用戶已被凍結，請聯繫幫助中心
415000 | Unsupported Media Type -- 請求頭等Content-Type需要設置成application/json
429000 | Too Many Requests -- 超頻錯誤:基於接口的全站流量限制，可以直接重試請求
500000 | Internal Server Error -- 服務器出錯，請稍後再試

系統返回的HTTP狀態碼不是200時，接口返回會顯示錯誤碼。調用成功時，系統將返回code和data字段；調用失敗時，系統將返回code和msg字段。您可根據返回的參數消息排查錯誤。



### 成功返回

當系統返回**HTTP狀態碼200和系統代碼200000**時，表示響應成功，返回結果如下：

```json
  {
    "code": "200000",
  }
```


### 分頁器

KuCoin Futures 使用了**Pagination**和**HasMore**兩種分頁器，來返回數組的REST請求。


#### Pagination
```json
  {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 6,
      "totalPage": 1,
      "data": ...
  }
```

**Pagination**允許使用當前頁數獲取結果，非常適用於獲取實時數據。如/api/v1/deposit-list、/api/v1/orders及/api/v1/fills端點均默認返回第一頁結果。如需獲取更多數據，請根據當前返回的數據指定其他分頁，然後再進行請求。

**示例** 
GET /api/v1/orders?currentPage=1&pageSize=50

**參數**

參數 | 默認值 | 含義
---------- | ------- | ------
currentPage | 1 | 當前頁碼
pageSize | 50 | 每頁數據條數



#### HasMore
HasMore分頁器採用滑動窗口機制，通過在一個數據流上滑動固定大小的窗口以獲取分頁數據，返回的數據中有一個字段HasMore，標誌是否還有更多的數據。HasMore分頁器每次滑動的時間相等，性能高效，非常適合於流式實時數據的查詢。


**參數**

| 參數 | 數據類型    | 含義                                |
| --------- | ------- | --------------- |
| offset    | -       | 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁
| forward   | boolean | 滑動方向，TRUE查詢大於offset方向的數據      
| maxCount  | int     | 每次滑動的最大數據條數                    

**示例**
GET /api/v1/interest/query?symbol=.XBTINT&offset=1558079160000&forward=true&maxCount=10

## 類型說明 
### 時間戳 
API中的所有時間戳以Unix時間戳毫秒爲單位返回（如：**546658861000**）

## 接口認證
### 創建API KEY
通過接口進行請求前，您需在Web端創建API-KEY。創建成功後，您需妥善保管好以下三條信息：


* Key
* Secret
* Passphrase

Key和Secret由KuCoin Futures隨機生成並提供，Passphrase是您在創建API時使用的密碼。以上信息若遺失將無法恢復，需要重新申請API KEY。


### 權限

您可在[KuCoin Futures](https://futures.kucoin.com) Web端管理API權限。API權限分爲以下幾類：


* **通用權限** - 開啓後，API可使用大部分GET請求來獲取數據。
* **交易權限** - 開啓後，可通過API進行下單/撤單和管理倉位。
* **轉賬權限** - 開啓後，可通過API進行提現。需注意，開啓此權限後，不需要兩步驗證，即可進行轉賬。


### 創建請求

所有REST參數的請求頭需包含以下內容：

* **KC-API-KEY** API KEY是字符串類型。
* **KC-API-SIGN** 簽名（請查看“消息簽名”，瞭解更多詳情）
* **KC-API-TIMESTAMP** 請求的時間戳
* **KC-API-PASSPHRASE** 創建API時填寫的API密碼
* **KC-API-KEY-VERSION** API-KEY版本號，可通過[API管理](https://futures.kucoin.com/api)頁面查看版本號


### 消息簽名

```php
    <?php
    class API {
        public function __construct($key, $secret, $passphrase) {
          $this->key = $key;
          $this->secret = $secret;
          $this->passphrase = $passphrase;
        }

        public function signature($request_path = '', $body = '', $timestamp = false, $method = 'GET') {
          
          $body = is_array($body) ? json_encode($body) : $body; // Body must be in json format
          
          $timestamp = $timestamp ? $timestamp : time() * 1000;

          $what = $timestamp . $method . $request_path . $body;

          return base64_encode(hash_hmac("sha256", $what, $this->secret, true));
        }
    }
    ?> 
```

```python
    #Example for get user position in python
    
    api_key = "api_key"
    api_secret = "api_secret"
    api_passphrase = "api_passphrase"
    url = 'https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM'
    now = int(time.time() * 1000)
    str_to_sign = str(now) + 'GET' + '/api/v1/position?symbol=XBTUSDM'
    signature = base64.b64encode(
        hmac.new(api_secret.encode('utf-8'), str_to_sign.encode('utf-8'), hashlib.sha256).digest())
    passphrase = base64.b64encode(hmac.new(api_secret.encode('utf-8'), api_passphrase.encode('utf-8'), hashlib.sha256).digest())    
    headers = {
        "KC-API-SIGN": signature,
        "KC-API-TIMESTAMP": str(now),
        "KC-API-KEY": api_key,
        "KC-API-PASSPHRASE": passphrase
        "KC-API-KEY-VERSION": "2"
    }
    response = requests.request('get', url, headers=headers)
    print(response.status_code)
    print(response.json())
    
    #Example for create deposit addresses in python
    url = 'https://api-futures.kucoin.com/api/v1/deposit-address'
    now = int(time.time() * 1000)
    data = {"currency": "XBT"}
    data_json = json.dumps(data)
    str_to_sign = str(now) + 'POST' + '/api/v1/deposit-address' + data_json
    signature = base64.b64encode(
        hmac.new(api_secret.encode('utf-8'), str_to_sign.encode('utf-8'), hashlib.sha256).digest())
    passphrase = base64.b64encode(hmac.new(api_secret.encode('utf-8'), api_passphrase.encode('utf-8'), hashlib.sha256).digest())
    headers = {
        "KC-API-SIGN": signature,
        "KC-API-TIMESTAMP": str(now),
        "KC-API-KEY": api_key,
        "KC-API-PASSPHRASE": passphrase,
        "KC-API-KEY-VERSION": "2",
        "Content-Type": "application/json" # specifying content type or using json=data in request
    }
    response = requests.request('post', url, headers=headers, data=data_json)
    print(response.status_code)
    print(response.json())
```
請求頭中的 **KC-API-SIGN**: 

1. 使用 API-Secret 對
{timestamp + method+ endpoint + body} 拼接的字符串進行**HMAC-sha256**加密。
2. 再將加密內容使用 **base64** 編碼。
                       

請求頭中的 **KC-API-PASSPHRASE**:

1. 對於V1版的API-KEY，請使用明文傳遞
2. 對於V2版的API-KEY，需要將KC-API-KEY-VERSION指定爲2，並將passphrase使用API-Secret進行**HMAC-sha256**加密，再將加密內容通過**base64**編碼後傳遞

注意：

* 加密的 timestamp 需要和請求頭中的KC-API-TIMESTAMP保持一致
* 進行加密的body需要和Request Body中的內容保持一致
* 請求方法需要大寫
* 對於 GET, DELETE 請求，endpoint 需要包含請求的參數（/api/v1/deposit-address?currency=XBT）。如果沒有請求體（通常用於GET請求），請使用空字符串“”表示請求體


```python
#Example for Create Deposit Address

curl -H "Content-Type:application/json" -H "KC-API-KEY:5c2db93503aa674c74a31734" -H "KC-API-TIMESTAMP:1547015186532" -H "KC-API-PASSPHRASE:QWIxMjM0NTY3OCkoKiZeJSQjQA==" -H "KC-API-SIGN:7QP/oM0ykidMdrfNEUmng8eZjg/ZvPafjIqmxiVfYu4=" -H "KC-API-KEY-VERSION:2" 
-X POST -d '{"currency":"XBT"}' http://api-futures.kucoin.com/api/v1/deposit-address

KC-API-KEY = 5c2db93503aa674c74a31734
KC-API-SECRET = f03a5284-5c39-4aaa-9b20-dea10bdcf8e3
KC-API-PASSPHRASE = QWIxMjM0NTY3OCkoKiZeJSQjQA==
KC-API-KEY-VERSION = 2
TIMESTAMP = 1547015186532
METHOD = POST
ENDPOINT = /api/v1/deposit-address
STRING-TO-SIGN = 1547015186532POST/api/v1/deposit-address{"currency":"XBT"}
KC-API-SIGN = 7QP/oM0ykidMdrfNEUmng8eZjg/ZvPafjIqmxiVfYu4=
```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

----------

### 選擇時間戳

請求頭的**KC-API-TIMESTAMP**必須爲Unix UTC時間，需精確到毫秒（如：1547015186532）。

服務器請求的時間戳與API服務器時差必須控制在**5秒**以內，否則請求會因過期而被服務器拒絕。如果服務器與API服務器之間存在時間偏差，請使用平臺提供的服務器時間接口，獲取API服務器的時間。

---
# 用戶
此部分需進行簽名驗證。

# 賬戶
## 獲取賬戶概覽
```json
  { 
    "code": "200000",
    "data": {
      "accountEquity": 99.8999305281, //賬戶總權益 = 賬戶餘額 + 未實現盈虧
      "unrealisedPNL": 0, //未實現盈虧
      "marginBalance": 99.8999305281, //賬戶餘額 = 倉位保證金 + 委託保證金 + 轉出提現凍結 + 可用餘額 - 未實現盈虧
      "positionMargin": 0, //倉位保證金
      "orderMargin": 0, //委託保證金
      "frozenFunds": 0, //轉出提現凍結
      "availableBalance": 99.8999305281, //可用餘額
      "currency":"XBT" //幣種
    }
  }
```
### HTTP請求
`GET /api/v1/account-overview`

### 請求示例
`GET /api/v1/account-overview?currency=XBT`

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
currency | String | [可選] 幣種 **XBT或USDT,默認XBT**

### 返回值
字段 | 含義
--------- | -------
| accountEquity | 賬戶總權益 = 賬戶餘額 + 未實現盈虧 | 
| unrealisedPNL | 未實現盈虧 | 
| marginBalance | 賬戶餘額 = 倉位保證金 + 委託保證金 + 轉出提現凍結 + 可用餘額 - 未實現盈虧 | 
| positionMargin | 倉位保證金 | 
| orderMargin | 委託保證金 | 
| frozenFunds | 轉出提現凍結 | 
| availableBalance | 可用餘額 | 
| currency | 幣種 | 

### API權限
該接口需要**通用權限**

### 頻率限制
此接口針對每個賬號請求頻率限制爲**30次/3s**

## 查詢資金記錄

```json
  {
    "code": "200000",
    "data": {
      "hasMore": false,//是否存在下一頁
      "dataList": [{
        "time": 1558596284040, //業務發生時間
        "type": "RealisedPNL", //類型
        "amount": 0, //交易金額
        "fee": null,//手續費
        "accountEquity": 8060.7899305281, //賬戶權益
        "status": "Pending", //狀態，當前8h有過未平倉合約
        "remark": "XBTUSDM",//合約
        "offset": -1, //起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁
        "currnecy": "XBT" //幣種
      },
      {
        "time": 1557997200000,
        "type": "RealisedPNL",
        "amount": -0.000017105,
        "fee": 0,
        "accountEquity": 8060.7899305281,
        "status": "Completed",//狀態，已完成的8h週期
        "remark": "XBTUSDM",
        "offset": 1,
        "currency": "XBT"
     }]
    }
  }

```
當存在未平倉合約時，第一頁返回數據包含的未平倉合約記錄狀態爲**Pending**，是當前8小時結算週期內的已實現盈虧。翻頁使用當前頁返回的最小**offset**，作爲請求**offset**參數值傳入，實現翻頁。

### HTTP請求
`GET /api/v1/transaction-history`

### 請求示例
`GET /api/v1/transaction-history?offset=1&forward=true&maxCount=50`

### API權限
該接口需要**通用權限**

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
startAt| long | [可選] 開始時間（毫秒）
endAt| long | [可選]  截止時間（毫秒） 
type | String | [可選] 類型 **RealisedPNL-已實現盈虧；Deposit-充值,Withdrawal-提現；TransferIn-轉入；TransferOut-轉出**
offset | long | [可選] 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁
maxCount | long | [可選] 每頁顯示條數 默認50
currency | String | [可選] 幣種 **XBT或USDT**
forward | boolean | [可選] 是否前向查詢，**true**或者**false**，默認爲**true** 

<aside class="notice">針對startAt、endAt的補充說明：startAt必須小於endAt；並且間隔不能超過1天；允許只傳一個字段，如果只傳一個字段時，另有一個字段系統會自動進行加減1天補齊</aside>


### 返回值
字段 | 含義
--------- | -------
| time | 業務發生時間 | 
| type | 類型包括 **RealisedPNL-已實現盈虧,Deposit-充值,Withdrawal-提現,TransferIn-轉入 ,TransferOut-轉出** | 
| amount | 交易金額 | 
| fee | 手續費 | 
| accountEquity | 賬戶權益 | 
| status | 狀態包括 **Completed，Pending** | 
| remark | 合約 | 
| offset | 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁 | 
| currnecy | 幣種 | 

## 獲取⼦賬號合約API列表
```json
{
    "code": "200000",
    "data": [
        {
            "subName": "AAAAAAAAAAAAA0022",
            "remark": "hytest01-01",
            "apiKey": "63032453e75087000182982b",
            "permission": "General",
            "ipWhitelist": "",
            "createdAt": 1661150291000
        }
    ]
}
```
這個接⼝⽤以獲取⼦賬號合約API列表
### HTTP請求
`GET /api/v1/sub/api-key`
### 請求示例
`GET /api/v1/sub/api-key`
### API權限
此接⼝需要`通⽤權限`。
### 請求參數
請求參數 | 類型 | 是否必須 | 含義
--------- | ------- | ------- | -------
apiKey | String | 否 | API-Key
subName | String | 是 | ⼦賬號名
### 返回值
字段 | 含義
--------- | -------
apiKey | API-Key
createdAt | 創建時間
ipWhitelist | IP⽩名單
permission | 權限列表
remark | 備註
subName | ⼦賬號名

## 創建子賬號合約API
```json
{
    "code": "200000",
    "data": {
        "subName": "AAAAAAAAAA0007",
        "remark": "remark",
        "apiKey": "630325e0e750870001829864",
        "apiSecret": "110f31fc-61c5-4baf-a29f-3f19a62bbf5d",
        "passphrase": "passphrase",
        "permission": "General",
        "ipWhitelist": "",
        "createdAt": 1661150688000
    }
}
```
這個接口用以創建子賬號合約API
### HTTP請求
`POST /api/v1/sub/api-key`
### 請求示例
`POST /api/v1/sub/api-key`
### API權限
此接口需要`通用權限`。
### 請求參數
請求參數 | 類型 | 是否必須 |  含義
--------- | ------- | ------- | -------
subName | String | 是 | 子賬號名, 創建api key的子賬號名稱
passphrase | String | 是 | 密碼(7～32位字符，不可輸入空格)
remark | String | 是 | 備註(1～24位字符)
permission | String | 否 | 權限列表(只能設置General、Trade權限，如："General,Trade”。默認爲General)
ipWhitelist | String | 否 | IP白名單(每個IP用半角逗號隔開，最多添加20個)
expire | String | 否 | API過期時間；不過期(默認值)`-1`，30天`30`，90天`90`，180天`180`，360天`360`

### 返回值
字段 | 含義
--------- | -------
apiKey | API-Key
createdAt | 創建時間
ipWhitelist | IP白名單
permission | 權限列表
remark | 備註
subName | 子賬號名
apiSecret | 祕鑰
passphrase | 密碼

## 修改⼦賬號合約API
```json
{
    "code": "200000",
    "data": {
        "subName": "AAAAAAAAAA0007",
        "apiKey": "630329b4e7508700018298c5",
        "permission": "General",
        "ipWhitelist": "127.0.0.1"
    }
}
```
這個接⼝⽤以修改⼦賬號合約API
### HTTP請求
`POST /api/v1/sub/api-key/update`
### 請求示例
`POST /api/v1/sub/api-key/update`
### API權限
此接⼝需要`通⽤權限`。
### 請求參數
請求參數 | 類型 | 是否必須 | 含義
--------- | ------- | ------- | -------
subName | String |  是 | 子賬號名(API Key對應子賬號名)
apiKey | String | 是 | API-Key(需要修改的API Key)
passphrase | String | 是 |  密碼(API Key 密碼)
permission | String |  否 |  權限列表(只能設置General、Trade權限，默認爲General。如果修改，權限將會重置)
ipWhitelist | String | 否 | IP白名單(每個IP用半角逗號隔開，最多添加20個。如果修改，ip將會重置)
expire | String | 否 | API過期時間；不過期(默認值)`-1`，30天`30`，90天`90`，180天`180`，360天`360`

### 返回值
字段 | 含義
--------- | -------
apiKey | API-Key
ipWhitelist | IP⽩名單
permission | 權限列表
subName | ⼦賬號名

## 刪除⼦賬號合約API
```json
{
    "code": "200000",
    "data": {
        "subName": "AAAAAAAAAA0007",
        "apiKey": "630325e0e750870001829864"
    }
}
```
這個接⼝⽤以刪除⼦賬號合約API
### HTTP請求
`DELETE /api/v1/sub/api-key`
### 請求示例
`DELETE /api/v1/sub/api-key`
### API權限
此接⼝需要`通⽤權限`。
### 請求參數
請求參數 | 類型 | 是否必須 | 含義
--------- | ------- | ------- | -------
apiKey | String | 是 | API-Key(要刪除的api key)
passphrase | String | 是 | 密碼(api key的密碼)
subName | String | 是 | ⼦賬號名(api key對應⼦賬號名)
### 返回值
字段 | 含義
--------- | -------
apiKey | API-Key
subName | ⼦賬號名



# 劃轉


## 轉出到KuCoin儲蓄賬戶
```json
{
  "applyId": "620a0bbefeaa6a000110e833",//轉出申請id
  "bizNo": "620a0bbefeaa6a000110e832",//業務編號
  "payAccountType": "CONTRACT",//付款賬戶類型
  "payTag": "DEFAULT",//付款賬戶子類型
  "remark": "",//用戶備註
  "recAccountType": "MAIN",//收款賬戶類型
  "recTag": "DEFAULT",//收款賬戶子類型
  "recRemark": "",//收款賬戶流水備註
  "recSystem": "KUCOIN",//收款服務方
  "status": "PROCESSING",//狀態
  "currency": "USDT",//幣種
  "amount": "0.001",//劃轉金額
  "fee": "0",//劃轉手續費
  "sn": 889048787670001,//序列號
  "reason": "",//失敗原因
  "createdAt": 1644825534000,//創建時間
  "updatedAt": 1644825534000//更新時間
}
```
 轉出金額將從KuCoin Futures賬戶扣除，轉出前請確保KuCoin Futures賬戶可用餘額充足。接口響應表示轉出成功後，系統將返回applyId。此ID可用於取消轉出申請。

### HTTP請求
`POST /api/v2/transfer-out` （推薦使用 POST /api/v3/transfer-out）
### 請求示例
`POST /api/v2/transfer-out`
### API權限
該接口需要**交易權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
amount | Number | 轉出金額，最大不能超過1000000000
currency | String | 幣種 **XBT,USDT**

### 返回值
參數  | 含義
--------- | -----------
applyId  | 轉出申請id
bizNo  | 業務編號
payAccountType  | 付款賬戶類型
payTag  | 付款賬戶子類型
remark  | 用戶備註
recAccountType  | 收款賬戶類型
recTag  | 收款賬戶子類型
recRemark  | 收款賬戶流水備註
recSystem  | 收款服務方
status  | 狀態：APPLY-待處理，PROCESSING-處理中，PENDING_APPROVAL-待審覈，APPROVED-審覈通過，REJECTED-審覈拒絕，PENDING_CANCEL-待退款，CANCEL-取消，SUCCESS-成功
currency  | 幣種
amount  | 劃轉金額
fee  | 劃轉手續費
sn  | 序列號
reason  | 失敗原因
createdAt  | 創建時間
updatedAt  | 更新時間

## 轉出到KuCoin儲蓄/幣幣賬戶
```json
{
  "applyId": "620a0bbefeaa6a000110e833",//轉出申請id
  "bizNo": "620a0bbefeaa6a000110e832",//業務編號
  "payAccountType": "CONTRACT",//付款賬戶類型
  "payTag": "DEFAULT",//付款賬戶子類型
  "remark": "",//用戶備註
  "recAccountType": "MAIN",//收款賬戶類型
  "recTag": "DEFAULT",//收款賬戶子類型
  "recRemark": "",//收款賬戶流水備註
  "recSystem": "KUCOIN",//收款服務方
  "status": "PROCESSING",//狀態
  "currency": "USDT",//幣種
  "amount": "0.001",//劃轉金額
  "fee": "0",//劃轉手續費
  "sn": 889048787670001,//序列號
  "reason": "",//失敗原因
  "createdAt": 1644825534000,//創建時間
  "updatedAt": 1644825534000//更新時間
}
```
 轉出金額將從KuCoin Futures賬戶扣除，轉出前請確保KuCoin Futures賬戶可用餘額充足。接口響應表示轉出成功後，系統將返回applyId。此ID可用於取消轉出申請。

### HTTP請求
`POST /api/v3/transfer-out`
### 請求示例
`POST /api/v3/transfer-out`
### API權限
該接口需要**交易權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
amount | Number | 轉出金額，最大不能超過1000000000
currency | String | 幣種 **XBT,USDT**
recAccountType | String | 收款賬戶，只能是*MAIN*-儲蓄賬戶、*TRADE*-幣幣賬戶

### 返回值
參數  | 含義
--------- | -----------
applyId  | 轉出申請id
bizNo  | 業務編號
payAccountType  | 付款賬戶類型
payTag  | 付款賬戶子類型
remark  | 用戶備註
recAccountType  | 收款賬戶類型
recTag  | 收款賬戶子類型
recRemark  | 收款賬戶流水備註
recSystem  | 收款服務方
status  | 狀態：APPLY-待處理，PROCESSING-處理中，PENDING_APPROVAL-待審覈，APPROVED-審覈通過，REJECTED-審覈拒絕，PENDING_CANCEL-待退款，CANCEL-取消，SUCCESS-成功
currency  | 幣種
amount  | 劃轉金額
fee  | 劃轉手續費
sn  | 序列號
reason  | 失敗原因
createdAt  | 創建時間
updatedAt  | 更新時間

## 資金轉入合約賬戶
```json
{
  "code": "200", //code爲200代表轉入成功，否則代表失敗
  "msg": "",
  "retry": true,
  "success": true
}
```
從KuCoin付款賬戶(目前支持MAIN-儲蓄賬戶、TRADE-幣幣賬戶)轉入金額到合約賬戶，轉出前請確保付款賬戶可用餘額充足。

### HTTP請求
`POST /api/v1/transfer-in`
### 請求示例
`POST /api/v1/transfer-in`
### API權限
該接口需要**交易權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
amount | Number | 轉出金額
currency | String | 幣種 **XBT,USDT**
payAccountType | String | 付款賬戶，只能是*MAIN*-儲蓄賬戶、*TRADE*-幣幣賬戶

## 查詢轉出申請記錄
```json
{
  "currentPage": 1,
  "pageSize": 50,
  "totalNum": 1,
  "totalPage": 1,
  "items": [
    {
      "applyId": "620a0bbefeaa6a000110e833",//轉出申請id
      "currency": "USDT",//幣種
      "recRemark": "",//收款賬戶流水備註
      "recSystem": "KUCOIN",//收款服務方
      "status": "SUCCESS",//狀態 PROCESSING-處理中，SUCCESS-成功, FAILURE-失敗
      "amount": "0.001",//交易金額
      "reason": "",//失敗原因
      "offset": 889048787670001,//起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁
      "createdAt": 1644825534000,//提交申請時間
      "remark": ""//用戶備註
    }
  ]
}
```
默認查詢第一頁數據
### HTTP請求
`GET /api/v1/transfer-list`

### 請求示例
`GET /api/v1/transfer-list?currentPage=1&pageSize=50&status=PROCESSING?currency=XBT`

### API權限
該接口需要**通用權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
startAt | long | [可選] 開始時間（毫秒）
endAt | long | [可選]  截止時間（毫秒） 
status | String | [可選] 狀態 **PROCESSING-處理中，SUCCESS-成功, FAILURE-失敗**
queryStatus| List | [可選] 狀態集合 **PROCESSING-處理中，SUCCESS-成功, FAILURE-失敗**
currency | String | [可選]  幣種 **XBT,USDT**
currentPage | long | [可選] 頁數，不傳默認1
pageSize | long | [可選] 頁碼，不傳默認50


<aside class="notice">針對startAt、endAt的補充說明：startAt必須小於endAt；並且間隔不能超過1天；允許只傳一個字段，如果只傳一個字段時，另有一個字段系統會自動進行加減1天補齊</aside>



### 返回值
參數  | 含義
--------- | -----------
applyId  | 轉出申請id
currency  | 幣種
recRemark  | 收款賬戶流水備註
recSystem  | 收款服務方
status  | 狀態 PROCESSING-處理中，SUCCESS-成功, FAILURE-失敗
amount  | 交易金額
reason  | 失敗原因
offset  | 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁
createdAt  | 提交申請時間
remark  | 用戶備註

## 取消轉出（廢棄）

```json
 { 
    "code": "200000",
    "data":null
  }
```

只能在**PROCESSING**狀態時可取消轉出請求。
### HTTP請求
`DELETE /api/v1/cancel/transfer-out`

### 請求示例
`DELETE /api/v1/cancel/transfer-out?applyId=5cd53be30c19fc3754b60928`

### API權限
該接口需要**通用權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
applyId | String | 轉出申請ID(發起轉出返回)


# 交易
此部分需進行簽名驗證。

# 訂單
## 下單

```json
  {
    "code": "200000",
    "data": {
      "orderId": "5bd6e9286d99522a52e458de"
      }
  }
```

有兩種訂單類型：限價單和市價單。只有當賬戶資金充足時，下單纔會成功。下單後，系統將根據指定訂單類型和參數，凍結相應資金，直至訂單完結。


注意：系統將在訂單進入買賣盤前凍結手續費用。查看[成交記錄](#4cee70a191)，瞭解更多詳情。


**請勿在JSON字符串中添加空格。**

### 下單限制：

一個賬戶中，每種合約最多可下**100**個限價單和**50**個止損單。



### HTTP 請求

`POST /api/v1/orders`

### API權限
該接口需獲取**交易權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**30次/3s**

### 參數

| 參數     | 數據類型   | 含義                                                  |
| --------- | ------ | -----------------------|
| clientOid | String | 唯一的訂單ID，可用於識別訂單，長度最大不超過40。如：UUID<br/>只能包含數字、字母、下劃線（_）或 分隔線（-）|
| side      | String | **buy** 或 **sell**    |
| symbol    | String | 有效合約代碼。如：XBTUSDM                  |
| type      | String | [可選] 訂單類型，包括**limit**或**market**,默認limit。|
| leverage  | String | 下單槓桿倍數
| remark    | String | [可選] 下單備註，字符長度不能超過100 個字符（UTF-8）。|
| stop      | String | [可選] 觸發價格的兩種類型。下跌至某個價格（**down**），或上漲至某個價格（**up**）。設置後，就必須設置**stopPrice**和**stopPriceType** 參數。 |
| stopPriceType  | String | [可選] 止損單觸發價類型，包括**TP**、**IP**和**MP**， 只要設置**stop**參數，就必須設置此屬性。|
| stopPrice | String | [可選] 只要設置**stop**參數，就必須設置此屬性。|
| reduceOnly | boolean | [可選] 只減倉標記, 默認值是 false 。值爲true時，需要設置平倉數量|
| closeOrder | boolean | [可選] 平倉單標記, 默認值是 false 。值爲true時，代表倉位全平|
| forceHold | boolean | [可選] 強制凍結標記（減倉同樣適用）,可將訂單留在買賣盤中而不受倉位變化的影響。默認值是 false |

參數詳細描述見術語解釋。



#### 限價單需額外請求的參數


| 參數       | 數據類型    | 含義    |
| ----------- | ------- | ----------------------------- |
| price       | String  | 限價單的價格   |
| size        | Integer  | 訂單數量。必須是一個正數。 |
| timeInForce | String  | [可選] 訂單時效策略，包括GTC、IOC（默認爲GTC）。 |
| postOnly    | boolean | [可選] 只掛單的標識。選擇postOnly，不允許選擇hidden和iceberg。當訂單時效爲IOC策略時，該參數無效。  |
| hidden      | boolean | [可選] 訂單不會在買賣盤中展示。選擇hidden，不允許選擇postOnly。|
| iceberg    | boolean | [可選] 僅設置可見的部分會顯示在買賣盤中。選擇iceberg，不允許選擇postOnly。|
| visibleSize | Integer  | [可選] 冰山單最大可展示的數量。  |

#### 市價單需額外請求的參數


| 參數 | 數據類型 | 含義
| --------- | ------- | -----------
| size | Integer | [可選] 下單數量

### 返回值
參數  | 含義
--------- | -----------
orderId  | 訂單id

### 示例
`POST /api/v1/orders`

```json
  {
    "clientOid": "5c52e11203aa677f33e493fb",
    "reduceOnly": false,
    "closeOrder": false,
    "forceHold": false,
    "hidden": false,
    "iceberg": false,
    "leverage": 20,
    "postOnly": false,
    "price": 8000,
    "remark": "remark",
    "side": "buy",
    "size": 20,
    "stop": "",
    "stopPrice": 0,
    "stopPriceType": "",
    "symbol": "XBTUSDM",
    "timeInForce": "",
    "type": "limit",
    "visibleSize": 0
  }
```

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

### 術語解釋

### 合約

合約必須是KuCoin Futures支持的合約，如XBTUSDM。

### 用戶訂單ID

ClientOid字段是客戶端創建的唯一的ID（推薦使用UUID），只能包含數字、字母、下劃線（_） 和 分隔線（-）。這個字段會在獲取訂單信息時返回。您可使用clientOid來標識您的訂單。ClientOid不同於服務端創建的訂單ID。請不要使用同一個clientOid發起請求。**clientOid最長不得超過40個字符**。

請妥善記錄服務端創建的orderId，以用於查詢訂單狀態的更新。


### TYPE
您在下單時指定的訂單類型，決定了您是否需要請求其他參數，同時還會影響到撮合引擎的執行。如果您在下單時未指定訂單類型，系統將默認按照限價單執行。

下限價單時，您需指定限價單的價格（**price**）和數量（**size**）。系統將根據市場行情以指定或更優價格撮合該訂單。如果訂單未能被立即撮合，將繼續留買賣盤中，直至被撮合或被用戶取消。

與限價單不同，市價單價格會隨着市場價格波動而變化。下市價單時，您無需指定價格，只需指定合約數量。市價單會立即成交，不會進入買賣盤。所有市價單都是taker，需支付taker費用。



###  LEVERAGE

槓桿表示訂單凍結保證金時使用的槓桿倍數，如果是平倉單可以不設置此屬性。 

### STOP ORDERS
止損單，是指當市場價格達到了設置的止損觸發價格（stopPrice）觸發合約後，訂單按照市場價格或指定價格買進或賣出相應數量的合約。止損單分爲兩種，向下止損（down）和向上止損（up）。


**止損單類型**

向下止損（**down**）：當價格下跌至或低於設置的止損價格（stopPrice）時，訂單將被觸發。

向上止損（**up**）：當價格上漲至或高於設置的止損價格（stopPrice）時，訂單將被觸發。

**止損單觸發價類型**：

1. 最新交易價格（TP）
2. 標記價格（MP）
3. 指數價格（IP）

最新交易價格，指最近一次的訂單成交價格，該價格可在最新撮合消息中找到。

**mark price** 和 **index price** 可以通過相關指數服務OPEN API獲取。

請注意，當止損單被觸發後，訂單將按照指定訂單類型，以市價單或限價單成交。

進行止盈止損交易時，系統不會提前凍結您的賬戶資金。訂單被觸發後，如果賬戶餘額不足時，訂單會被自動取消。


### PRICE

下單價格必須是合約tickSize的整數倍，否則下單時會報錯。tickSize是合約價格的最小精度單位。合約價格不能超過合約最高價格（maxPrice）規定。**KuCoin Futures平臺採用了IEPR價格保護機制（Immediately Executable Price Range, 簡稱爲IEPR）**，合約購買價格不得超過**1.05*標記價格**的價格上限，合約賣出價格不得低於**0.95 *標記價格**的價格下限。

下市價單不需要價格字段。



### 訂單數量

訂單數量是合約的張數, 訂單數量不能小於合約最小數量（lotSize）或大於合約最大數量（maxOrderQty）。訂單數量必須是lotSize的整數倍，否則下單時系統會報錯。訂單數量表示要買入或賣出的合約數量大小。每張 XBTUSDTM 合約對應 0.001 BTC, 每張 XBTUSDM 合約對應 1 USD.


### 訂單時效

訂單時效是一種交易時使用的特殊策略，用於設定訂單在被撮合或取消前的生效時間。訂單時效策略分爲兩種：

1. 取消前有效（Good Till Canceled，簡稱爲GTC）
2. 立即成交或取消（Immediate Or Cancel，簡稱爲IOC）

**取消前有效（GTC）**：指委託將持續有效直到被手動取消。如果用戶在交易時沒有設置該字段，系統將默認按照GTC策略執行訂單。

**立即成交或取消（IOC）**：指如果委託全部成交或部分成交，未成交部分將被立即取消。


**注意：成交也包含自成交**

### 被動委託

選擇被動委託的訂單，增加了市場的流動性，將按照maker費用執行。

### 隱藏單&冰山單

您可在高級設置中設置隱藏單和冰山單（冰山單是一種特殊形式的隱藏單）。進行限價單和限價止損單交易時，您可選擇按照隱藏單或冰山單執行。

隱藏單不會展示在買賣盤上。

與隱藏單不同，冰山單分爲可見和隱藏兩部分。進行冰山單交易時，需設置可見訂單數量。冰山單最小可見數量是總訂單量的1/20。

進行撮合時，冰山單的可見部分會首先被撮合，當可見部分被全部撮合後，另一部分隱藏訂單將浮出，直至訂單全部成交。

**注意：**

1. 系統將對隱藏和冰山單收取taker費用。
2. 如果您同時設定了冰山單和隱藏單，您的訂單將默認作爲冰山單處理。


### 凍結訂單

下單時，系統會根據訂單的價格和數量凍結一定的賬戶金額，作爲倉位保證金和交易費。沒有凍結的訂單不能進行加倉撮合, 只能進行平倉撮合。當撮合引擎發現訂單零凍結數量超過反向倉位數量時, 會取消多餘零凍結訂單, 以保證沒有零凍結訂單加倉撮合。

交易實際產生的手續費會在訂單成交時確定。如果您取消了一個未完全成交的委託，則其相應的剩餘的已凍結資金會被釋放到可用餘額中。

訂單的加倉數量需要凍結，平倉數量不需要凍結，平倉單（closeOrder is true）和只減倉訂單（reduceOnly is true）不需要凍結。


### CLOSE ORDER 平倉
平倉單會把當前用戶的所有倉位修改爲 0。平倉單標記爲 true 時，不需要傳入買賣方向和訂單數量參數，系統會根據用戶當前倉位的方向和數量動態決定訂單的買賣方向和訂單數量。由於減倉不需要凍結，所以有也不需要傳入槓桿參數。

### REDUCE ONLY 只減倉
被標記爲只減倉的訂單隻會被以減倉的方向撮合（不會增加倉位），當用戶倉位數量小於只減倉訂單數量時，多餘數量的訂單數量會被撮合引擎取消掉。

### FORCE HOLD 強制凍結
強制凍結訂單費用。下當前倉位的反向訂單時，會凍結相應資金以確保訂單不會因爲沒有足夠的資金而被撮合引擎自動取消。


### 訂單生命週期

當下單請求因請求成功（撮合引擎已收到訂單）或（因餘額不足、參數不合法等原因）被拒絕時，HTTP 請求會進行響應。下單成功，返回訂單ID，訂單將被撮合，可能會部分或全部成交。部分成交後，訂單剩餘爲未成交部分會變成等待撮合（**Active**）狀態（不包括使用立即成交或取消[**IOC**]的訂單）。已完全成交的訂單會變成“已完成”（**Done**）狀態。

訂閱市場數據頻道的用戶可使用訂單ID（orderId）和用戶訂單ID（clientOid）來識別消息。


### 響應

下單成功後會返回一個訂單ID（order id）。下單成功即表示撮合引擎已收到訂單。

**未被撮合的訂單將繼續留在買賣盤上直至被完全撮合或取消。**


## 單個撤單

```json
  {
    "code": "200000",
    "data": {
      "cancelledOrderIds": [
        "5bd6e9286d99522a52e458de"
      ]
    }
  }
```

提交以下請求，可撤銷單個訂單（包括止損單）。


系統收到取消訂單的請求後，將向您返回結果。撮合引擎將按照訂單順序處理取消訂單的申請。您可檢查訂單狀態或更新推送消息以瞭解您的請求是否已被處理。

返回訂單ID爲服務端創建的訂單ID，而非客戶端創建的ID（**clientOid**）。

如果訂單無法被取消（因已成交、已被取消等原因），系統將報錯進行錯誤響應，在返回字段中顯示錯誤原因。


### HTTP 請求
`DELETE /api/v1/orders/{order-id}`

### 示例
`DELETE /api/v1/orders/5cdfc120b21023a909e5ad52`

### API權限 ###
該接口需獲取**交易權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**40次/3s**

### 返回值
參數  | 含義
--------- | -----------
cancelledOrderIds  | 取消的訂單ID

## 限價單批量撤單

```json
  {
    "code": "200000",
    "data": {
      "cancelledOrderIds": [
        "5c52e11203aa677f33e493fb",
        "5c52e12103aa677f33e493fe",
        "5c6265c503aa676fee84129c",
        "5c6269e503aa676fee84129f",
        "5c626b0803aa676fee8412a2"
      ]
    } 
  }
```

該接口可用於取消全部當前委託（不包括止損單）。返回結果將列出被取消訂單的訂單ID（orderID）。


### HTTP請求
`DELETE /api/v1/orders`

### 示例
`DELETE /api/v1/orders?symbol=XBTUSDM`

### API權限
該接口需獲取**交易權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 參數

參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String | [可選] 刪除指定合約的所有限價單。

使用查詢參數可刪除指定合約的全部訂單。如果沒有指定symbol參數，將取消全部限價單。

### 返回值
參數  | 含義
--------- | -----------
cancelledOrderIds  | 取消的訂單ID

## 止損單批量撤單

```json
  {
    "code": "200000",
    "data": {
      "cancelledOrderIds": [
        "5c52e11203aa677f33e49311",
        "5c52e12103aa677f33e49322"
      ]
    }
  }
```

刪除所有未被觸發的止損單。請求成功後，系統將返回已被取消的止損單的訂單ID列表。取消已觸發止損單，請使用"限價單批量撤單"接口。



### HTTP請求
`DELETE /api/v1/stopOrders`

### 示例
`DELETE /api/v1/stopOrders?symbol=XBTUSDM`


### API權限
該接口需獲取**交易權限**。

### 參數

參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String | [可選] 取消指定合約的所有未觸發止損單。

使用查詢參數可刪除指定合約的全部訂單。如果沒有指定symbol參數，將取消全部止損單。

### 返回值
參數  | 含義
--------- | -----------
cancelledOrderIds  | 取消的訂單ID

## 查詢訂單列表

```json
  {
   "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 100,
      "totalNum": 1000,
      "totalPage": 10,
      "items": [
        {
            "id": "5cdfc138b21023a909e5ad55", //訂單編號
            "symbol": "XBTUSDM",  //合約編號
            "type": "limit",   //類型, 市價單或限價單
            "side": "buy",  //買賣方向
            "price": "3600",  //下單價格
            "size": 20000,  //數量
            "value": "56.1167227833",  //訂單價值
            "dealValue": "56.1167227833",  //成交額
            "dealSize": 20000,//成交數量
            "stp": "",  //stp 類型
            "stop": "",  //止損訂單類型
            "stopPriceType": "",  //止損訂單觸發價格類型
            "stopTriggered": true,  //止損訂單是否觸發標誌
            "stopPrice": null,  //止損訂單觸發價格
            "timeInForce": "GTC",  //timeInForce類型
            "postOnly": false,  //postOnly標誌
            "hidden": false,  //隱藏單標誌
            "iceberg": false,  //冰山單標誌 
            "leverage": "20",  //槓桿倍數
            "forceHold": false,  //強制凍結單標誌
            "closeOrder": false, //平倉單標誌
            "visibleSize": null,  //冰山單可見數量
            "clientOid": "5ce24c16b210233c36ee321d",  //客戶訂單編號
            "remark": null,  //註解
            "tags": null,//訂單標籤
            "isActive": false,  //未完成訂單標誌
            "cancelExist": false,  //訂單存在取消數量標誌
            "createdAt": 1558167872000,  //創建時間
            "updatedAt": 1558167872000, //最新更新時間
            "endAt": 1558167872000,//截止時間
            "orderTime": 1558167872000000000, //下單時間納秒
            "settleCurrency": "XBT", //結算幣種
            "status": "done", //訂單狀態: “open” 或 “done”
            "filledValue": "56.1167227833",  //已經成交訂單價值
            "filledSize": 20000,  //已經成交訂單數量
            "reduceOnly": false  //只減倉標記
          }
      ]
    }  
 }
```

結果返回當前所有委託。

### HTTP請求
`GET /api/v1/orders`

### 示例
`GET /api/v1/orders?status=active`
獲取所有活動訂單  

### API權限
該接口需獲取**通用權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**30次/3s**

### 參數

參數 | 數據類型 | 含義
--------- | ------- | -----------
status | String |[可選] 訂單狀態。活躍（**active**）狀態或已完成單（**done**）狀態。默認設置爲“已完成”狀態。請求發送成功後，僅返回指定狀態的委託列表。
symbol |String|[可選] 僅返回指定的委託列表，如：XBTUSDM。
side | String | [可選] **buy** 或 **sell** 
type | String | [可選] 訂單類型，包括：限價單、市價單、限價止損、市價止損。**limit**, **market**, **limit_stop** or **market_stop** 
startAt | long | [可選] 開始時間（毫秒）
endAt | long | [[可選]  截止時間（毫秒）
currentPage | long | [可選] 頁數，不傳默認1
pageSize | long | [可選] 頁碼，不傳默認50，最大不能超過1000

### 返回值
參數  | 含義
--------- | -----------
id | 訂單編號
symbol | 合約編號
type | 類型, 市價單或限價單
side | 買賣方向
price | 下單價格
size | 數量
value | 訂單價值
dealValue | 成交額
dealSize | 成交數量
stp | stp 類型
stop | 止損訂單類型
stopPriceType | 止損訂單觸發價格類型
stopTriggered | 止損訂單是否觸發標誌
stopPrice | 止損訂單觸發價格
timeInForce | timeInForce類型
postOnly | postOnly標誌
hidden | 隱藏單標誌
iceberg | 冰山單標誌
leverage | 槓桿倍數
forceHold | 強制凍結單標誌
closeOrder | 平倉單標誌
visibleSize | 冰山單可見數量
clientOid | 客戶訂單編號
remark | 註解
tags | 訂單標籤
isActive | 未完成訂單標誌
cancelExist | 訂單存在取消數量標誌
createdAt | 創建時間
updatedAt | 最新更新時間
endAt | 截止時間
orderTime | 下單時間納秒
settleCurrency | 結算幣種
status | 訂單狀態: “open” 或 “done”
filledSize | 已經成交訂單價值
filledValue | 已經成交訂單數量
reduceOnly | 只減倉標記

請使用查詢參數獲取指定合約的訂單。

請求返回數據使用了**Pagination**分頁方式。


#### 訂單狀態和結算
在買賣盤上，所有限價委託都處於活躍（**Active**）狀態，從買賣盤上移除的訂單則被標記爲已完成（**Done**）狀態。訂單被成交後到入賬，因系統清算可能會有毫秒級別的延遲。


您可發送請求，查詢任一狀態的訂單。如果您未指定狀態參數，系統將默認返回“已完結”（**Done**）狀態的訂單。

查詢“活躍”狀態的訂單，沒有時間限制。但查詢“已完成”狀態的訂單時，您只能獲取 7 * 24 小時時間範圍內的數據（即：查詢時，開始時間到結束時間的時間範圍不能超過24 * 7小時）。若超出時間範圍，系統會報錯。如果您只指定了結束時間，沒有指定開始時間，系統將按照 24小時的範圍自動計算開始時間（開始時間=結束時間-24小時）並返回相應數據，反之亦然。


#### POLLING 輪詢
對於高頻交易的用戶，建議您在本地緩存和維護一份自己的活動委託列表，並使用市場數據流實時更新自己的訂單信息。

如果需要低延時獲取自己的最近成交歷史訂單記錄, 請使用“24小時內完成訂單列表”小節中的接口（Get List of Orders Completed in 24H）。 此接口返回的歷史訂單可能存在一定的延遲。


## 查詢未觸發止損訂單列表

```json
  {
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 100,
      "totalNum": 1000,
      "totalPage": 10,
      "items":  [
        {
          "id": "622076e79f12700001f84138",
          "symbol": "XBTUSDTM",
          "type": "limit",
          "side": "sell",
          "price": "32000",
          "size": 2,
          "value": "0",
          "dealValue": "0",
          "dealSize": 0,
          "stp": "",
          "stop": "down",
          "stopPriceType": "TP",
          "stopTriggered": null,
          "stopPrice": "3000",
          "timeInForce": "GTC",
          "postOnly": false,
          "hidden": false,
          "iceberg": false,
          "leverage": "20",
          "forceHold": false,
          "closeOrder": false,
          "visibleSize": null,
          "clientOid": null,
          "remark": null,
          "tags": null,
          "isActive": true,
          "cancelExist": false,
          "createdAt": 1646294759000,
          "updatedAt": 1646294759000,
          "endAt": null,
          "orderTime": null,
          "settleCurrency": "USDT",
          "status": "open",
          "filledValue": "0",
          "filledSize": 0,
          "reduceOnly": false
        }
      ]
    }
 }
```
您可通過該接口查詢未觸發止損訂單列表。已經觸發的止損訂單通過一般訂單接口查詢。

### HTTP請求
`GET /api/v1/stopOrders`

### 示例
`GET /api/v1/stopOrders?symbol=XBTUSDM`
發送該請求可獲取XBTUSDM合約的未觸發止損訂單

### API權限
該接口需獲取**通用權限**。

### 參數

參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol |String|[可選] 僅返回指定的委託列表，如：XBTUSDM。
side | String | [可選]**buy** 或 **sell** 
type | String | [可選]**limit** 或 **market** 
startAt | long | [可選] 開始時間（毫秒）
endAt | long | [可選]  截止時間（毫秒）
currentPage | long | [可選] 頁數，不傳默認1
pageSize | long | [可選] 頁碼，不傳默認50，最大不能超過1000

請使用查詢參數獲取指定合約的訂單。

**請求返回數據使用了Pagination分頁方式。**

### 返回值
參數  | 含義
--------- | -----------
id | 訂單編號
symbol | 合約編號
type | 類型, 市價單或限價單
side | 買賣方向
price | 下單價格
size | 數量
value | 訂單價值
dealValue | 成交額
dealSize | 成交數量
stp | stp 類型
stop | 止損訂單類型
stopPriceType | 止損訂單觸發價格類型
stopTriggered | 止損訂單是否觸發標誌
stopPrice | 止損訂單觸發價格
timeInForce | timeInForce類型
postOnly | postOnly標誌
hidden | 隱藏單標誌
iceberg | 冰山單標誌
leverage | 槓桿倍數
forceHold | 強制凍結單標誌
closeOrder | 平倉單標誌
visibleSize | 冰山單可見數量
clientOid | 客戶訂單編號
remark | 註解
tags | 訂單標籤
isActive | 未完成訂單標誌
cancelExist | 訂單存在取消數量標誌
createdAt | 創建時間
updatedAt | 最新更新時間
endAt | 截止時間
orderTime | 下單時間納秒
settleCurrency | 結算幣種
status | 訂單狀態: “open” 或 “done”
filledSize | 已經成交訂單價值
filledValue | 已經成交訂單數量
reduceOnly | 只減倉標記



## 24小時內完成訂單列表

```json
  {
    "code": "200000",
    "data": [
          {
            "id": "5cdfc138b21023a909e5ad55", //訂單編號
            "symbol": "XBTUSDM",  //合約編號
            "type": "limit",   //類型, 市價單或限價單
            "side": "buy",  //買賣方向
            "price": "3600",  //下單價格
            "size": 20000,  //數量
            "value": "56.1167227833",  //訂單價值
            "dealValue": "56.1167227833",  //成交額
            "dealSize": 20000,//成交數量
            "stp": "",  //stp 類型
            "stop": "",  //止損訂單類型
            "stopPriceType": "",  //止損訂單觸發價格類型
            "stopTriggered": true,  //止損訂單是否觸發標誌
            "stopPrice": null,  //止損訂單觸發價格
            "timeInForce": "GTC",  //timeInForce類型
            "postOnly": false,  //postOnly標誌
            "hidden": false,  //隱藏單標誌
            "iceberg": false,  //冰山單標誌 
            "leverage": "20",  //槓桿倍數
            "forceHold": false,  //強制凍結單標誌
            "closeOrder": false, //平倉單標誌
            "visibleSize": null,  //冰山單可見數量
            "clientOid": "5ce24c16b210233c36ee321d",  //客戶訂單編號
            "remark": null,  //註解
            "tags": null,//訂單標籤
            "isActive": false,  //未完成訂單標誌
            "cancelExist": false,  //訂單存在取消數量標誌
            "createdAt": 1558167872000,  //創建時間
            "updatedAt": 1558167872000, //最新更新時間
            "endAt": 1558167872000,//截止時間
            "orderTime": 1558167872000000000, //下單時間納秒
            "settleCurrency": "XBT", //結算幣種
            "status": "done", //訂單狀態: “open” 或 “done”
            "filledValue": "56.1167227833",  //已經成交訂單價值
            "filledSize": 20000,  //已經成交訂單數量
            "reduceOnly": false  //只減倉標記
          }
    ]
 }
```

如果需要低延時獲取自己的最近成交歷史訂單, 請使用此接口。 使用此接口可獲取過去24小時內最近完成的1000筆訂單。

### HTTP請求
`GET /api/v1/recentDoneOrders`

### 示例
`GET /api/v1/recentDoneOrders`

### API權限
該接口需獲取**通用權限**。

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String |[可選] 合約symbol 

### 返回值
參數  | 含義
--------- | -----------
id | 訂單編號
symbol | 合約編號
type | 類型, 市價單或限價單
side | 買賣方向
price | 下單價格
size | 數量
value | 訂單價值
dealValue | 成交額
dealSize | 成交數量
stp | stp 類型
stop | 止損訂單類型
stopPriceType | 止損訂單觸發價格類型
stopTriggered | 止損訂單是否觸發標誌
stopPrice | 止損訂單觸發價格
timeInForce | timeInForce類型
postOnly | postOnly標誌
hidden | 隱藏單標誌
iceberg | 冰山單標誌
leverage | 槓桿倍數
forceHold | 強制凍結單標誌
closeOrder | 平倉單標誌
visibleSize | 冰山單可見數量
clientOid | 客戶訂單編號
remark | 註解
tags | 訂單標籤
isActive | 未完成訂單標誌
cancelExist | 訂單存在取消數量標誌
createdAt | 創建時間
updatedAt | 最新更新時間
endAt | 截止時間
orderTime | 下單時間納秒
settleCurrency | 結算幣種
status | 訂單狀態: “open” 或 “done”
filledSize | 已經成交訂單價值
filledValue | 已經成交訂單數量
reduceOnly | 只減倉標記

## 單個訂單詳情

```json
  {
    "code": "200000",
    "data": {
            "id": "5cdfc138b21023a909e5ad55", //訂單編號
            "symbol": "XBTUSDM",  //合約編號
            "type": "limit",   //類型, 市價單或限價單
            "side": "buy",  //買賣方向
            "price": "3600",  //下單價格
            "size": 20000,  //數量
            "value": "56.1167227833",  //訂單價值
            "dealValue": "56.1167227833",  //成交額
            "dealSize": 20000,//成交數量
            "stp": "",  //stp 類型
            "stop": "",  //止損訂單類型
            "stopPriceType": "",  //止損訂單觸發價格類型
            "stopTriggered": true,  //止損訂單是否觸發標誌
            "stopPrice": null,  //止損訂單觸發價格
            "timeInForce": "GTC",  //timeInForce類型
            "postOnly": false,  //postOnly標誌
            "hidden": false,  //隱藏單標誌
            "iceberg": false,  //冰山單標誌 
            "leverage": "20",  //槓桿倍數
            "forceHold": false,  //強制凍結單標誌
            "closeOrder": false, //平倉單標誌
            "visibleSize": null,  //冰山單可見數量
            "clientOid": "5ce24c16b210233c36ee321d",  //客戶訂單編號
            "remark": null,  //註解
            "tags": null,//訂單標籤
            "isActive": false,  //未完成訂單標誌
            "cancelExist": false,  //訂單存在取消數量標誌
            "createdAt": 1558167872000,  //創建時間
            "updatedAt": 1558167872000, //最新更新時間
            "endAt": 1558167872000,//截止時間
            "orderTime": 1558167872000000000, //下單時間納秒
            "settleCurrency": "XBT", //結算幣種
            "status": "done", //訂單狀態: “open” 或 “done”
            "filledValue": "56.1167227833",  //已經成交訂單價值
            "filledSize": 20000,  //已經成交訂單數量
            "reduceOnly": false  //只減倉標記
          }
}
```
您可通過訂單號獲取單個訂單的詳情（包括止損單）。


### HTTP請求
`GET /api/v1/orders/{order-id}?clientOid={client-order-id}`

### 示例
`GET /api/v1/orders/5cdfc138b21023a909e5ad55` (通過 orderId 獲取訂單信息), </br>
`GET /api/v1/orders/byClientOid?clientOid=eresc138b21023a909e5ad59` (通過用戶傳入的訂單id查詢訂單信息)


### API權限
該接口需獲取**通用權限**。

### 返回值
參數  | 含義
--------- | -----------
id | 訂單編號
symbol | 合約編號
type | 類型, 市價單或限價單
side | 買賣方向
price | 下單價格
size | 數量
value | 訂單價值
dealValue | 成交額
dealSize | 成交數量
stp | stp 類型
stop | 止損訂單類型
stopPriceType | 止損訂單觸發價格類型
stopTriggered | 止損訂單是否觸發標誌
stopPrice | 止損訂單觸發價格
timeInForce | timeInForce類型
postOnly | postOnly標誌
hidden | 隱藏單標誌
iceberg | 冰山單標誌
leverage | 槓桿倍數
forceHold | 強制凍結單標誌
closeOrder | 平倉單標誌
visibleSize | 冰山單可見數量
clientOid | 客戶訂單編號
remark | 註解
tags | 訂單標籤
isActive | 未完成訂單標誌
cancelExist | 訂單存在取消數量標誌
createdAt | 創建時間
updatedAt | 最新更新時間
endAt | 截止時間
orderTime | 下單時間納秒
settleCurrency | 結算幣種
status | 訂單狀態: “open” 或 “done”
filledSize | 已經成交訂單價值
filledValue | 已經成交訂單數量
reduceOnly | 只減倉標記

# 成交記錄

## 獲取成交記錄

```json
  {
    "code": "200000",
    "data": {
      "currentPage":1,
      "pageSize":1,
      "totalNum":251915,
      "totalPage":251915,
      "items":[
          {
            "symbol": "XBTUSDM",  //合約編號
            "tradeId": "5ce24c1f0c19fc3c58edc47c",  //交易編號
            "orderId": "5ce24c16b210233c36ee321d",  //訂單編號
            "side": "sell",  //買賣方向
            "liquidity": "taker",  //流動性類型 taker or maker
            "forceTaker": true, //是否強製作爲taker處理
            "price": "8302",  //成交價格
            "size": 10,  //成交數量
            "value": "0.001204529",  //成交價值
            "feeRate": "0.0005",  //費率
            "fixFee": "0.00000006",  //固定費用
            "feeCurrency": "XBT",  //收費幣種
            "stop": "",  //止損單類型標記
            "fee": "0.0000012022",  //交易費用
            "orderType": "limit",  //訂單類型
            "tradeType": "trade",  //交易類型: trade, liquidation, ADL or settlement
            "createdAt": 1558334496000,  //創建時間
            "settleCurrency": "XBT", //結算幣種
			"openFeePay": "0.002",
			"closeFeePay": "0.002",
            "tradeTime": 1558334496000000000 //交易時間納秒
          }
      ]
    }
}
```

您可通過此接口獲取最近成交的訂單列表。

### HTTP請求
`GET /api/v1/fills`

### 示例
`GET /api/v1/fills`

### API權限
該接口需獲取**通用權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 參數


參數 | 數據類型 | 含義
--------- | ------- | -----------
orderId | String |[可選] 如果指定了訂單號，會忽略其他參數。
symbol | String |[可選] 合約symbol 
side | String |[可選] **buy** or **sell** 
type | String |[可選] **limit**, **market**, **limit_stop** or **market_stop** 
startAt | long |[可選] 開始時間（毫秒）
endAt | long |[可選]  截止時間（毫秒）
currentPage | long | [可選] 頁數，不傳默認1
pageSize | long | [可選] 頁碼，不傳默認50，最大不能超過1000

### 返回值
參數  | 含義
--------- | -----------
symbol  | 合約編號
tradeId  | 交易編號
orderId  | 訂單編號
side  | 買賣方向
liquidity  | 流動性類型 taker or maker
forceTaker  | 是否強製作爲taker處理
price  | 成交價格
size  | 成交數量
value  | 成交價值
feeRate  | 費率
fixFee  | 固定費用(廢棄字段，沒有實際使用價值)
feeCurrency  | 收費幣種
stop  | 止損單類型標記
fee  | 交易費用
orderType  | 訂單類型
tradeType  | 交易類型: trade, liquidation, ADL or settlement
createdAt  | 創建時間
settleCurrency  | 結算幣種
tradeTime  | 交易時間納秒
openFeePay | 開倉交易手續費
closeFeePay | 平倉交易手續費

如果需要低延時獲取自己的最近成交歷史記錄, 請使用24小時成交列表接口。 此接口返回的歷史成交可能存在一定的延遲。
請使用查詢參數獲取指定合約的已成交訂單。

**數據時間範圍**

您可檢索一週時間範圍內的數據您範圍內檢索數據（默認從最近一天開始算起）。 若檢索時間範圍超過一週，系統將提示您超過時間限制。如果查詢只提供開始時間沒有提供結束時間，系統將自動計算結束時間（結束時間=開始時間+ 24小時），反之亦然。


**手續費**

KuCoin Futures平臺上的訂單分爲兩種類型：**Taker** 和 **Maker**。Taker單會與買賣盤上的已有訂單立即成交，而Maker單則相反，會一直留在買賣盤中等待撮合。Taker單消耗了市場的流動性，因此會被收取taker費用，而Maker單增加了市場的流動性，會被收取較低的手續費甚至獲得手續費補貼。請注意：市價單、冰山單和隱藏單都會被扣除taker手續費。


下單時，系統會預凍結您賬戶中的taker費用。流動性（liquidity）字段中的參數說明瞭訂單將會被收取taker還是maker費用。

加倉訂單需要預凍費用，減倉訂單不凍結任何費用。加倉訂單凍結的費用包括倉位保證金、開倉交易費和平倉交易費。訂單撮合成功後, 如果加倉會扣除加倉交易費, 如果是平倉會扣取平倉交易費。


## 最近成交記錄

```json
  {
    "code": "200000",
    "data": 
    [ {
     "symbol": "XBTUSDM",  //合約編號
     "tradeId": "5ce24c1f0c19fc3c58edc47c",  //交易編號
     "orderId": "5ce24c16b210233c36ee321d",  //訂單編號
     "side": "sell",  //買賣方向
     "liquidity": "taker",  //流動性類型 taker or maker
     "price": "8302",  //成交價格
     "size": 10,  //成交數量
     "value": "0.001204529",  //成交價值
     "feeRate": "0.0005",  //費用率
     "fixFee": "0.00000006",  //固定費用(廢棄字段，沒有實際使用價值)
     "feeCurrency": "XBT",  //收費幣種
     "stop": "",  //止損單類型標記
     "fee": "0.0000012022",  //交易費用
     "orderType": "limit",  //訂單類型
     "tradeType": "trade",  //交易類型, 可能是交易, 強平 或ADL 
     "createdAt": 1558334496000,  //創建時間
     "settleCurrency": "XBT", //結算幣種
	 "openFeePay": "0.002",
	 "closeFeePay": "0.002",
     "tradeTime": 1558334496000000000 //交易時間納秒
    }
   ]
  }
```

如果需要低延時獲取自己的最近成交歷史記錄, 請使用此接口。
使用此接口可獲取過去24小時內最近完成的1000筆訂單。

### HTTP請求
`GET /api/v1/recentFills`

### 示例
`GET /api/v1/recentFills`

### API權限
該接口需獲取**通用權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String |[可選] 合約symbol 




### 返回值
參數  | 含義
--------- | -----------
symbol  | 合約編號
tradeId  | 交易編號
orderId  | 訂單編號
side  | 買賣方向
liquidity  | 流動性類型 taker or maker
forceTaker  | 是否強製作爲taker處理
price  | 成交價格
size  | 成交數量
value  | 成交價值
feeRate  | 費率
fixFee  | 固定費用(廢棄字段，沒有實際使用價值)
feeCurrency  | 收費幣種
stop  | 止損單類型標記
fee  | 交易費用
orderType  | 訂單類型
tradeType  | 交易類型: trade, liquidation, ADL or settlement
createdAt  | 創建時間
settleCurrency  | 結算幣種
tradeTime  | 交易時間納秒
openFeePay | 開倉交易手續費
closeFeePay | 平倉交易手續費

## 活動訂單價值統計

```json
{
  "code": "200000",
  "data": {
    "openOrderBuySize": 20,  //未完成買單總數量
    "openOrderSellSize": 0,  //未完成賣單總數量
    "openOrderBuyCost": "0.0025252525",  //未完成買單價值總量
    "openOrderSellCost": "0",  //未完成賣單價值總量
    "settleCurrency": "XBT" //結算幣種
    }  
  }
```

此接口可用於統計用戶所有活動訂單的數量和價值信息

### HTTP請求
`GET /api/v1/openOrderStatistics`

### 示例
`GET /api/v1/openOrderStatistics`

### API權限
該接口需獲取**通用權限**。

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol |String| 指定合約的活動訂單，如：XBTUSDM的活動訂單。

### 返回值
參數  | 含義
--------- | -----------
openOrderBuySize  | 未完成買單總數量
openOrderSellSize  | 未完成賣單總數量
openOrderBuyCost  | 未完成買單價值總量
openOrderSellCost  | 未完成賣單價值總量
settleCurrency  | 結算幣種

# 倉位

## 獲取倉位詳情

```json
{
    "id": "5e81a7827911f40008e80715",                //倉位id
    "symbol": "XBTUSDTM",  							//合約symbol
    "autoDeposit": False,  							//是否自動追加保證金
    "maintMarginReq": 0.005,  						//維持保證金率
    "riskLimit": 2000000,  							//風險限額
    "realLeverage": 5.0,  							//槓桿倍數
    "crossMode": False,  							//是否全倉模式
    "delevPercentage": 0.35,  						//ADL分位數
    "openingTimestamp": 1623832410892,  			//開倉時間
    "currentTimestamp": 1623832488929,  			//當前時間戳
    "currentQty": 1,  								//當前倉位數量
    "currentCost": 40.008,  						//當前倉位價值
    "currentComm": 0.0240048,  						//當前倉位總費用
    "unrealisedCost": 40.008,  						//未實現價值
    "realisedGrossCost": 0.0,  						//累計已實現毛利價值
    "realisedCost": 0.0240048,  					//累計已實現倉位價值
    "isOpen": True,  								//是否開倉
    "markPrice": 40014.93,  						//標記價格
    "markValue": 40.01493,  						//標記價值
    "posCost": 40.008,  							//倉位價值
    "posCross": 0.0,  								//追加到倉位的保證金
    "posInit": 8.0016,  							//槓桿保證金
    "posComm": 0.02880576,  						//破產費用
    "posLoss": 0.0,  								//資金費用減少的資金
    "posMargin": 8.03040576,  						//倉位保證金
    "posMaint": 0.23284656,  						//維持保證金
    "maintMargin": 8.03733576, 						//包含未實現盈虧的倉位保證金
    "realisedGrossPnl": 0.0,  						//累計已實現毛利
    "realisedPnl": -0.0240048,  					//已實現盈虧
    "unrealisedPnl": 0.00693,  						//未實現盈虧
    "unrealisedPnlPcnt": 0.0002,  					//倉位盈虧率
    "unrealisedRoePcnt": 0.0009,  					//投資回報率
    "avgEntryPrice": 40008.0,  						//平均開倉價格
    "liquidationPrice": 32211.0,  					//強平價格
    "bankruptPrice": 32006.0,  						//破產價格
    "settleCurrency": "USDT",  						//結算幣種
    "maintainMargin": 0.25,  						//維持保證金率
    "userId": 1234321123,
	"riskLimitLevel": 1   							//當前風險限額等級
}
```

獲取用戶指定合約的倉位詳情

### HTTP請求

`GET /api/v1/position`

### 示例
`GET /api/v1/position?symbol=XBTUSDM`

### API權限
該接口需獲取**通用權限**。

### 參數

| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱    |

### 返回值
參數  | 含義
--------- | -----------
id  | 倉位id
symbol  | 合約symbol
autoDeposit  | 是否自動追加保證金
maintMarginReq  | 維持保證金率
riskLimit  | 風險限額
realLeverage  | 槓桿倍數
crossMode  | 是否全倉模式
delevPercentage  | ADL分位數
openingTimestamp  | 開倉時間
currentTimestamp  | 當前時間戳
currentQty  | 當前倉位數量
currentCost  | 當前倉位價值
currentComm  | 當前倉位總費用
unrealisedCost  | 未實現價值
realisedGrossCost  | 累計已實現毛利價值
realisedCost  | 累計已實現倉位價值
isOpen  | 是否開倉
markPrice  | 標記價格
markValue  | 標記價值
posCost  | 倉位價值
posCross  | 追加到倉位的保證金
posInit  | 槓桿保證金
posComm  | 破產費用
posLoss  | 資金費用減少的資金
posMargin  | 倉位保證金
posMaint  | 維持保證金
maintMargin  | 包含未實現盈虧的倉位保證金
realisedGrossPnl  | 累計已實現毛利
realisedPnl  | 已實現盈虧
unrealisedPnl  | 未實現盈虧
unrealisedPnlPcnt  | 倉位盈虧率
unrealisedRoePcnt  | 投資回報率
avgEntryPrice  | 平均開倉價格
liquidationPrice  | 強平價格
bankruptPrice  | 破產價格
settleCurrency  | 結算幣種
maintainMargin  | 維持保證金率
riskLimitLevel  | 當前風險限額等級
userId | 用戶id

## 獲取用戶倉位列表

```json
    {
    "id": "5e81a7827911f40008e80715",                //倉位id
    "symbol": "XBTUSDTM",                            //合約symbol
    "autoDeposit": False,                            //是否自動追加保證金
    "maintMarginReq": 0.005,                         //維持保證金率
    "riskLimit": 2000000,                            //風險限額
    "realLeverage": 5.0,                             //槓桿倍數
    "crossMode": False,                              //是否全倉模式
    "delevPercentage": 0.35,                         //ADL分位數
    "openingTimestamp": 1623832410892,               //開倉時間
    "currentTimestamp": 1623832488929,               //當前時間戳
    "currentQty": 1,                                 //當前倉位數量
    "currentCost": 40.008,                           //當前倉位價值
    "currentComm": 0.0240048,                        //當前倉位總費用
    "unrealisedCost": 40.008,                        //未實現價值
    "realisedGrossCost": 0.0,                        //累計已實現毛利價值
    "realisedCost": 0.0240048,                       //累計已實現倉位價值
    "isOpen": True,                                  //是否開倉
    "markPrice": 40014.93,                           //標記價格
    "markValue": 40.01493,                           //標記價值
    "posCost": 40.008,                               //倉位價值
    "posCross": 0.0,                                 //追加到倉位的保證金
    "posInit": 8.0016,                               //槓桿保證金
    "posComm": 0.02880576,                           //破產費用
    "posLoss": 0.0,                                  //資金費用減少的資金
    "posMargin": 8.03040576,                         //倉位保證金
    "posMaint": 0.23284656,                          //維持保證金
    "maintMargin": 8.03733576,                       //包含未實現盈虧的倉位保證金
    "realisedGrossPnl": 0.0,                         //累計已實現毛利
    "realisedPnl": -0.0240048,                       //已實現盈虧
    "unrealisedPnl": 0.00693,                        //未實現盈虧
    "unrealisedPnlPcnt": 0.0002,                     //倉位盈虧率
    "unrealisedRoePcnt": 0.0009,                     //投資回報率
    "avgEntryPrice": 40008.0,                        //平均開倉價格
    "liquidationPrice": 32211.0,                     //強平價格
    "bankruptPrice": 32006.0,                        //破產價格
    "settleCurrency": "USDT",                        //結算幣種
    "isInverse": false,                              //是否是反向合約
	"userId": 1234321123,							 //用戶id
    "maintainMargin": 0.005                          //維持保證金率
  }
```

使用該請求，可獲取用戶所有的倉位列表。

### HTTP請求
`GET /api/v1/positions`

### 示例
`GET /api/v1/positions`

### API權限
該接口需獲取**通用權限**。

### 參數
| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| currency | String | [可選] 幣種，比如XBT,USDT    |

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 返回值
參數  | 含義
--------- | -----------
id  | 倉位id
symbol  | 合約symbol
autoDeposit  | 是否自動追加保證金
maintMarginReq  | 維持保證金率
riskLimit  | 風險限額
realLeverage  | 槓桿倍數
crossMode  | 是否全倉模式
delevPercentage  | ADL分位數
openingTimestamp  | 開倉時間
currentTimestamp  | 當前時間戳
currentQty  | 當前倉位數量
currentCost  | 當前倉位價值
currentComm  | 當前倉位總費用
unrealisedCost  | 未實現價值
realisedGrossCost  | 累計已實現毛利價值
realisedCost  | 累計已實現倉位價值
isOpen  | 是否開倉
markPrice  | 標記價格
markValue  | 標記價值
posCost  | 倉位價值
posCross  | 追加到倉位的保證金
posInit  | 槓桿保證金
posComm  | 破產費用
posLoss  | 資金費用減少的資金
posMargin  | 倉位保證金
posMaint  | 維持保證金
maintMargin  | 包含未實現盈虧的倉位保證金
realisedGrossPnl  | 累計已實現毛利
realisedPnl  | 已實現盈虧
unrealisedPnl  | 未實現盈虧
unrealisedPnlPcnt  | 倉位盈虧率
unrealisedRoePcnt  | 投資回報率
avgEntryPrice  | 平均開倉價格
liquidationPrice  | 強平價格
bankruptPrice  | 破產價格
settleCurrency  | 結算幣種
isInverse  | 是否是反向合約
maintainMargin  | 維持保證金率
userId | 用戶Id

## 更改自動追加保證金狀態
```json
{
  "code": "200000",
  "data": false
}
```
### HTTP請求
`POST /api/v1/position/margin/auto-deposit-status`

### 示例
`POST /api/v1/position/margin/auto-deposit-status`

### API權限
該接口需獲取**通用權限**。

### 參數
| 參數  | 數據類型    | 含義 |
| ------ | ------- | ----------- |
| symbol | String  | 合約名稱    |
| status | boolean | 狀態        |

## 手動追加保證金
```json
{
  "id": "6200c9b83aecfb000152ddcd",
  "symbol": "XBTUSDTM",
  "autoDeposit": false,
  "maintMarginReq": 0.005,
  "riskLimit": 500000,
  "realLeverage": 18.72,
  "crossMode": false,
  "delevPercentage": 0.66,
  "openingTimestamp": 1646287090131,
  "currentTimestamp": 1646295055021,
  "currentQty": 1,
  "currentCost": 43.388,
  "currentComm": 0.0260328,
  "unrealisedCost": 43.388,
  "realisedGrossCost": 0.0,
  "realisedCost": 0.0260328,
  "isOpen": true,
  "markPrice": 43536.65,
  "markValue": 43.53665,
  "posCost": 43.388,
  "posCross": 2.4985e-05,
  "posInit": 2.1694,
  "posComm": 0.02733446,
  "posLoss": 0.0,
  "posMargin": 2.19675944,
  "posMaint": 0.24861326,
  "maintMargin": 2.34540944,
  "realisedGrossPnl": 0.0,
  "realisedPnl": -0.0260328,
  "unrealisedPnl": 0.14865,
  "unrealisedPnlPcnt": 0.0034,
  "unrealisedRoePcnt": 0.0685,
  "avgEntryPrice": 43388.0,
  "liquidationPrice": 41440.0,
  "bankruptPrice": 41218.0,
  "userId": 1234321123,
  "settleCurrency": "USDT"
}
```
### HTTP請求
`POST /api/v1/position/margin/deposit-margin`

### 示例
`POST /api/v1/position/margin/deposit-margin`

### API權限
該接口需獲取**通用權限**。

### 參數

| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱    |
| margin | Number | 保證金數量（增加保證金不能低於0.00001667XBT）|
| bizNo  | String | 業務唯一id，長度最大不超過36 |

### 返回值
參數  | 含義
--------- | -----------
id  | 倉位id
symbol  | 合約symbol
autoDeposit  | 是否自動追加保證金
maintMarginReq  | 維持保證金率
riskLimit  | 風險限額
realLeverage  | 槓桿倍數
crossMode  | 是否全倉模式
delevPercentage  | ADL分位數
openingTimestamp  | 開倉時間
currentTimestamp  | 當前時間戳
currentQty  | 當前倉位數量
currentCost  | 當前倉位價值
currentComm  | 當前倉位總費用
unrealisedCost  | 未實現價值
realisedGrossCost  | 累計已實現毛利價值
realisedCost  | 累計已實現倉位價值
isOpen  | 是否開倉
markPrice  | 標記價格
markValue  | 標記價值
posCost  | 倉位價值
posCross  | 追加到倉位的保證金
posInit  | 槓桿保證金
posComm  | 破產費用
posLoss  | 資金費用減少的資金
posMargin  | 倉位保證金
posMaint  | 維持保證金
maintMargin  | 包含未實現盈虧的倉位保證金
realisedGrossPnl  | 累計已實現毛利
realisedPnl  | 已實現盈虧
unrealisedPnl  | 未實現盈虧
unrealisedPnlPcnt  | 倉位盈虧率
unrealisedRoePcnt  | 投資回報率
avgEntryPrice  | 平均開倉價格
liquidationPrice  | 強平價格
bankruptPrice  | 破產價格
settleCurrency  | 結算幣種
userId | 用戶id

# 階梯風險限額
## 獲取合約階梯風險

```json
{
  "code": "200000",
  "data": [
    {
      "symbol": "ADAUSDTM",
      "level": 1,
      "maxRiskLimit": 500, // 該等級所處最大限額（包含）
      "minRiskLimit": 0, // 最小限額
      "maxLeverage": 20, // 最大可用槓桿
      "initialMargin": 0.05, // 初始保證金率
      "maintainMargin": 0.025 // 維持保證金率
    },
    {
      "symbol": "ADAUSDTM",
      "level": 2,
      "maxRiskLimit": 1000,
      "minRiskLimit": 500,
      "maxLeverage": 2,
      "initialMargin": 0.5,
      "maintainMargin": 0.25
    }
  ]
}
```

使用此接口可獲取指定合約的階梯風險限額等級信息

### HTTP請求
`GET /api/v1/contracts/risk-limit/{symbol}`

### 示例 Example
`GET /api/v1/contracts/risk-limit/ADAUSDTM`

### API權限
該接口需要**通用權限**

### 請求參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String | 路徑參數。合約名稱.

### 返回值
參數  | 含義
--------- | -----------
symbol  | 路徑參數。合約名稱.
level  | 等級
maxRiskLimit  | 該等級所處最大限額（包含）
minRiskLimit  | 最小限額
maxLeverage  | 最大可用槓桿
initialMargin | 初始保證金率
maintainMargin | 維持保證金率

## 修改階梯風險限額等級
```json 
// request
{ 
    "symbol": "ADASUDTM", // 合約名稱
    "level": 2 // 等級
} 
``` 

```json
// response
{
  "code": "200000",
  "data": true
} 
``` 

該接口用於修改用戶風險限額等級，修改會撤銷用戶當前掛單，返回結果僅代表修改申請提交是否成功。修改是否成功需要監聽ws消息:[風險限額調整結果](#c2df0abf58)

### HTTP請求
`POST /api/v1/position/risk-limit-level/change`

### 示例
`POST /api/v1/position/risk-limit-level/change`

### API權限
該接口需要**交易權限**

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String | 路徑參數。合約名稱.
level | Integer | 等級.

### 返回值
字段 | 含義
--------- | -------
data | 修改會撤銷用戶當前掛單，返回結果僅代表修改申請提交是否成功。

# 資金費用

## 查詢資金費用歷史

```json
  {
    "dataList": [
      {
        "id": 36275152660006,                //id
        "symbol": "XBTUSDM",                  //合約symbol
        "timePoint": 1557918000000,          //時間點(毫秒)
        "fundingRate": 0.000013,             //資金費率
        "markPrice": 8058.27,                //標記價格
        "positionQty": 10,                   //結算時的倉位數
        "positionCost": -0.001241,           //結算時的倉位價值
        "funding": -0.00000464,              //結算的資金費用，正數表示收入；負數表示支出
        "settleCurrency": "XBT"             //結算幣種

      },
      {
        "id": 36275152660004,
        "symbol": "XBTUSDM",
        "timePoint": 1557914400000,
        "fundingRate": 0.00375,
        "markPrice": 8079.65,
        "positionQty": 10,
        "positionCost": -0.0012377,
        "funding": -0.00000465,
        "settleCurrency": "XBT" 
      },
      {
        "id": 36275152660002,
        "symbol": "XBTUSDM",
        "timePoint": 1557910800000,
        "fundingRate": 0.00375,
        "markPrice": 7889.03,
        "positionQty": 10,
        "positionCost": -0.0012676,
        "funding": -0.00000476,
        "settleCurrency": "XBT" 
      }
    ],
    "hasMore": true                         //是否還有下一頁
  }
```

查詢資金費用歷史

### HTTP請求
`GET /api/v1/funding-history`

### API權限
該接口需獲取**通用權限**。

### 頻率限制
此接口針對每個賬號請求頻率限制爲**9次/3s**

### 參數

| 參數     | 數據類型    | 含義                                                  |
| --------- | ------- | -------------- |
| symbol    | String  | 合約symbol      |
| startAt | long    | [可選] 開始時間（毫秒）                      |
| endAt   | long    | [可選]  截止時間（毫秒）  |
| reverse   | boolean | [可選] 是否逆序查詢， **true** 或者 **false**，默認爲**true** |
| offset    | long    | [可選] 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁                   |
| forward   | boolean | [可選] 是否前向查詢，**true**或者**false**，默認爲**true** |
| maxCount  | int     | [可選] 最大記錄條數，默認爲10                          |


**注意:**由於數據變化的很快，如果只選擇offset,而沒有選擇startAt和endAt，可能會導致數據不准或數據重複，建議按startAt和endAt來分頁


### 返回值
字段 | 含義
--------- | -------
id | id
symbol | 合約symbol
timePoint | 時間點(毫秒)
fundingRate | 資金費率
markPrice | 標記價格
positionQty | 結算時的倉位數
positionCost | 結算時的倉位價值
funding | 結算的資金費用，正數表示收入；負數表示支出
settleCurrency | 結算幣種
hasMore | 是否還有下一頁


# 市場數據

市場數據是公共的，不需要驗證簽名。

# 合約
## 獲取開放合約列表

```json
[{
  "symbol": "XBTUSDTM",
  "rootSymbol": "USDT",
  "type": "FFWCSX",
  "firstOpenDate": 1585555200000,
  "expireDate": null,
  "settleDate": null,
  "baseCurrency": "XBT",
  "quoteCurrency": "USDT",
  "settleCurrency": "USDT",
  "maxOrderQty": 1000000,
  "maxPrice": 1000000.0,
  "lotSize": 1,
  "tickSize": 1.0,
  "indexPriceTickSize": 0.01,
  "multiplier": 0.001,
  "initialMargin": 0.01,
  "maintainMargin": 0.005,
  "maxRiskLimit": 2000000,
  "minRiskLimit": 2000000,
  "riskStep": 1000000,
  "makerFeeRate": 0.0002,
  "takerFeeRate": 0.0006,
  "takerFixFee": 0.0,
  "makerFixFee": 0.0,
  "settlementFee": null,
  "isDeleverage": true,
  "isQuanto": true,
  "isInverse": false,
  "markMethod": "FairPrice",
  "fairMethod": "FundingRate",
  "fundingBaseSymbol": ".XBTINT8H",
  "fundingQuoteSymbol": ".USDTINT8H",
  "fundingRateSymbol": ".XBTUSDTMFPI8H",
  "indexSymbol": ".KXBTUSDT",
  "settlementSymbol": "",
  "status": "Open",
  "fundingFeeRate": 0.0001,
  "predictedFundingFeeRate": 0.0001,
  "openInterest": "5191275",
  "turnoverOf24h": 2361994501.712677,
  "volumeOf24h": 56067.116,
  "markPrice": 44514.03,
  "indexPrice": 44510.78,
  "lastTradePrice": 44493.0,
  "nextFundingRateTime": 21031525,
  "maxLeverage": 100,
  "sourceExchanges": [
    "huobi",
    "Okex",
    "Binance",
    "Kucoin",
    "Poloniex",
    "Hitbtc"
  ],
  "premiumsSymbol1M": ".XBTUSDTMPI",
  "premiumsSymbol8H": ".XBTUSDTMPI8H",
  "fundingBaseSymbol1M": ".XBTINT",
  "fundingQuoteSymbol1M": ".USDTINT",
  "lowPrice": 38040,
  "highPrice": 44948,
  "priceChgPct": 0.1702,
  "priceChg": 6476
}]
```

獲取所有開放的合約信息

### HTTP請求
`GET /api/v1/contracts/active`

### 示例
`GET /api/v1/contracts/active`

### 參數
無

### 返回值
字段 | 含義
--------- | -------
symbol | 合約名稱
rootSymbol | 合約系列
type | 合約類型
firstOpenDate | 首次開放時間
expireDate | 到期日期, 爲NULL表示永不過期
settleDate | 結算日期, 爲NULL表示不支持自動結算
baseCurrency | 基礎貨幣
quoteCurrency | 計價貨幣
settleCurrency | 結算幣種
maxOrderQty | 最大委託數量
maxPrice | 最大下單價格
lotSize | 最小合約數量
tickSize | 最小的價格變化
indexPriceTickSize | 指數價格變化步長
multiplier | 合約乘數
initialMargin | 初始保證金率
maintainMargin | 維持保證金率
maxRiskLimit | 最大風險限額(以XBT爲單位)
minRiskLimit | 最小風險限額(以XBT爲單位)
riskStep | 風險限額遞增值(以XBT爲單位)
makerFeeRate | maker手續費
takerFeeRate | taker手續費
takerFixFee | taker手續費固定值(廢棄字段，沒有實際使用價值)
makerFixFee | maker手續費固定值(廢棄字段，沒有實際使用價值)
settlementFee | 結算手續費
isDeleverage | 是否支持自動減倉
isQuanto | 是否quanto(廢棄字段，沒有實際使用價值)
isInverse | 是否是反向合約
markMethod | 標記方式
fairMethod | 合理標記方式
fundingBaseSymbol | 基礎貨幣symbol
fundingQuoteSymbol | 計價貨幣symbol
fundingRateSymbol | 資金費率symbol
indexSymbol | 指數symbol
settlementSymbol | 結算symbol
status | 合約狀態
fundingFeeRate | 資金費率值
predictedFundingFeeRate | 預測資金費率值
openInterest | 活動倉位數
turnoverOf24h | 24 小時成交額
volumeOf24h | 24 小時成交量
markPrice | 標記價格
indexPrice | 指數價格
lastTradePrice | 最新成交價
nextFundingRateTime | 下次資金費率時間
maxLeverage | 最大可用槓桿
sourceExchanges | 該合約指數來源交易所
premiumsSymbol1M | 溢價指數symbol(1分鐘)
premiumsSymbol8H | 溢價指數symbol 8小時
fundingBaseSymbol1M | 基礎貨幣利率symbol(1分鐘)
fundingQuoteSymbol1M | 計價貨幣利率symbol(1分鐘)
lowPrice | 24 小時最低成交價
highPrice | 24 小時最高成交價
priceChgPct | 24 小時漲跌幅
priceChg | 24 小時漲跌價格

## 獲取合約詳細信息
```json
{
  "symbol": "DASHUSDTM",
  "rootSymbol": "USDT",
  "type": "FFWCSX",
  "firstOpenDate": 1610697600000,
  "expireDate": null,
  "settleDate": null,
  "baseCurrency": "DASH",
  "quoteCurrency": "USDT",
  "settleCurrency": "USDT",
  "maxOrderQty": 1000000,
  "maxPrice": 1000000.0,
  "lotSize": 1,
  "tickSize": 0.01,
  "indexPriceTickSize": 0.01,
  "multiplier": 0.01,
  "initialMargin": 0.05,
  "maintainMargin": 0.025,
  "maxRiskLimit": 100000,
  "minRiskLimit": 100000,
  "riskStep": 50000,
  "makerFeeRate": 0.0002,
  "takerFeeRate": 0.0006,
  "takerFixFee": 0.0,
  "makerFixFee": 0.0,
  "settlementFee": null,
  "isDeleverage": true,
  "isQuanto": false,
  "isInverse": false,
  "markMethod": "FairPrice",
  "fairMethod": "FundingRate",
  "fundingBaseSymbol": ".DASHINT8H",
  "fundingQuoteSymbol": ".USDTINT8H",
  "fundingRateSymbol": ".DASHUSDTMFPI8H",
  "indexSymbol": ".KDASHUSDT",
  "settlementSymbol": "",
  "status": "Open",
  "fundingFeeRate": 0.0001,
  "predictedFundingFeeRate": 0.0001,
  "openInterest": "2487402",
  "turnoverOf24h": 3166644.36115288,
  "volumeOf24h": 32299.4,
  "markPrice": 101.6,
  "indexPrice": 101.59,
  "lastTradePrice": 101.54,
  "nextFundingRateTime": 22646889,
  "maxLeverage": 20,
  "sourceExchanges": [
    "huobi",
    "Okex",
    "Binance",
    "Kucoin",
    "Poloniex",
    "Hitbtc"
  ],
  "premiumsSymbol1M": ".DASHUSDTMPI",
  "premiumsSymbol8H": ".DASHUSDTMPI8H",
  "fundingBaseSymbol1M": ".DASHINT",
  "fundingQuoteSymbol1M": ".USDTINT",
  "lowPrice": 88.88,
  "highPrice": 102.21,
  "priceChgPct": 0.1401,
  "priceChg": 12.48
}
```

使用此接口可獲取指定合約的信息


### HTTP請求
`GET /api/v1/contracts/{symbol}`

### 示例
`GET /api/v1/contracts/XBTUSDM`

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | String | 路徑參數。合約名稱

### 返回值
字段 | 含義
--------- | -------
symbol | 合約名稱
rootSymbol | 合約系列
type | 合約類型
firstOpenDate | 首次開放時間
expireDate | 到期日期, 爲NULL表示永不過期
settleDate | 結算日期, 爲NULL表示不支持自動結算
baseCurrency | 基礎貨幣
quoteCurrency | 計價貨幣
settleCurrency | 結算幣種
maxOrderQty | 最大委託數量
maxPrice | 最大下單價格
lotSize | 最小合約數量
tickSize | 最小的價格變化
indexPriceTickSize | 指數價格變化步長
multiplier | 合約乘數
initialMargin | 初始保證金率
maintainMargin | 維持保證金率
maxRiskLimit | 最大風險限額(以XBT爲單位)
minRiskLimit | 最小風險限額(以XBT爲單位)
riskStep | 風險限額遞增值(以XBT爲單位)
makerFeeRate | maker手續費
takerFeeRate | taker手續費
takerFixFee | taker手續費固定值(廢棄字段，沒有實際使用價值)
makerFixFee | maker手續費固定值(廢棄字段，沒有實際使用價值)
settlementFee | 結算手續費
isDeleverage | 是否支持自動減倉
isQuanto | 是否quanto(廢棄字段，沒有實際使用價值)
isInverse | 是否是反向合約
markMethod | 標記方式
fairMethod | 合理標記方式
fundingBaseSymbol | 基礎貨幣symbol
fundingQuoteSymbol | 計價貨幣symbol
fundingRateSymbol | 資金費率symbol
indexSymbol | 指數symbol
settlementSymbol | 結算symbol
status | 合約狀態
fundingFeeRate | 資金費率值
predictedFundingFeeRate | 預測資金費率值
openInterest | 活動倉位數
turnoverOf24h | 24 小時成交額
volumeOf24h | 24 小時成交量
markPrice | 標記價格
indexPrice | 指數價格
lastTradePrice | 最新成交價
nextFundingRateTime | 下次資金費率時間
maxLeverage | 最大可用槓桿
sourceExchanges | 該合約指數來源交易所
premiumsSymbol1M | 溢價指數symbol(1分鐘)
premiumsSymbol8H | 溢價指數symbol 8小時
fundingBaseSymbol1M | 基礎貨幣利率symbol(1分鐘)
fundingQuoteSymbol1M | 計價貨幣利率symbol(1分鐘)
lowPrice | 24 小時最低成交價
highPrice | 24 小時最高成交價
priceChgPct | 24 小時漲跌幅
priceChg | 24 小時漲跌價格

# 行情快照

## 獲取實時行情

```json
  {
    "code": "200000",
    "data": {
      "sequence": 1001,				// 順序號
      "symbol": "XBTUSDM",				// 合約
      "side": "buy",					// 成交方向 - taker
      "size": 10,						// 成交數量
      "price": "7000.0",				// 成交價格
      "bestBidSize": 20,				// 最佳買一價總量
      "bestBidPrice": "7000.0",		// 最佳買一價
      "bestAskSize": 30,				// 最佳賣一價總量
      "bestAskPrice": "7001.0",		// 最佳賣一價
      "tradeId": "5cbd7377a6ffab0c7ba98b26",  // 交易號
      "ts": 1550653727731			   // 成交時間 - 納秒
    }
  }
```
返回的實時行情數據將包含最近成交價格、最近成交數量、最近交易號、taker方向，成交後的最佳買一價及其數量、成交後最佳賣一價及其數量，以及成交時間等。您也可通過該websocket獲取該數據。返回數據中的，順序號可用於判斷websocket推送的消息的連續性。



### HTTP請求
`GET /api/v1/ticker`

### 示例
`GET /api/v1/ticker?symbol=XBTUSDM`

### 參數
參數 | 數據類型 | 含義
--------- | ------- | -----------
symbol | string | 合約名稱

### 返回值
字段 | 含義
--------- | -------
sequence | 順序號
symbol | 合約
side | 成交方向
size | 成交數量
price | 成交價格
bestBidSize | 最佳買一價總量
bestBidPrice | 最佳買一價
bestAskSize | 最佳賣一價總量
bestAskPrice | 最佳賣一價
tradeId | 交易號
ts | 成交時間 - 納秒

# 委託買賣盤
## 獲取全部買賣盤 - Level 2

```json
{
	"code": "200000",
	"data": {
		"symbol": "XBTUSDM",		// 合約
		"sequence": 100,			// 快照序號
		"asks": [
			["5000.0", 1000],	// 價格、數量
			["6000.0", 1983]		// 價格、數量
		],
		"bids": [
			["3200.0", 800],		// 價格、數量
			["3100.0", 100]		// 價格、數量
		],
    "ts": 1604643655040584408  // 時間戳
	}
}
```

此接口獲取指定合約的所有活動委託的快照。
Level 2 買賣盤上的買單和賣單均按照價格彙總，每個價格下僅返回一個根據價格彙總的掛單量。
此接口將返回全部的買賣盤數據。

該功能適用於專業交易員，因爲該過程將使用較多服務器資源及流量，訪問頻率受到了嚴格控制。
爲保證本地買賣盤數據爲最新數據，在獲取Level 2快照後，請使用[Websocket](#level-2-4)推送的增量消息來更新Level 2買賣盤。
返回值中，賣盤數據是按照價格從低到高排序的，買盤數據是按照價格從高到低排序的。

### HTTP請求
`GET /api/v1/level2/snapshot`

### 示例
`GET /api/v1/level2/snapshot?symbol=XBTUSDM`

### 頻率限制
此接口針對每個賬號請求頻率限制爲**30次/3s**

### 參數
| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱    |

### 返回值
字段 | 含義
--------- | -------
symbol | 合約
sequence | 快照序號
asks | 賣盤
bids | 買盤
ts | 時間戳

## 獲取部分買賣盤 - Level 2
```json
{
	"code": "200000",
	"data": {
		"symbol": "XBTUSDM",		// 合約
		"sequence": 100,			// 快照序號
		"asks": [
			["5000.0", 1000],	// 價格、數量
			["6000.0", 1983]		// 價格、數量
		],
		"bids": [
			["3200.0", 800],		// 價格、數量
			["3100.0", 100]		// 價格、數量
		],
    "ts": 1604643655040584408  // 時間戳
	}
}
```
此接口，可獲取指定合約的買賣盤數據。

買賣盤上的買單和賣單均按照價格彙總，每個價格下僅返回一個根據價格彙總的掛單量。

此接口，只會返回部分的買賣盤數據，level2_20是指返回買賣方各20條數據，level_100 是指返回買賣方各100條數據。推薦您使用這個接口，因爲響應速度更快，流量消耗小。
### HTTP請求
`GET /api/v1/level2/depth20`<br/>
`GET /api/v1/level2/depth100`
### 示例
`GET /api/v1/level2/depth100?symbol=XBTUSDM`

### 參數
| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱    |

### 返回值
字段 | 含義
--------- | -------
symbol | 合約
sequence | 快照序號
asks | 賣盤
bids | 買盤
ts | 時間戳

## Level 2消息拉取(廢棄)
```json
  {
    "code": "200000",
    "data": [
      {
          "symbol": "XBTUSDM",				// 合約
          "sequence": 1,					// 消息順序號
          "change": "7000.0,sell,10"		// 價格、方向、數量
        },
      {
          "symbol": "XBTUSDM",				// 合約
          "sequence": 2,					// 消息順序號
          "change": "7000.0,sell,0"		// 價格、方向、數量
      }
    ]
  }
```
如果websocket推送的消息不連續，可通過該請求拉取缺失的消息。start爲上一次收到websocket推送的sequence+1，end爲本次收到的websocket推送的sequence-1。重放拉取的消息，完成後繼續消費websocket消息。如果end和start的差值超過500，則不能直接使用該接口，建議重新構建Level 2的買賣盤。

Level 2拉取消息使用方法：以價格爲鍵值，用消息中的數量覆蓋本地的數量。當數量爲0時，刪除該數量在本地記錄中對應的價格。

### HTTP請求
`GET /api/v1/level2/message/query`

### 示例
`GET /api/v1/level2/message/query?symbol=XBTUSDM&start=100&end=200`

### 參數
| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱    |
| start | long | 開始順序號（返回的數據會包含該順序號）   |
| end | long | 結束順序號（返回的數據會包含該順序號）   |



# 歷史數據

## 成交歷史

```json
  {
    "code": "200000",
    "data": {
			"sequence": 102,					              // 序號
			"tradeId": "5cbd7377a6ffab0c7ba98b26",      // 交易號
			"takerOrderId": "5cbd7377a6ffab0c7ba98b27", // Taker方訂單ID
			"makerOrderId": "5cbd7377a6ffab0c7ba98b28", // Maker方訂單ID
			"price": "7000.0",                          // 成交價格
			"size": 1,                                // 成交數量
			"side": "buy",                              // 成交方向 - taker
      "ts": 1545904567062140823                   // 成交時間 - 納秒
		}
  }
```
使用該接口可獲取指定合約的最近一百條交易記錄

### HTTP請求
`GET /api/v1/trade/history`

### 示例
`GET /api/v1/trade/history?symbol=XBTUSDM`

### 參數
| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String | 合約名稱     |

### 返回值
字段 | 含義
--------- | -------
sequence | 序號
tradeId | 交易號
takerOrderId | Taker方訂單ID
makerOrderId | Maker方訂單ID
price | 成交價格
size | 成交數量
side | 成交方向
ts | 成交時間 - 納秒

### 屬性含義

**交易方向**

Taker訂單的成交方向。Taker訂單指立刻與買賣盤上的已有訂單成交的訂單類型。



# 指數

## 查詢利率列表

```json
  {
    "dataList": [
      {
        "symbol": ".XBTINT",                 //利率symbol
        "granularity": 60000,                //粒度(毫秒)
        "timePoint": 1557996300000,          //時間點(毫秒)
        "value": 0.0003                      //利率值
      },
      {
        "symbol": ".XBTINT",
        "granularity": 60000,
        "timePoint": 1557996240000,
        "value": 0.0003
      },
      {
        "symbol": ".XBTINT",
        "granularity": 60000,
        "timePoint": 1557996180000,
        "value": 0.0003
      }
    ],
    "hasMore": true                          //是否還有下一頁
  }
```

查詢利率列表

### HTTP請求
`GET /api/v1/interest/query`

### 示例
`GET /api/v1/interest/query?symbol=.XBTINT`

### 參數

| 參數     | 數據類型    | 含義    |
| --------- | ------- | ---------------------- |
| symbol    | String  | 利率symbol     |
| startAt | long    | [可選] 開始時間（毫秒）                     |
| endAt   | long    | [[可選]  截止時間（毫秒）              |
| reverse   | boolean | [可選]是否逆序查詢, **true**或**false**，默認爲**true** |
| offset    | long    | [可選] 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁|
| forward   | boolean | [可選] 是否前向查詢，true或false，默認爲true |
| maxCount  | int     | [可選] 最大記錄條數，默認爲10，長度最大不超過100  |

### 返回值
字段 | 含義
--------- | -------
symbol | 利率symbol
granularity | 粒度(毫秒)
timePoint | 時間點(毫秒)
value | 利率值
hasMore | 是否還有下一頁


## 查詢指數列表

```json
{ 
    "dataList": [
      {
        "symbol": ".KXBT",                   //指數symbol
        "granularity": 1000,                 //粒度(毫秒)
        "timePoint": 1557998570000,          //時間點(毫秒)
        "value": 8016.24,                    //指數值
        "decomposionList": [                 //成分列表
          {
            "exchange": "gemini",            //成分交易所
            "price": 8016.24,                //最近成交價
            "weight": 0.08042611             //權重
          },
          {
            "exchange": "kraken",
            "price": 8016.24,
            "weight": 0.02666502
          },
          {
            "exchange": "coinbase",
            "price": 8016.24,
            "weight": 0.03847059
          },
          {
            "exchange": "liquid",
            "price": 8016.24,
            "weight": 0.20419723
          },
          {
            "exchange": "bittrex",
            "price": 8016.24,
            "weight": 0.29848962
          },
          {
            "exchange": "bitstamp",
            "price": 8016.24,
            "weight": 0.35175143
          }
        ]
      }
    ],
    "hasMore": true                            //是否還有下一頁
  }
```

查詢指數列表

### HTTP請求
`GET /api/v1/index/query`

### 參數
| 參數     | 數據類型    | 含義                                                  |
| --------- | ------- | ------------------------ |
| symbol    | String  | 利率symbol        |
| startAt | long    | [可選] 開始時間（毫秒）               |
| endAt   | long    | [可選]  截止時間（毫秒）   |
| reverse   | boolean | [可選] 是否逆序查詢，**true** 或 **false**，默認爲**true** |
| offset    | long    | [可選] 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁   |
| forward   | boolean | [可選] 是否前向查詢，**true** 或 **false**，默認爲**true** |
| maxCount  | int     | [可選] 最大記錄條數，默認爲10，長度最大不超過100      |

### 返回值
字段 | 含義
--------- | -------
symbol | 指數symbol
granularity | 粒度(毫秒)
timePoint | 時間點(毫秒)
value | 指數值
decomposionList | 成分列表
exchange | 成分交易所
price | 最近成交價
weight | 權重
hasMore | 是否還有下一頁

## 查詢當前標記價格

```json
  {
    "symbol": "XBTUSDM",                //合約symbol
    "granularity": 1000,               //粒度(毫秒)
    "timePoint": 1557999585000,        //時間點(毫秒)
    "value": 8052.51,                  //標記價格
    "indexPrice": 8041.95              //指數價格
  }
```

查詢當前標記價格

### HTTP請求
`GET /api/v1/mark-price/{symbol}/current`

### 示例
`GET /api/v1/mark-price/XBTUSDM/current`

### 參數

| 參數  | 數據類型   | 含義 |
| ------ | ------ | ----------- |
| symbol | String |  合約symbol  |

### 返回值
字段 | 含義
--------- | -------
symbol | 合約symbol
granularity | 粒度(毫秒)
timePoint | 時間點(毫秒)
value | 標記價格
indexPrice | 指數價格

## 查詢溢價指數

```json
  {
    "dataList": [
      {
        "symbol": ".XBTUSDMPI",              //溢價指數symbol
        "granularity": 60000,                //粒度(毫秒)
        "timePoint": 1558000320000,          //時間點(毫秒)
        "value": 0.022585                    //溢價指數值
      },
      {
        "symbol": ".XBTUSDMPI",
        "granularity": 60000,
        "timePoint": 1558000260000,
        "value": 0.022611
      },
      {
        "symbol": ".XBTUSDMPI",
        "granularity": 60000,
        "timePoint": 1558000200000,
        "value": 0.021421
      }
    ],
    "hasMore": true                        //是否還有下一頁
  }
```

查詢溢價指數

### HTTP請求
`GET /api/v1/premium/query`

### 參數

| 參數     | 數據類型    | 含義                                                  |
| --------- | ------- | -------------------------- |
| symbol    | String  | 溢價指數symbol      |
| startAt | long    | [可選] 開始時間（毫秒）                     |
| endAt   | long    | [可選]  截止時間（毫秒）  |
| reverse   | boolean | [可選] 是否逆序查詢, **true** 或者 **false**, 默認爲**true** |
| offset    | long    | [可選] 起始偏移量，一般使用上個請求最後一條返回結果的唯一屬性，默認返回第一頁|
| forward   | boolean | [可選] 是否前向查詢, **true**或者**false**, 默認爲**true** |
| maxCount  | int     | [可選] 最大記錄條數, 默認爲10，長度最大不超過100           |

### 返回值
字段 | 含義
--------- | -------
symbol | 資金費率symbol
granularity | 粒度(毫秒)
timePoint | 時間點(毫秒)
value | 資金費率
hasMore | 是否還有下一頁

## 查詢當前資金費率

```json
  {
    "symbol": ".XBTUSDMFPI8H",              //資金費率symbol 
    "granularity": 28800000,               //粒度(毫秒)
    "timePoint": 1558000800000,            //時間點(毫秒)
    "value": 0.00375,                      //資金費率
    "predictedValue": 0.00375              //預測資金費率
  }
```
查詢當前資金費率

### HTTP請求
`GET /api/v1/funding-rate/{symbol}/current`

### 示例
`GET /api/v1/funding-rate/XBTUSDM/current`


### 參數

| 參數  | 數據類型   | 含義    |
| ------ | ------ | -------------- |
| symbol | String | 合約名稱|

### 返回值
字段 | 含義
--------- | -------
symbol | 資金費率symbol
granularity | 粒度(毫秒)
timePoint | 時間點(毫秒)
value | 資金費率
predictedValue | 預測資金費率

# 時間
## 獲取服務器時間

```json
  {  
    "code":"200000",
    "msg":"success",
    "data":1546837113087
  }
```

獲取API服務器時間。這是Unix時間戳。

### HTTP請求
`GET /api/v1/timestamp`

### 返回值
字段 | 含義
--------- | -------
data | 服務器時間, Unix時間戳。


# 服務狀態

## 獲取當前服務狀態


```json
  {    
    "code": "200000",     
    "data": {
        "status": "open",                //open: 正常運行, close: 服務關閉, cancelonly:只能撤單
        "msg":  "upgrade match engine"   //備註
      }
  }
```

獲取當前服務狀態


### HTTP Request
`GET /api/v1/status`

### 返回值
字段 | 含義
--------- | -------
status | 服務狀態。open: 正常運行, close: 服務關閉, cancelonly:只能撤單
msg | 備註

# K線

## 獲取合約K線數據

### HTTP請求
`GET /api/v1/kline/query`

### 示例
`GET /api/v1/kline/query?symbol=.KXBT&granularity=480&from=1535302400000&to=1559174400000`

### 參數
| 參數     | 數據類型    | 含義                                                  |
| --------- | ------- | -------------------------- |
| symbol    | String  | [必選]symbol|
| granularity | int    | [必選]K線粒度（分鐘數）|
| from   | long    | [可選]開始時間（毫秒）|
| to   | long | [可選]結束時間（毫秒）|

### 返回值
```json
{
    "code": "200000",
    "data": [
        [
            1575331200000,//時間
            7495.01,      //開盤價
            8309.67,      //最高價
            7250,         //最低價
            7463.55,      //收盤價
            0             //成交量
        ],
        [
            1575374400000,
            7464.37,
            8297.85,
            7273.02,
            7491.44,
            0
        ]
    ]
}
```



### 說明
1.granularity（k線粒度參數）代表分鐘數，可選範圍：1,5,15,30,60,120,240,480,720,1440,10080。granularity不在該範圍的請求將被拒絕<br/>

2.單次請求的最大數據量是200。如果您選擇的開始/結束時間和時間粒度導致超過單次請求的最大數據量，您的請求將只會返回200個數據。如果您希望在更大的時間範圍內獲取足夠精細的數據，則需要使用多個開始/結束範圍進行多次請求。<br/>

3.如果只提供了開始時間，則查詢開始時間到系統當前時間最大200條數據。如果只提供了結束時間，則返回離結束時間最近的200條數據。如果開始時間和結束時間均未提供，則查詢離系統當前時間最近的200條數據<br/>


# Websocket
REST API的使用受到了訪問頻率的限制，因此推薦您使用Websocket獲取實時數據。

**推薦您創建一條Websocket連接，多頻道訂閱獲取實時數據。**

## 申請連接令牌

```json
  {
    "code": "200000",
    "data": {
        "instanceServers": [
            {
                "pingInterval": 50000,
                "endpoint": "wss://push.kucoin.com/endpoint",
                "protocol": "websocket",
                "encrypt": true,
                "pingTimeout": 10000
            }
        ],
        "token": "vYNlCtbz4XNJ1QncwWilJnBtmmfe4geLQDUA62kKJsDChc6I4bRDQc73JfIrlFaVYIAE0Gv2--MROnLAgjVsWkcDq_MuG7qV7EktfCEIphiqnlfpQn4Ybg==.IoORVxR2LmKV7_maOR9xOg=="
    }
  }
```

在創建Websocket連接前，您需申請一個令牌（Token）。



### 公共令牌 (不需要加簽登錄)

如果您只訂閱公共頻道的數據，請按照以下方式請求獲取服務器列表和臨時公共令牌。

#### HTTP請求
`POST /api/v1/bullet-public`

### 私有頻道（需要驗證簽名）

```json
  {
    "code": "200000",
    "data": {
        "instanceServers": [
            {
                "pingInterval": 50000,
                "endpoint": "wss://push.kucoin.com/endpoint",
                "protocol": "websocket",
                "encrypt": true,
                "pingTimeout": 10000
            }
        ],
        "token": "vYNlCtbz4XNJ1QncwWilJnBtmmfe4geLQDUA62kKJsDChc6I4bRDQc73JfIrlFaVYIAE0Gv2--MROnLAgjVsWkcDq_MuG7qV7EktfCEIphiqnlfpQn4Ybg==.IoORVxR2LmKV7_maOR9xOg=="
    }
  }
```

如您需請求私有頻道的數據（如：賬戶資金變化），請在簽名驗證後按照以下方式獲取Websocket的服務實例和已驗籤的令牌。


#### HTTP 請求
`POST /api/v1/bullet-private`


### 返回值

|字段 | 含義|
-----|-----
|pingInterval| 發送消息的間隔時間（毫秒）|
|pingTimeout| 如果在pingTimeout時間後，未收到pong消息，那麼連接可能已斷開了 |
|endpoint| Websocket建立連接的服務器地址 |
|protocol| 支持的協議 |
|encrypt| 表示是否使用了SSL加密 |
|token | 令牌 |

## 建立連接

```javascript
var socket = new WebSocket("wss://push.kucoin.com/endpoint?token=xxx&[connectId=xxxxx]");
```

成功建立連接後，您將會收到系統向您發出的歡迎（welcome）消息。

```json
  {
    "id":"hQvf8jkno",
    "type":"welcome"
  } 
```

**connectId**：連接ID，是客戶端生成的唯一標識。您在創建連接時收到的歡迎（welcome）消息的ID以及錯誤消息的ID都屬於連接ID（connectId）。


## Ping
```json
  {
    "id":"1545910590801",
    "type":"ping"
  }
```

爲防止服務器斷開TCP連接，客戶端需要向服務器發送ping消息以保持連接的活躍性。

在服務器收到ping消息後，系統會向客戶端返回一條pong消息。

如果服務器在60秒內沒有收到來自客戶端的ping消息，連接將被斷開。


```json
  {
    "id":"1545910590801",
    "type":"pong"
  }
```

## 訂閱數據

```json
  {
    "id": 1545910660739,                          //表示ID的唯一值 
    "type": "subscribe",
    "topic": "/market/ticker:XBTUSDM",  // 被訂閱的頻道。一些頻道支持使用“,”分開訂閱多個合約的信息推送。
    "privateChannel": false,                      // 是否使用了私有頻道，默認設置爲“false”。
    "response": true                              // 服務器是否需要返回該頻道推送的信息。默認設置爲“false”。
  }
```

使用服務器訂閱消息時，客戶端應向服務器發送訂閱消息。

訂閱成功後，當“response”參數爲“false”時，系統將向您發出“ack”消息。

```json
  {
    "id":"1545910660739",
    "type":"ack"
  }
```

當訂閱頻道產生新消息時，系統將向客戶端推送消息。瞭解消息格式，請查看頻道介紹。


### 參數
#### ID
ID用於標識請求和ack的唯一字符串。

#### Topic
您訂閱的頻道內容。

#### PrivateChannel

您可通過privateChannel參數訂閱以一些用戶私有的topic（如：/contractMarket/level2）。該參數默認設置爲“false”。設置爲“true”時，則您只能收到與您訂閱的topic相關的內容推送。

#### Response
若設置爲True, 用戶成功訂閱後，系統將返回ack消息。

## 退訂
用於取消您之前訂閱的topic

```json
  {
    "id": "1545910840805",                            // 表示ID的唯一值 
    "type": "unsubscribe",
    "topic": "/market/ticker:XBTUSDM",      //被取消訂閱的頻道。一些頻道支持使用“,”分開取消多個交易對的信息訂閱。
    "privateChannel": false, 
    "response": true,                                  //服務器是否需要返回該頻道推送的信息。默認設置爲“false”。


  }
```

```json
  {
    "id": "1545910840805",
    "type": "ack"
  }
```
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

### 參數

#### ID
用於標識請求的唯一字符串。

#### Topic
您訂閱的topic內容。

#### PrivateChannel
您可通過privateChannel參數訂閱以一些公共topic（如：/contractMarket/tradeOrders）。該參數默認設置爲“false”。設置爲“true”，您只能收到與您訂閱相關的內容推送。

#### Response
若設置爲True, 用戶成功取消訂閱後，系統將返回ack消息。

## 多路複用
 在一條物理連接上，您可開啓多條多路複用通道，以訂閱不同topic，獲取多種數據推送。

例如：
請輸入以下指令定開啓多條bt1通道
 {"id": "1Jpg30DEdU", "type": "openTunnel", "newTunnelId": "bt1", "response": true}

在指定中添加參數**tunnelId**：
{"id": "1JpoPamgFM", "type": "subscribe", "topic": "/market/ticker:XBTUSDM"，"tunnelId": "bt1", "response": true}

請求成功後，您將收到 **tunnelIId** 對應的消息推送：
{"id": "1JpoPamgFM", "type": "message", "topic": "/market/ticker:XBTUSDM", "subject": "trade.ticker", "tunnelId": "bt1", "data": {...}}

關閉**通道**，請輸入以下指令：
{"id": "1JpsAHsxKS", "type": "closeTunnel", "tunnelId": "bt1", "response": true}

##### 限制

1. 多路複用僅限API用戶使用。
2. 最多可開啓的多路複用通道條數：5條。

## 定序編號
買賣盤數據化、成交歷史數據以及快照消息均會默認返回sequence字段。您可以從Level 2和Level 3市場行情數據中的sequence來判斷數據是否丟失，連接是否穩定。如果連接不穩定，請按照校準流程進行校準。

## 客戶端消息判斷邏輯

1. 判斷消息類型。當前消息類型有三種消息類型：
    message（常用的推送消息）
    notice（通知）
    command（連續的命令）
2. 通過userId判斷消息類型。有userId的消息爲私有消息，沒有userId的消息爲一般消息。
3. 通過topic判斷消息類型。可通過topic，來判斷消息類型。
4. 通過subject判斷消息類型。對於同一個topic下不同類型的消息，可通過subject判斷消息類型。


# 公共頻道

## 交易實時行情 ticker v2

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/tickerV2:XBTUSDM",
    "response": true                              
  }
```

Topic:`/contractMarket/tickerV2:{symbol}`

```json
  {
    "subject": "tickerV2",
    "topic": "/contractMarket/tickerV2:XBTUSDM",
    "data": {
      "symbol": "XBTUSDM",					// 行情
      "bestBidSize": 795,					// 最佳買一價總數量
      "bestBidPrice": 3200.00,			// 最佳買一價
      "bestAskPrice": 3600.00,			// 最佳賣一價
      "bestAskSize": 284,					// 最佳賣一價總數量
      "ts": 1553846081210004941		// 成交時間 - 納秒
   }
  }
```
訂閱此topic，可獲取指定交易對的最佳買一和賣一價（BBO）的數據推送。

每當買賣盤有變化時，推送實時ticker。v2版本推送更具有實時性，推薦接入該版本。

推送頻率爲最多100ms一次。

<aside class="spacer8"></aside>

## 交易實時行情 ticker

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/ticker:XBTUSDM",
    "response": true                              
  }
```

Topic: `/contractMarket/ticker:{symbol}`

```json
  {
    "subject": "ticker",
    "topic": "/contractMarket/ticker:XBTUSDM",
    "data": {
      "symbol": "XBTUSDM",					// 行情
      "sequence": 45,						// 順序號，用於判斷消息連續
      "side": "sell",						// 最新成交的taker方向
      "price": 3600.00,					// 成交價格
      "size": 16,							// 成交數量
      "tradeId": "5c9dcf4170744d6f5a3d32fb",    // 訂單號
      "bestBidSize": 795,					// 最佳買一價總數量
      "bestBidPrice": 3200.00,			// 最佳買一價
      "bestAskPrice": 3600.00,			// 最佳賣一價
      "bestAskSize": 284,					// 最佳賣一價總數量
      "ts": 1553846081210004941		// 成交時間 - 納秒
   }
  }
```
訂閱此topic，可獲取指定交易對的最佳買一和賣一價（BBO）的數據推送。

每完成一筆撮合，該渠道就會實時推送一次價格。如果有多個訂單在同一時間被撮合，僅推送最近一筆完成撮合的訂單事件。

該推送已不推薦使用，獲取實時的ticker，請訂閱 `/contractMarket/tickerV2:{symbol}`。

<aside class="spacer8"></aside>

## Level 2 市場行情

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/level2:XBTUSDM",
    "response": true                              
  }
```

Topic：`/contractMarket/level2:{symbol}`

訂閱此topic，獲取Level 2買賣盤數據。

訂閱成功後，Websocket系統將向您推送增量數據的消息。

```json
  {
    "subject": "level2",
    "topic": "/contractMarket/level2:XBTUSDM",
    "type": "message",
    "data": {
      "sequence": 18,					// 順序號，用於判斷消息連續
      "change": "5000.0,sell,83"		// 價格、方向、數量
      "timestamp": 1551770400000 
      
      }
  }
```

校準流程：

1. 將Websocket推送的Level 2數據緩存在本地。
2. 通過REST請求拉取[Level 2](#level-2)買賣盤的快照信息。
3. 回放緩存的Level 2數據流。
4. 將拉取的最新Level 2數據流回放到本地緩存中，以確保最新的Level 2買賣盤數據順序號與之前的Level 2數據順序號連續無間斷。丟棄掉舊Level 2數據該順序號之前的數據，更新Level 2數據流。
5. 請根據訂單數量對應的順序號更新Level 2的全部買賣盤數據。如果數量爲0，則需要將該數量對應的訂單價格從Level 2數據流中移除。如遇其他情況，正常更新買賣盤數據即可。
6. 如果收到的消息的sequence與上一條消息不連續，可通過REST請求(GET /api/v1/level2/message/query), start和end間隔不超過500。
[Level 2](#level-2消息拉取) 的Change屬性是一個“price, size, sequence”的字符串值。請注意，size指的是price對應的最新size。當size爲0時，需要將其對應的price從買賣盤中刪除。

**示例**

通過REST請求（Get Order Book）拉取[Level 2](#level-2)買賣盤的快照信息。獲取的快照信息如下：


Sequence：**16**

``` json
  {
    "sequence": 16,
    "asks":[
      ["3988.59",3],
      ["3988.60",47],
      ["3988.61",32],
      ["3988.62",8]
    ],
    "bids":[
      ["3988.51",56],
      ["3988.50",15],
      ["3988.49",100],
      ["3988.48",10]
    ]
  }
```

如上所示，當前拉取的買賣盤快照數據如下：

| 價格   | 數量 | 方向 |
| ------- | ---- | ---- |
| 3988.62 | 8    | 賣4 |
| 3988.61 | 32   | 賣3 |
| 3988.60 | 47   | 賣2 |
| 3988.59 | 3    | 賣1 |
| 3988.51 | 56   | 買1 |
| 3988.50 | 15   | 買2  |
| 3988.49 | 100  | 買3  |
| 3988.48 | 10   | 買4  |

訂閱成功後，您將收到如下變更消息：

``` json
  "data": {
    "sequence": 17,
    "change": "3988.50,buy,44"     // 價格、方向、數量
  }
```
``` json
  "data": {
    "sequence": 18,
    "change": "3988.61,sell,0"     // 價格、方向、數量
  }
```

當前買賣盤快照信息的順序號爲16。丟棄買賣盤數據中順序號小於等於16的數據，回放順序號爲17和18的數據，並更新買賣盤快照信息。現在，您的順序號變成了18，本地買賣盤已最新。

**變更**

1. **將價格3988.50對應的數量變更爲44 （順序號爲17）**
2. **移除價格爲3988.61的數據（順序號爲8）**


變更後，當前買賣盤數據爲最新數據，具體數據如下：

| 價格   | 數量 | 方向 |
| ------- | ---- | ---- |
| 3988.62 | 8    | 賣3 |
| 3988.60 | 47   | 賣2 |
| 3988.59 | 3    | 賣1 |
| 3988.51 | 56   | 買1  |
| 3988.50 | 44   | 買2  |
| 3988.49 | 100  | 買3  |
| 3988.48 | 10  | 買4  |

## 成交記錄 
```json
  {
    "id": 1545910660741,                          
    "type": "subscribe",
    "topic": "/contractMarket/execution:XBTUSDM",
    "response": true                              
  }
```
```json
 {
   "topic": "/contractMarket/execution:XBTUSDM",
   "subject": "match",
   "data": {
        "symbol": "XBTUSDM",				// 合約
        "sequence": 36,						// 順序號，用於判斷websocket消息連續
        "side": "buy",						//  taker的方向 
        "matchSize": 1,           // 成交數量
        "size": 1,							// 訂單剩餘數量
        "price": 3200.00,					// 成交價格
        "takerOrderId": "5c9dd00870744d71c43f5e25",  // taker方訂單ID
        "time": 1553846281766256031,		             // 成交時間 - 納秒
        "makerOrderId": "5c9d852070744d0976909a0c",  // maker方訂單ID
        "tradeId": "5c9dd00970744d6f5a3d32fc"        // 交易號
    }
 }
```
Topic:`/contractMarket/execution:{symbol}`

每撮合一筆訂單，系統就會按照如下格式向您推送消息：

<aside class="spacer8"></aside>



## level2的5檔全量數據推送頻道 
```json
{
   "type": "message",
   "topic": "/contractMarket/level2Depth5:XBTUSDM",
   "subject": "level2",
   "data": {
       "asks":[
         ["9993", "3"],
         ["9992", "3"],
         ["9991", "47"],
         ["9990", "32"],
         ["9989", "8"]
       ],
       "bids":[
         ["9988", "56"],
         ["9987", "15"],
         ["9986", "100"],
         ["9985", "10"],
         ["9984", "10"]
  
       ],
         "ts": 1590634672060667000
    }
 }

```
Topic: `/contractMarket/level2Depth5:{symbol}`

推送頻率爲最多100ms一次。

<aside class="spacer8"></aside>


## level2的50檔全量數據推送頻道
```json
{
   "type": "message",
   "topic": "/contractMarket/level2Depth50:XBTUSDM",
   "subject": "level2",
   "data": {
       "asks":[
         ["9993",3],
         ["9992",3],
         ["9991",47],
         ["9990",32],
         ["9989",8]
       ],
       "bids":[
         ["9988",56],
         ["9987",15],
         ["9986",100],
         ["9985",10],
         ["9984",10]
       ],
        "ts": 1590634672060667000
    }
}
```
Topic:`/contractMarket/level2Depth50:{symbol}`
推送頻率爲最多100ms一次。

<aside class="spacer8"></aside>


## 產品行情數據
Topic:`/contract/instrument:{symbol}`

訂閱此topic，可獲取指定合約產品的行情數據。

```json
 //產品行情數據
  { 
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/instrument:XBTUSDM",   
    "response": true                              
  }
```
<aside class="spacer4"></aside>

### 標記價格、指數價格

```json
  //標記價格、指數價格
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "mark.index.price",
    "data": {
        "granularity": 1000,           //粒度
        "indexPrice": 4000.23,            //指數價格
        "markPrice": 4010.52,           //標記價格
        "timestamp": 1551770400000
    }
  }
```

<aside class="spacer4"></aside>

### 資金費率

```json
 //資金費率
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "funding.rate",
    "data": {
        "granularity": 60000,  //粒度(預測資金費率：1分鐘粒度60000; 資金費率: 8小時粒度28800000)
        "fundingRate": -0.002966,     //資金費率
        "timestamp": 1551770400000
    }
  }
```

<aside class="spacer4"></aside>

## 資金費用結算
Topic:`/contract/announcement`

訂閱此topic，可獲取資金費用結算的推送。

```json
 //系統公告
  { 
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/announcement",   
    "response": true                              
  }
```

<aside class="spacer4"></aside>

### 資金費用結算開始

```json
 //資金費用結算開始
  { 
    "topic": "/contract/announcement",
    "subject": "funding.begin",
    "data": {
        "symbol": "XBTUSDM",                   //合約symbol
        "fundingTime": 1551770400000,          //費用時間
        "fundingRate": -0.002966,             //資金費率
        "timestamp": 1551770400000
    }
  }
```

<aside class="spacer4"></aside>

### 資金費用結算結束

```json
  //資金費用結算結束
  { 
    "type":"message",
    "topic": "/contract/announcement",
    "subject": "funding.end",
    "data": {
        "symbol": "XBTUSDM",                   //合約symbol
        "fundingTime": 1551770400000,          //費用時間
        "fundingRate": -0.002966,            //資金費率
        "timestamp": 1551770410000          
    }
  }
```

<aside class="spacer2"></aside>
<aside class="spacer4"></aside>

## 交易統計定時觸發事件


```json
  //交易統計定時觸發事件
  { 
    "topic": "/contractMarket/snapshot:XBTUSDM",
    "subject": "snapshot.24h",
    "data": {
        "volume": 30449670,            //24小時成交量
        "turnover": 845169919063,      //24小時成交額
        "lastPrice": 3551,           //最新成交價
        "priceChgPct": 0.0043,         //24小時漲跌幅
        "ts": 1547697294838004923      //快照時間，精確到納秒
    }  
  }
```
Topic:`/contractMarket/snapshot:{symbol}`

每 5 秒定時觸發交易統計信息推送。

<aside class="spacer4"></aside>



# 私有頻道

訂閱私人頻道需要`privateChannel=“true”`。

## 訂單私有消息-按照市場獨立推送
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders:XBTUSDM",
   "subject": "symbolOrderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //訂單號
       "symbol": "XBTUSDM",  //合約symbol
       "type": "match",  //消息類型，取值列表: "open", "match", "filled", "canceled", "update" 
       "status": "open", //訂單狀態: "match", "open", "done"
       "matchSize": "", //成交數量 (當類型爲"match"時包含此字段) 
       "matchPrice": "",//成交價格 (當類型爲"match"時包含此字段) 
       "orderType": "limit", //訂單類型, "market"表示市價單", "limit"表示限價單 
       "side": "buy",  // 訂單方向，買或賣 
       "price": "3600",  //訂單價格
       "size": "20000",  //訂單數量
       "remainSize": "20001",  //訂單剩餘可用於交易的數量
       "filledSize":"20000",  //訂單已成交的數量
       "canceledSize": "0",  //  update消息中，訂單減少的數量
       "tradeId": "5ce24c16b210233c36eexxxx",  //交易號(當類型爲"match"時包含此字段) 
       "clientOid": "5ce24c16b210233c36ee321d", //用戶自定義ID 
       "orderTime": 1545914149935808589,  // 下單時間 
       "oldSize ": "15000", // 更新前的數量(當類型爲"update"時包含此字段) 
       "liquidity": "maker", // 成交方向，取taker一方的買賣方向 
       "ts": 1545914149935808589 // 時間戳
   }
}
```
Topic:`/contractMarket/tradeOrders:{symbol}`

* `status`訂單狀態說明:
    - "match": 訂單爲taker時與買賣盤中訂單成交，此時該taker訂單狀態爲match；
    - "open": 訂單存在於買賣盤中；
    - "done": 訂單完成；
<br/>
* `type`消息類型說明:
  - "open": 訂單進入買賣盤時發出的消息；
  - "match": 訂單成交時發出的消息；
  - "filled": 訂單因成交後狀態變爲DONE時發出的消息；
  - "canceled": 訂單因被取消後狀態變爲DONE時發出的消息；
  - "update": 訂單因被修改發出的消息；

<aside class="spacer4"></aside>

## 訂單私有消息
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders",
   "subject": "orderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //訂單號
       "symbol": "XBTUSDM",  //合約symbol
       "type": "match",  //消息類型，取值列表: "open", "match", "filled", "canceled", "update" 
       "status": "open", //訂單狀態: "match", "open", "done"
       "matchSize": "", //成交數量 (當類型爲"match"時包含此字段) 
       "matchPrice": "",//成交價格 (當類型爲"match"時包含此字段) 
       "orderType": "limit", //訂單類型, "market"表示市價單", "limit"表示限價單 
       "side": "buy",  // 訂單方向，買或賣 
       "price": "3600",  //訂單價格
       "size": "20000",  //訂單數量
       "remainSize": "20001",  //訂單剩餘可用於交易的數量
       "filledSize":"20000",  //訂單已成交的數量
       "canceledSize": "0",  //  update消息中，訂單減少的數量
       "tradeId": "5ce24c16b210233c36eexxxx",  //交易號(當類型爲"match"時包含此字段) 
       "clientOid": "5ce24c16b210233c36ee321d", //用戶自定義ID 
       "orderTime": 1545914149935808589,  // 下單時間 
       "oldSize ": "15000", // 更新前的數量(當類型爲"update"時包含此字段) 
       "liquidity": "maker", // 成交方向，取taker一方的買賣方向 
       "ts": 1545914149935808589 // 時間戳
   }
}
```
Topic:`/contractMarket/tradeOrders`

* `status`訂單狀態說明
    - "match": 訂單爲taker時與買賣盤中訂單成交，此時該taker訂單狀態爲match；
    - "open": 訂單存在於買賣盤中；
    - "done": 訂單完成；
<br/>
* `type`消息類型說明
  - "open": 訂單進入買賣盤時發出的消息；
  - "match": 訂單成交時發出的消息；
  - "filled": 訂單因成交後狀態變爲DONE時發出的消息；
  - "canceled": 訂單因被取消後狀態變爲DONE時發出的消息；
  - "update": 訂單因被修改發出的消息；

<aside class="spacer4"></aside>


## 止損單生命週期監聽事件
```json
  {
       "userId": "5cd3f1a7b7ebc19ae9558591", // 不推薦使用, 後續版本將刪除
       "topic": "/contractMarket/advancedOrders", 
       "subject": "stopOrder",
       "data": {
           "orderId": "5cdfc138b21023a909e5ad55", //訂單編號
           "symbol": "XBTUSDM",  //合約symbol
           "type": "open",  // 消息類型: open (止損下單成功), triggered (止損單觸發), cancel (止損單取消)
           "orderType":"stop", // 訂單類型: stop
           "side":"buy", // 訂單買賣方向
           "size":"1000", //數量 
           "orderPrice":"9000",  //訂單價格
           "stop":"up", //止損類型 ("up" 或 "down")
           "stopPrice":"9100", //止損單觸發價格
           "stopPriceType":"TP", //止損單觸發價格類型
           "triggerSuccess": true, //觸發成功標記, 只有triggered類型消息需要
           "error": "error.createOrder.accountBalanceInsufficient", //錯誤碼, 觸發失敗時使用
           "createdAt": 1558074652423  //創建時間
           "ts":1558074652423004000  //創建時間戳納秒
       }
  }
```
Topic:`/contractMarket/advancedOrders`

<aside class="spacer8"></aside>

## 賬戶資金髮生變化

Topic:`/contractAccount/wallet`

### 委託保證金變更事件
```json
  //委託保證金變更事件
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // 不推薦使用, 後續版本將刪除
    "topic": "/contractAccount/wallet",
    "subject": "orderMargin.change",
    "data": {
        "orderMargin": 5923,//當前委託保證金
        "currency":"USDT",//幣種
        "timestamp": 1553842862614
    }
  }
```

<aside class="spacer4"></aside>

### 可用餘額變更事件

```json
   //可用餘額變更事件
  {
    "userId": "xbc453tg732eba53a88ggyt8c", // 不推薦使用, 後續版本將刪除
    "topic": "/contractAccount/wallet",
    "subject": "availableBalance.change",
    "data": {
      "availableBalance": 5923, //當前可用餘額
      "holdBalance": 2312, //凍結金額
      "currency":"USDT",//幣種
      "timestamp": 1553842862614
    }
  }
```

<aside class="spacer4"></aside>

### 提現轉出凍結變更事件

```json
   //提現轉出凍結變更事件
  {
    "userId": "xbc453tg732eba53a88ggyt8c",  // 不推薦使用, 後續版本將刪除
    "topic": "/contractAccount/wallet",
    "subject": "withdrawHold.change",
    "data": {
      "withdrawHold": 5923, //當前提現凍結
      "currency":"USDT",//幣種
      "timestamp": 1553842862614
    }
  }
```

<aside class="spacer4"></aside>
<aside class="spacer2"></aside>

## 倉位變化

Topic: `/contract/position:{symbol}`

### 倉位操作引起的倉位變化
```json
  //倉位操作引起的倉位變化
  { 
    "type": "message",
    "userId": "5c32d69203aa676ce4b543c7",  // 不推薦使用, 後續版本將刪除
    "channelType": "private",
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
      "realisedGrossPnl": 0E-8,                //累加已實現毛利
      "symbol":"XBTUSDM",                      //有效合約代碼
      "crossMode": false,                      //是否全倉
      "liquidationPrice": 1000000.0,           //強平價格
      "posLoss": 0E-8,                         //手動追加的保證金
      "avgEntryPrice": 7508.22,                //平均開倉價格
      "unrealisedPnl": -0.00014735,            //未實現盈虧
      "markPrice": 7947.83,                    //標記價格
      "posMargin": 0.00266779,                 //倉位保證金
      "autoDeposit": false,                    //是否自動追加保證金
      "riskLimit": 100000,                     //風險限額
      "unrealisedCost": 0.00266375,            //未實現價值
      "posComm": 0.00000392,                   //破產費用
      "posMaint": 0.00001724,                  //維持保證金
      "posCost": 0.00266375,                   //倉位價值
      "maintMarginReq": 0.005,                 //維持保證金比例
      "bankruptPrice": 1000000.0,              //破產價格
      "realisedCost": 0.00000271,              //當前累計已實現倉位價值
      "markValue": 0.00251640,                 //標記價值
      "posInit": 0.00266375,                   //槓桿保證金
      "realisedPnl": -0.00000253,              //已實現盈虧
      "maintMargin": 0.00252044,               //倉位保證金
      "realLeverage": 1.06,                    //槓桿倍數
      "changeReason": "positionChange",        //變化原因:marginChange、positionChange、liquidation、autoAppendMarginStatusChange、adl
      "currentCost": 0.00266375,               //當前總倉位價值
      "openingTimestamp": 1558433191000,       //開倉時間
      "currentQty": -20,                       //當前倉位
      "delevPercentage": 0.52,                 //ADL分位數
      "currentComm": 0.00000271,               //當前總費用
      "realisedGrossCost": 0E-8,               //累計已實現毛利價值
      "isOpen": true,                          //是否開倉
      "posCross": 1.2E-7,                      //手動追加的保證金
      "currentTimestamp": 1558506060394,       //當前時間戳
      "unrealisedRoePcnt": -0.0553,            //投資回報率
      "unrealisedPnlPcnt": -0.0553,            //倉位盈虧率
      "settleCurrency": "XBT"                  //結算幣種
      }
  }
```
* `changeReason`說明
    - “marginChange”: 倉位保證金變化;
    - “positionChange”: 倉位變化;
    - “liquidation”: 強平;
    - “autoAppendMarginStatusChange”: 修改是否自動追加保證金;
    - “adl”: 自動減倉;

<aside class="spacer8"></aside>
<aside class="spacer4"></aside>
<aside class="spacer2"></aside>

### 標記價格變化引起的倉位變化
```json
 //標記價格變化引起的倉位變化
  { 
    "userId": "5cd3f1a7b7ebc19ae9558591",  // 不推薦使用, 後續版本將刪除
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
          "markPrice": 7947.83,                   //標記價格
          "markValue": 0.00251640,                 //標記價值
          "maintMargin": 0.00252044,              //倉位保證金
          "realLeverage": 10.06,                   //槓桿倍數
          "unrealisedPnl": -0.00014735,           //未實現盈虧
          "unrealisedRoePcnt": -0.0553,           //投資回報率
          "unrealisedPnlPcnt": -0.0553,            //倉位盈虧率
          "delevPercentage": 0.52,             //ADL分位數
          "currentTimestamp": 1558087175068,       //當前時間戳
          "settleCurrency": "XBT"                  //結算幣種
      }
  }
```

<aside class="spacer8"></aside>


### 資金費用結算

```json
 //資金費用結算
  { 
    "userId": "xbc453tg732eba53a88ggyt8c",  // 不推薦使用, 後續版本將刪除
    "topic": "/contract/position:XBTUSDM",
    "subject": "position.settlement",
    "data": {
        "fundingTime": 1551770400000,          //費用時間
        "qty": 100,                            //倉位數
        "markPrice": 3610.85,                 //結算價格，爲8時刻標記價格，四捨五入到最近合法價格
        "fundingRate": -0.002966,             //結算資金費率
        "fundingFee": -296,                   //資金費用
        "ts": 1547697294838004923,             //當前時間(納秒)
        "settleCurrency": "XBT"                //結算幣種
    }
  }
```

<aside class="spacer8"></aside>

### 風險限額調整結果

```json 
// Adjustment Result of Risk Limit Level
{ 
  "userId": "xbc453tg732eba53a88ggyt8c", 
  "topic": "/contract/position:ADAUSDTM", 
  "subject": "position.adjustRiskLimit", 
  "data": { 
    "success": true, // 是否成功 
    "riskLimitLevel": 1, // 當前風險限額等級
    "msg": "" // 失敗原因 
  }
} 
``` 
* `msg`失敗原因有兩種情況：
    - 1.持倉價值大於風險限額等級額度;
    - 2.餘額不足，保證金追加失敗。

<aside class="spacer2"></aside>

# 登錄 KuCoin
<a href="https://www.kucoin.com">登錄 KuCoin</a>

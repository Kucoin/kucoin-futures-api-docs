# 基本介绍
## 简介
欢迎使用KuCoin合约(KuCoin Futures)开发者文档。 此文档概述了交易功能、市场行情和其他应用开发接口。


KuCoin Futures API分为两部分：**REST API 和 Websocket 实时数据流**

 -  REST API 包含三个类别：**用户（私有）、交易（私有）、市场数据（公共）**
 -  Websocket包含两类：**公共频道和私人频道**

## 更新预告

**为了进一步提升API安全性，KuCoin已经升级到了V2版本的API-KEY，验签逻辑也发生了一些变化，建议到[API管理页面](https://futures.kucoin.com/api)添加并更换到新的API-KEY。KuCoin将继续支持使用老的API-KEY到2021年05月01日。请查看“消息签名”，了解更多详情**

#### 2021.08.18
* 移除了[POST /api/v2/transfer-out](#kucoin-2)接口的输出参数BizNo.
* 修正了[GET /api/v1/account-overview](#8ec888deb4)接口的返回字段marginBalance描述.

#### 2021.07.15
* 修改[请求频率限制](#26435b04cf)

#### 2021.03.18
* 账户资金消息推送 /contractAccount/wallet 的subject:availableBalance.change增加了holdBalance字段。 <br/>

#### 2021.03.02
* 停用已废弃的level-3推送：/contractMarket/level3:{symbol}, 推荐使用/contractMarket/level3v2:{symbol}
* 新增实时ticker消息：/contractMarket/tickerV2:{symbol} 提供实时 ticker 推送
* websocket私有频道新增按照合约独立推送订单消息topic：/contractMarket/tradeOrders:{symbol}

#### 2021.02.25
* Level-3全部买卖盘 GET /api/v2/level3/snapshot 的请求频率限制为：每个IP每分钟1次请求。 <br/>

#### 2021.02.07
* level3消息更新： /contractMarket/level3:{symbol} 将不再支持2021年2月7日后上线的合约，请升级使用 /contractMarket/level3v2:{symbol} 。 <br/>


#### 2021.01.14
* 接口权限修改，划转功能由提现权限调整为交易权限，影响的接口： <br/>
    POST /api/v2/transfer-out  <br/>


#### 2020.12.23
* 接口合约返回对象添加lowPrice(24小时最低成交价)、highPrice(24小时最高成交价)、priceChgPct(24小时涨跌幅)、priceChg(24小时涨跌价格) 属性. 影响的接口: <br/>
    GET /api/v1/contracts/active  <br/>
    GET /api/v1/contracts/{symbol}  <br/>

#### 2020.12.17
* 为了降低下单延迟, 系统不再校验 clientOId 的唯一性, 通过 api 下单clientOId参数重复会正常下单成功

#### 2020.08.24
* 增加查询20或100深度的买卖盘API

#### 2020.06.18
* 增加Websocket推送消息增加channelType字段: public(公共频道，默认)、private(用户私有频道)、session(会话频道)；
* 弃用: 三个月后移除私用频道topic中({topic}:privateChannel:{userId})和私有消息中的userId
* 合约信息中添加24小时成交量, 24小时成交额, 活动仓位数, 影响接口
    GET /api/v1/contracts/active
    GET /api/v1/contracts/{symbol}

#### 2020.06.12
* level3消息格式全新改版，提供更全面的消息字段
   接收新版level3消息，请订阅："/contractMarket/level3v2:{symbol}"
* 增加level3新版本全量数据获取接口
   GET /api/v2/level3/snapshot
* 增加订单私有消息频道：/contractMarket/tradeOrders
* 增加level2的5档全量数据推送频道：/contractMarket/level2Depth5:{symbol}
* 增加level2的50档全量数据推送频道：/contractMarket/level2Depth50:{symbol}

#### 2020.06.03
* 品牌升级合约域名为KuCoin合约(KuCoin Futures)

#### 2020.06.01
* 新增获取服务状态接口，新增的接口：
    GET /api/v1/status
#### 2020.05.13
* 新增获取合约K线数据接口，新增的接口：
    GET /api/v1/kline/query

#### 2020.04.30
* 修改入参chain 的默认值（从OMNI 修改为ERC20）, 影响的接口:<br/>
    POST /api/v1/withdrawals


#### 2020.04.28
* 按 id 查询订单接口支持按用户订单编号查询, 影响的接口:<br/>
    GET /api/v1/orders/{}


#### 2020.04.15
* 账务接口返回增加字段：memo(地址标识), 影响的接口:<br/>
     GET /api/v1/withdrawal-list <br/>

     
#### 2020.03.30
USDT结算合约上线, 交易所从只支持一个币种 XBT, 变为同时支持 XBT 和 USDT 两个币种, 具体修改如下:
*  接口订单记录对象返回增加新属性: settleCurrency(结算币种), status(订单状态: "open" 或 "done"), updatedAt(订单最近更新时间), 
    orderTime(下单时间纳秒). 影响的接口:   

     GET /api/v1/orders  
     GET /api/v1/orders/{order-id}  
     GET /api/v1/stopOrders   
     GET /api/v1/recentDoneOrders  

* 接口成交记录对象返回增加新属性: settleCurrency(结算币种), tradeTime(交易时间纳秒). 影响的接口:   
    GET /api/v1/fills   <br/>
    GET /api/v1/recentFills  <br/>

* 接口活动订单价值统计返回对象添加settleCurrency(结算币种) 属性. 影响的接口: <br/>
    GET /api/v1/openOrderStatistics  <br/>

* 接口查询资金费用历史返回对象添加settleCurrency(结算币种) 属性. 影响的接口: <br/>
    GET /api/v1/funding-history   <br/>

* 接口合约返回对象添加maxLeverage(合约最大杠杆倍数) 属性. 影响的接口: <br/>
    GET /api/v1/contracts/active  <br/>
    GET /api/v1/contracts/{symbol}  <br/>
* 新增止损的Websocket高级委托私有频道推送 (topic: /contractMarket/advancedOrders, subject: stopOrder)


   ```json
      {
        "userId": "5cd3f1a7b7ebc19ae9558591", // 不推荐使用, 后续版本将删除 
        "topic": "/contractMarket/advancedOrders",  
        "subject": "stopOrder",
        "data": {
            "orderId": "5cdfc138b21023a909e5ad55", //订单编号
            "symbol": "XBTUSDM",  //合约编号
            "type": "open",  // 消息类型 open(下单) triggered(触发) cancel(取消)
            "orderType":"stop", //订单类型: 止损
            "side":"buy", //买卖方向
            "size":"1000", //数量
            "orderPrice":"9000",  //订单价格, 市价单为null
            "stop":"up", //止损订单类型
            "stopPrice":"9100", //止损订单触发价格
            "stopPriceType":"TP", //止损订单触发价格类型
            "triggerSuccess": true, //触发是否成功, 仅triggered类型使用
            "error": "error.createOrder.accountBalanceInsufficient", //错误码, 仅triggered类型触发失败使用
            "createdAt": 1558074652423,  //订单创建时间
            "ts":1558074652423004000  //消息时间纳秒
        }
      }
   ```


* 接口仓位信息记录返回增加属性：settleCurrency(结算货币), 影响的接口:<br/>
     GET /api/v1/position <br/>
     GET /api/v1/positions <br/>

* 仓位的Websocket推送增加字段：settleCurrency(结算货币), 影响topic "/contract/position:{symbol}" 的如下subject:<br/>
     position.change position.settlement<br/>

* 接口查询资金记录入参增加currency(币种) 用来过滤对应盈亏币种的记录;<br/>
     接口查询资金记录对象返回增加属性：currency(币种)，影响的接口:<br/>
     GET /api/v1/transaction-history <br/>

* <font color="#dd0000">账务接口增加参数：memo(没有memo的币种，提现的时候不可以传递memo), chain[可选] 针对一币多链的币种，可通过chain指定提现链。比如， USDT存在的链有 OMNI, ERC20, TRC20 影响的接口: <br/>
    POST /api/v1/withdrawals</font><br/>


* 账务接口入参增加过滤币种：currency(币种), 影响的接口:<br/>
     GET /api/v1/account-overview <br/>
     GET /api/v1/deposit-list <br/>
     GET /api/v1/withdrawal-list <br/>
     GET /api/v1/transfer-list  <br/>

* 账务接口记录对象返回增加字段：currency(币种),影响的接口:<br/>
     GET /api/v1/account-overview <br/>

* 新增账务按币种转出资金接口:<br/>
     POST /api/v2/transfer-out,新接口比老接口增加currency(币种)参数, 用于指定转出币种(XBT/USDT)
    (原来的POST /api/v1/transfer-out接口还可以使用, 但是需要升级到新接口支持 USDT 币种转出)

* 账务WebsocketAPI接口推送增加字段：currency(结算货币), 影响topic "/contractAccount/wallet" 的如下subject:<br/>
     availableBalance.change <br/>
     withdrawHold.change<br/>
     orderMargin.change<br/>

* 弃用level3局部数据查询接口，建议使用level3数据快照接口。
  
     GET /api/v1/level3/message/query

<br/>

####2020.03.05
* 修复 accountEquity 和 marginBalance 含义，修复后 accountEquity为账户总权益，= unrealisedPNL + marginBalance;

## 客户端开发库

使用客户端开发库可快速集成到KuCoin Futures API。

**官方软件开发工具包（SDK）**

- [PHP SDK](https://github.com/Kucoin/kucoin-futures-php-sdk)

- [Java SDK](https://github.com/Kucoin/kucoin-futures-java-sdk)

- [Node.js SDK](https://github.com/Kucoin/kucoin-futures-node-sdk)

- [Python SDK](https://github.com/Kucoin/kucoin-futures-python-sdk)

- [Go SDK](https://github.com/Kucoin/kucoin-futures-go-sdk)

  

## 沙盒测试环境
沙盒是测试环境，用于测试API连接和Web交易，并提供交易的所有功能。在沙盒中，您可以使用虚假资金来测试交易功能。
沙盒环境中的登录会话和API密钥与生产环境完全分离。您可以使用沙盒环境中的Web界面来创建API密钥。

**注意:**在沙盒环境中注册后，您将收到系统在您的帐户中自动充值的一定数量的测试资金（XBT）。如果您想交易，请将资产从储蓄账户转移到交易账户。这些资金仅用于测试目的，不能提现。

**用于API测试的沙盒URL：**
* 网址：https://sandbox-futures.kucoin.com 
* REST API 连接地址: **https://api-sandbox-futures.kucoin.com** (https://sandbox-api.kumex.com has been Deprecated)


## 请求频率限制

当请求频率超过限制频率时，系统将返回code为**429**的超频错误码。
<aside class="notice">如果接口请求频率超过限制，你的IP或账户会被限制使用10s。</aside>

### REST API

对需要校验API权限的私有接口，限制账号userid。不需要检验权限API，则限制IP。
<aside class="notice">接口有特定请求频率限制说明，以特定说明为准。</aside>

### WEBSOCKET

**连接数量**
- 每个用户ID同时建立的连接数：**≤ 50个**

**连接次数**

 - 连接请求次数限制：**每分钟 30次 请求**

**上行消息条数** 

 - 向服务器发送指令条数限制：**每10秒 100条**

**订阅topic数量** 

 - 每个连接最大可订阅topic数量限制：**100个topics**

## 做市激励计划
KuCoin Futures为专业做市商提供做市激励计划。 参与该计划，可以获得以下激励：

 - 做市商返佣
 - 每月嘉奖前十名表现优良的做市团队，高额回馈最佳做市商
 - 直接市场接入（提供private link接入）

具有良好做市策略和大交易量的用户欢迎参与此长期做市激励计划。如果您的账户在过去30天内的交易量超过5000 BTC，请提供以下信息以发送电子邮件至**mm@kucoin.com**，邮件主题为“Futures Market Maker Application”：

 - 提供平台帐户（需要电子邮件，无需推荐关系）
 - 过去30天内在任何交易所交易的交易量证明或VIP级别的证明
 - 请简要说明做市的方法（不需要详细说明）以及估算做市订单量的百分比。

## VIP快速通道
具有良好做市策略和大交易量的用户欢迎参与长期做市商计划。 如果您的帐户资产大于10BTC，请提供以下信息以发送电子邮件至：**vip_futures@kucoin.com**进行做市商申请;

 - 提供平台帐户（需要电子邮件，无需推荐关系）;
 - 提供其他交易平台做市商交易量的截图（例如30天内的交易量，或VIP级别等）; 
 - 请简要说明做市的方法，不需要详细说明；

---
# REST API
## 请求说明

### API服务器地址

REST API对用户、交易及市场数据均提供了接口。

基本URL： **https://api-futures.kucoin.com** (**https://api.kumex.com** 已过期不推荐使用)

<aside class="notice">为了遵守当地法律要求，使用中国IP的用户不允许访问以上URL。</aside>

请求URL由基本URL和指定接口端点组成。


### 接口端点

每个接口都提供了对应的端点，可在**HTTP请求**模块下获取。


对于**GET请求**，只需将请求参数拼接在请求路径后面。

例如：对于“仓位” 接口，其默认端点为/api/v1/position。请求“合约”参数（XBTUSDM）时，该端点将变为：**/api/v1/position?symbol=XBTUSDM**。因此，您最终请求的URL应为：**https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM**。

### 请求
所有的请求和响应的内容类型都是application/json。  


除非另行说明，所有的时间戳参数均以Unix时间戳毫秒计算。如：**1544657947759**

### 参数

对于**GET**和**DELETE**请求，需将参数拼接在请求URL中（如：**/api/v1/position?symbol=XBTUSDM**）。

对于**POST**和**PUT**请求，需将参数以JSON格式拼接在请求主体中（如： {"side":"buy"}）。

**注意：不要在JSON字符串中添加空格。**


### 错误返回

系统会返回HTTP错误代码或系统错误代码。您可根据返回的参数消息排查错误原因。


#### HTTP错误码

```json
  {
    "code": "400100",
    "msg": "Invalid Parameter."
  }
```

代码 | 含义
---------- | -------
400 | Bad Request -- 请求格式不正确
401 | Unauthorized -- 无效API Key
403 | Forbidden -- 请求被禁止
404 | Not Found -- 找不到指定资源
405 | Method Not Allowed -- 您请求资源的方法不正确
415 | Content-Type -- 请求类型必须为application/json类型
429 | Too Many Requests -- 请求频率超出限制
500 | Internal Server Error -- 服务器出错，请稍后再试
503 | Service Unavailable -- 服务器维护中，请稍后再试



#### 系统错误码

代码 | 含义
---------- | -------
400001 | Any of KC-API-KEY, KC-API-SIGN, KC-API-TIMESTAMP, KC-API-PASSPHRASE is missing in your request header -- 请求头中缺少KC-API-KEY、KC-API-SIGN、KC-API-TIMESTAMP和KC-API-PASSPHRASE参数
400002 | KC-API-TIMESTAMP Invalid -- 请求时间与服务器时差超过5秒
400003 | KC-API-KEY not exists -- API-KEY不存在
400004 | KC-API-PASSPHRASE error -- API-PASSPHRAE不正确
400005 | Signature error -- 签名错误，请检查您的签名
400006 | The requested ip address is not in the api whitelist -- 请求IP不在API白名单中
400007 | Access Denied -- API权限不足，无法访问该URI目标地址。
404000 | Url Not Found -- 找不到请求资源
400100 | Parameter Error -- 请求参数不合法
411100 | User are frozen -- 用户已被冻结，请联系帮助中心
500000 | Internal Server Error -- 服务器出错，请稍后再试


系统返回的HTTP状态码不是200时，接口返回会显示错误码。调用成功时，系统将返回code和data字段；调用失败时，系统将返回code和msg字段。您可根据返回的参数消息排查错误。



### 成功返回

当系统返回**HTTP状态码200和系统代码200000**时，表示响应成功，返回结果如下：

```json
  {
    "code": "200000",
  }
```


### 分页器

KuCoin Futures 使用了**Pagination**和**HasMore**两种分页器，来返回数组的REST请求。


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

**Pagination**允许使用当前页数获取结果，非常适用于获取实时数据。如/api/v1/deposit-list、/api/v1/orders及/api/v1/fills端点均默认返回第一页结果。如需获取更多数据，请根据当前返回的数据指定其他分页，然后再进行请求。

**示例** 
GET /api/v1/orders?currentPage=1&pageSize=50

**参数**

参数 | 默认值 | 含义
---------- | ------- | ------
currentPage | 1 | 当前页码
pageSize | 50 | 每页数据条数



#### HasMore
HasMore分页器采用滑动窗口机制，通过在一个数据流上滑动固定大小的窗口以获取分页数据，返回的数据中有一个字段HasMore，标志是否还有更多的数据。HasMore分页器每次滑动的时间相等，性能高效，非常适合于流式实时数据的查询。


**参数**

| 参数 | 数据类型    | 含义                                |
| --------- | ------- | --------------- |
| offset    | -       | 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页
| forward   | boolean | 滑动方向，TRUE查询大于offset方向的数据      
| maxCount  | int     | 每次滑动的最大数据条数                    

**示例**
GET /api/v1/interest/query?symbol=.XBTINT&offset=1558079160000&forward=true&maxCount=10

## 类型说明 
### 时间戳 
API中的所有时间戳以Unix时间戳毫秒为单位返回（如：**546658861000**）
## 接口认证
### 创建API KEY
通过接口进行请求前，您需在Web端创建API-KEY。创建成功后，您需妥善保管好以下三条信息：


* Key
* Secret
* Passphrase

Key和Secret由KuCoin Futures随机生成并提供，Passphrase是您在创建API时使用的密码。以上信息若遗失将无法恢复，需要重新申请API KEY。


### 权限

您可在[KuCoin Futures](https://futures.kucoin.com) Web端管理API权限。API权限分为以下几类：


* **通用权限** - 开启后，API可使用大部分GET请求来获取数据。
* **交易权限** - 开启后，可通过API进行下单/撤单和管理仓位。
* **转账权限** - 开启后，可通过API进行提现。需注意，开启此权限后，不需要两步验证，即可进行转账。


### 创建请求

所有REST参数的请求头需包含以下内容：

* **KC-API-KEY** API KEY是字符串类型。
* **KC-API-SIGN** 签名（请查看“消息签名”，了解更多详情）
* **KC-API-TIMESTAMP** 请求的时间戳
* **KC-API-PASSPHRASE** 创建API时填写的API密码
* **KC-API-KEY-VERSION** API-KEY版本号，可通过[API管理](https://futures.kucoin.com/api)页面查看版本号


### 消息签名

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
        "KC-API-KEY-VERSION": 2
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
        "KC-API-KEY-VERSION": 2,
        "Content-Type": "application/json" # specifying content type or using json=data in request
    }
    response = requests.request('post', url, headers=headers, data=data_json)
    print(response.status_code)
    print(response.json())
```
请求头中的 KC-API-SIGN: 

1. 使用 API-Secret 对
{timestamp + method+ endpoint + body} 拼接的字符串进行**HMAC-sha256**加密。
2. 再将加密内容使用 **base64** 编码。
                       

请求头中的 **KC-API-PASSPHRASE**:

1. 对于V1版的API-KEY，请使用明文传递
2. 对于V2版的API-KEY，需要将KC-API-KEY-VERSION指定为2，并将passphrase使用API-Secret进行**HMAC-sha256**加密，再将加密内容通过**base64**编码后传递

注意：

* 加密的 timestamp 需要和请求头中的KC-API-TIMESTAMP保持一致
* 进行加密的body需要和Request Body中的内容保持一致
* 请求方法需要大写
* 对于 GET, DELETE 请求，endpoint 需要包含请求的参数（/api/v1/deposit-address?currency=XBT）。如果没有请求体（通常用于GET请求），请使用空字符串“”表示请求体


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

### 选择时间戳

请求头的**KC-API-TIMESTAMP**必须为Unix UTC时间，需精确到毫秒（如：1547015186532）。

服务器请求的时间戳与API服务器时差必须控制在**5秒**以内，否则请求会因过期而被服务器拒绝。如果服务器与API服务器之间存在时间偏差，请使用平台提供的服务器时间接口，获取API服务器的时间。

---
# 用户
此部分需进行签名验证。

# 账户
## 获取账户概览
```json
  { 
    "code": "200000",
    "data": {
      "accountEquity": 99.8999305281, //账户总权益 = 账户余额 + 未实现盈亏
      "unrealisedPNL": 0, //未实现盈亏
      "marginBalance": 99.8999305281, //账户余额 = 仓位保证金 + 委托保证金 + 转出提现冻结 + 可用余额 - 未实现盈亏
      "positionMargin": 0, //仓位保证金
      "orderMargin": 0, //委托保证金
      "frozenFunds": 0, //转出提现冻结
      "availableBalance": 99.8999305281, //可用余额
      "currency":"XBT" //币种
    }
  }
```
### HTTP请求
GET /api/v1/account-overview

### 请求示例
GET /api/v1/account-overview?currency=XBT

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
currency | String | [可选] 币种 **XBT或USDT,默认XBT**
### API权限
该接口需要**通用权限**

### 频率限制
此接口针对每个账号请求频率限制为**30次/3s**

## 查询资金记录

```json
  {
    "code": "200000",
    "data": {
      "hasMore": false,//是否存在下一页
      "dataList": [{
        "time": 1558596284040, //业务发生时间
        "type": "RealisedPNL", //类型
        "amount": 0, //交易金额
        "fee": null,//手续费
        "accountEquity": 8060.7899305281, //账户权益
        "status": "Pending", //状态，当前8h有过未平仓合约
        "remark": "XBTUSDM",//合约
        "offset": -1, //起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页
        "currnecy": "XBT" //币种
      },
      {
        "time": 1557997200000,
        "type": "RealisedPNL",
        "amount": -0.000017105,
        "fee": 0,
        "accountEquity": 8060.7899305281,
        "status": "Completed",//状态，已完成的8h周期
        "remark": "XBTUSDM",
        "offset": 1,
        "currency": "XBT"
     }]
    }
  }

```
当存在未平仓合约时，第一页返回数据包含的未平仓合约记录状态为**Pending**，是当前8小时结算周期内的已实现盈亏。翻页使用当前页返回的最小**offset**，作为请求**offset**参数值传入，实现翻页。

### HTTP请求
GET /api/v1/transaction-history

### 请求示例
GET /api/v1/transaction-history?offset=1&forward=true&maxCount=50

### API权限
该接口需要**通用权限**

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
startAt| long | [可选] 开始时间（毫秒）
endAt| long | [可选]  截止时间（毫秒） 
type | String | [可选] 类型 **RealisedPNL-已实现盈亏；Deposit-充值,Withdrawal-提现；TransferIn-转入；TransferOut-转出**
offset | long | [可选] 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页
maxCount | long | [可选] 每页显示条数 默认50
currency | String | [可选] 币种 **XBT或USDT**
forward | boolean | [可选] 是否前向查询，**true**或者**false**，默认为**true** 


### 返回

**type:**  类型包括 **RealisedPNL-已实现盈亏,Deposit-充值,Withdrawal-提现,TransferIn-转入 ,TransferOut-转出**

**status:**  状态包括 **Completed，Pending**


# 充值
## 获取充币地址

```json
  {
    "code": "200000",
    "data": {
      "address": "0x78d3ad1c0aa1bf068e19c94a2d7b16c9c0fcd8b1",//充币地址
      "memo": null//地址标签，如果返回为空，则该币种没有memo。当您在其他平台申请提现到KuCoin的时候，如果该币种有memo(tag)，需要填写memo，确保能准确入账到您到账户
   } 
}
```

### HTTP请求
GET /api/v1/deposit-address

### 请求示例
GET /api/v1/deposit-address?currency=XBT

### API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
currency | String | 币种（**XBT 或USDT**）


## 获取充值列表
```json
  {
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 1,
      "totalPage": 1,
      "items": [{
        "currency": "XBT",//币种
        "status": "SUCCESS",//状态类型：PROCESSING, WALLET_PROCESSING, SUCCESS, FAILURE
        "address": "5CD018972914B66104BF8842",//充值地址
        "isInner": false,//是否站内充值
        "amount": 1,//充值金额
        "fee": 0,//充值手续费
        "walletTxId": "5CD018972914B66104BF8842",//钱包交易txId
        "createdAt": 1557141673000 //充值时间
      }]
    }
  }
```

默认返回最新第一页数据

### HTTP请求
GET /api/v1/deposit-list

### 请求示例
GET /api/v1/deposit-list?currentPage=1&pageSize=50&status=PROCESSING?currency=XBT

### API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
startAt | long | [可选]  开始时间（毫秒）
endAt | long | [可选]  截止时间（毫秒） 
status | String | [可选]  状态 **PROCESSING, SUCCESS, FAILURE**
currency | String | [可选]  币种 **XBT,USDT**

# 提现
## 获取提现额度

```json
 {
    "code": "200000",
    "data": {
      "currency": "XBT",//币种
      "limitAmount": 2,//24H提现总额度
      "usedAmount": 0,//24H已使用提现额度
      "remainAmount": 2,//24H可提现额度
      "availableAmount": 99.89993052,//可用余额
      "withdrawMinFee": 0.0005,//提现手续费
      "innerWithdrawMinFee": 0,// 站内提现手续费
      "withdrawMinSize": 0.002,//最小提现数量
      "isWithdrawEnabled": true,//是否可以提现
      "precision": 8//提现金额精度
    }
 }
```

### HTTP请求
GET /api/v1/withdrawals/quotas

### 请求示例
GET /api/v1/withdrawals/quotas?currency=XBT

### API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
currency | String | 币种（**XBT或USDT**）


## 发起提现

```json
  {
   "code": "200000",
    "data": {
    "withdrawalId": "5bffb63303aa675e8bbe18f9" //提现id`可以用于后续取消提现 
    }
  }
```

### HTTP请求
POST /api/v1/withdrawals

### 请求示例
POST /api/v1/withdrawals

### API权限
该接口需要**转出权限**

### 参数
参数 | 数量类型 | 含义
--------- | ------- | -----------
currency  | String | 币种（**XBT,USDT**）
address   | String | 提现地址
amount | Big  | 提现数量
isInner|	boolean| [可选] 是否站内提现。当提现地址是站内地址，可通过此字段选择是否上区块链，默认false表示表示上链，ture表示站内提现
remark | String | [可选]  备注
chain | String | [可选]  币种的链名。例如，对于USDT，现有的链有OMNI、ERC20、TRC20。默认值为ERC20。这个参数用于区分多链的币种，单链币种不需要。
memo | String | [可选]  地址标签memo(tag)，如果返回为空，则该币种没有memo。对于没有memo的币种，在提现的时候不可以传递memo


## 获取提现列表
```json
  {
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 10,
      "totalPage": 1,
      "items": [{
        "withdrawalId": "5cda659603aa67131f305f7e",//提现唯一标识，可用于取消提现
        "currency": "XBT",//币种
        "status": "FAILURE",//状态
        "address": "3JaG3ReoZCtLcqszxMEvktBn7xZdU9gaoJ",//提现地址
        "memo": "",//提现地址标识
        "isInner": true,//是否站内提现
        "amount": 1,//提现金额
        "fee": 0,//提现手续费
        "walletTxId": "",//钱包交易txId
        "createdAt": 1557816726000,//提现时间
        "remark": "测试.",//提现备注
        "reason": "Assets freezing failed."//失败原因
      }]
    }
  }
```


### HTTP请求
GET /api/v1/withdrawal-list

### 请求示例
GET /api/v1/withdrawal-list?currentPage=1&pageSize=50&status=PROCESSING

### API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
status | String | [可选] 状态. **PROCESSING, WALLET_PROCESSING, SUCCESS, FAILURE**
startAt | long | [可选] 开始时间（毫秒）
endAt | long | [可选]  截止时间（毫秒） 
currency | String | [可选]  币种 **XBT,USDT**


## 取消提现
只有在**PROCESSING**状态时才可能取消提现。

### HTTP请求
DELETE /api/v1/withdrawals/{withdrawalId}

### HTTP请求
DELETE /api/v1/withdrawals/5cda659603aa67131f305f7e

### API权限
该接口需要**转出权限** 

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
withdrawalId | String | 路径参数，发起提现返回**withdrawalId**

# 划转
## 转出到KuCoin储蓄账户
```json
  { 
    "code": "200000",
    "data": {
      "applyId": "5bffb63303aa675e8bbe18f9" //转出申请id
    }  
  }
```
 转出金额将从KuCoin Futures账户扣除，转出前请确保KuCoin Futures账户可用余额充足。接口响应表示转出成功后，系统将返回applyId。此ID可用于取消转出申请。

### HTTP请求
POST /api/v1/transfer-out （已废弃，请使用 POST /api/v2/transfer-out）
### 请求示例
POST /api/v1/transfer-out
### API权限
该接口需要**提现权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
bizNo | String | 业务请求号，建议使用UUID
amount | Number | 转出金额


## 转出到KuCoin储蓄账户
```json
  { 
    "code": "200000",
    "data": {
      "applyId": "5bffb63303aa675e8bbe18f9" //转出申请id
    }  
  }
```
 转出金额将从KuCoin Futures账户扣除，转出前请确保KuCoin Futures账户可用余额充足。接口响应表示转出成功后，系统将返回applyId。此ID可用于取消转出申请。

### HTTP请求
POST /api/v2/transfer-out
### 请求示例
POST /api/v2/transfer-out
### API权限
该接口需要**交易权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
amount | Number | 转出金额
currency | String | 币种 **XBT,USDT**

## 查询转出申请记录
```json
  { 
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 6,
      "totalPage": 1,
      "items": [{
        "applyId": "5cd53be30c19fc3754b60928", //转出申请id
        "currency": "XBT", //币种
        "status": "SUCCESS", //状态 PROCESSING-处理中，SUCCESS-成功, FAILURE-失败
        "amount": "0.01", //交易金额
        "reason": "", //失败原因
        "offset": 31986850860000, // 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页
        "createdAt": 1557769977000 //提交申请时间
      }]
    }   
  }
```
默认查询第一页数据
### HTTP请求
GET /api/v1/transfer-list

### 请求示例
GET /api/v1/transfer-list?currentPage=1&pageSize=50&status=PROCESSING?currency=XBT

###API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
startAt | long | [可选] 开始时间（毫秒）
endAt | long | [可选]  截止时间（毫秒） 
status | String | [可选] 状态 **PROCESSING-处理中，SUCCESS-成功, FAILURE-失败**
currency | String | [可选]  币种 **XBT,USDT**


## 取消转出

```json
 { 
    "code": "200000",
    "data":null
  }
```

只能在**PROCESSING**状态时可取消转出请求。
### HTTP请求
DELETE /api/v1/cancel/transfer-out

### 请求示例
DELETE /api/v1/cancel/transfer-out?applyId=5cd53be30c19fc3754b60928

### API权限
该接口需要**通用权限**

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
applyId | String | 转出申请ID(发起转出返回)


# 交易
此部分需进行签名验证。

# 订单
## 下单

```json
  {
    "code": "200000",
    "data": {
      "orderId": "5bd6e9286d99522a52e458de"
      }
  }
```

有两种订单类型：限价单和市价单。只有当账户资金充足时，下单才会成功。下单后，系统将根据指定订单类型和参数，冻结相应资金，直至订单完结。


注意：系统将在订单进入买卖盘前冻结手续费用。查看[成交记录](#获取成交记录)，了解更多详情。


**请勿在JSON字符串中添加空格。**

### 下单限制：

一个账户中，每种合约最多可下**100**个限价单和**50**个止损单。



### HTTP 请求

POST /api/v1/orders

### API权限
该接口需获取**交易权限**。

### 频率限制
此接口针对每个账号请求频率限制为**30次/3s**

### 参数

| 参数     | 数据类型   | 含义                                                  |
| --------- | ------ | -----------------------|
| clientOid | String | 唯一的订单ID，可用于识别订单。如：UUID<br/>只能包含数字、字母、下划线（_）或 分隔线（-）|
| side      | String | **buy** 或 **sell**    |
| symbol    | String | 有效合约代码。如：XBTUSDM                  |
| type      | String | [可选] 订单类型，包括**limit**或**market**,默认limit。|
| leverage  | String | 下单杠杆倍数
| remark    | String | [可选] 下单备注，字符长度不能超过100 个字符（UTF-8）。|
| stop      | String | [可选] 触发价格的两种类型。下跌至某个价格（**down**），或上涨至某个价格（**up**）。设置后，就必须设置**stopPrice**和**stopPriceType** 参数。 |
| stopPriceType  | String | [可选] 止损单触发价类型，包括**TP**、**IP**和**MP**， 只要设置**stop**参数，就必须设置此属性。|
| stopPrice | String | [可选] 只要设置**stop**参数，就必须设置此属性。|
| reduceOnly | boolean | [可选] 只减仓标记, 默认值是 false 。值为true时，需要设置平仓数量|
| closeOrder | boolean | [可选] 平仓单标记, 默认值是 false 。值为true时，代表仓位全平|
| forceHold | boolean | [可选] 强制冻结标记（减仓同样适用）,可将订单留在买卖盘中而不受仓位变化的影响。默认值是 false |

参数详细描述见术语解释。



#### 限价单需额外请求的参数


| 参数       | 数据类型    | 含义    |
| ----------- | ------- | ----------------------------- |
| price       | String  | 限价单的价格   |
| size        | Integer  | 订单数量。必须是一个正数。 |
| timeInForce | String  | [可选] 订单时效策略，包括GTC、IOC（默认为GTC）。 |
| postOnly    | boolean | [可选] 只挂单的标识。选择postOnly，不允许选择hidden和iceberg。当订单时效为IOC策略时，该参数无效。  |
| hidden      | boolean | [可选] 订单不会在买卖盘中展示。选择hidden，不允许选择postOnly。|
| iceberg    | boolean | [可选] 仅设置可见的部分会显示在买卖盘中。选择iceberg，不允许选择postOnly。|
| visibleSize | Integer  | [可选] 冰山单最大可展示的数量。  |

#### 市价单需额外请求的参数


| 参数 | 数据类型 | 含义
| --------- | ------- | -----------
| size | Integer | [可选] 下单数量


### 示例
POST /api/v1/orders

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

### 术语解释

### 合约

合约必须是KuCoin Futures支持的合约，如XBTUSDM。

### 用户订单ID

ClientOid字段是客户端创建的唯一的ID（推荐使用UUID），只能包含数字、字母、下划线（_） 和 分隔线（-）。这个字段会在获取订单信息时返回。您可使用clientOid来标识您的订单。ClientOid不同于服务端创建的订单ID。请不要使用同一个clientOid发起请求。**clientOid最长不得超过40个字符**。

请妥善记录服务端创建的orderId，以用于查询订单状态的更新。


### TYPE
您在下单时指定的订单类型，决定了您是否需要请求其他参数，同时还会影响到撮合引擎的执行。如果您在下单时未指定订单类型，系统将默认按照限价单执行。

下限价单时，您需指定限价单的价格（**price**）和数量（**size**）。系统将根据市场行情以指定或更优价格撮合该订单。如果订单未能被立即撮合，将继续留买卖盘中，直至被撮合或被用户取消。

与限价单不同，市价单价格会随着市场价格波动而变化。下市价单时，您无需指定价格，只需指定合约数量。市价单会立即成交，不会进入买卖盘。所有市价单都是taker，需支付taker费用。



###  LEVERAGE

杠杆表示订单冻结保证金时使用的杠杆倍数，如果是平仓单可以不设置此属性。 

### STOP ORDERS
止损单，是指当市场价格达到了设置的止损触发价格（stopPrice）触发合约后，订单按照市场价格或指定价格买进或卖出相应数量的合约。止损单分为两种，向下止损（down）和向上止损（up）。


**止损单类型**

向下止损（**down**）：当价格下跌至或低于设置的止损价格（stopPrice）时，订单将被触发。

向上止损（**up**）：当价格上涨至或高于设置的止损价格（stopPrice）时，订单将被触发。

**止损单触发价类型**：

1. 最新交易价格（TP）
2. 标记价格（MP）
3. 指数价格（IP）

最新交易价格，指最近一次的订单成交价格，该价格可在最新撮合消息中找到。

**mark price** 和 **index price** 可以通过相关指数服务OPEN API获取。

请注意，当止损单被触发后，订单将按照指定订单类型，以市价单或限价单成交。

进行止盈止损交易时，系统不会提前冻结您的账户资金。订单被触发后，如果账户余额不足时，订单会被自动取消。


### PRICE

下单价格必须是合约tickSize的整数倍，否则下单时会报错。tickSize是合约价格的最小精度单位。合约价格不能超过合约最高价格（maxPrice）规定。**KuCoin Futures平台采用了IEPR价格保护机制（Immediately Executable Price Range, 简称为IEPR）**，合约购买价格不得超过**1.05*标记价格**的价格上限，合约卖出价格不得低于**0.95 *标记价格**的价格下限。

下市价单不需要价格字段。



### SIZE

订单数量是合约的张数, 订单数量不能小于合约最小数量（lotSize）或大于合约最大数量（maxOrderQty）。订单数量必须是lotSize的整数倍，否则下单时系统会报错。订单数量表示要买入或卖出的合约数量大小。
每张 XBTUSDTM 合约对应 0.001 BTC, 每张 XBTUSDM 合约对应 1 USD.



### 订单时效

订单时效是一种交易时使用的特殊策略，用于设定订单在被撮合或取消前的生效时间。订单时效策略分为两种：

1. 取消前有效（Good Till Canceled，简称为GTC）
2. 立即成交或取消（Immediate Or Cancel，简称为IOC）

**取消前有效（GTC）**：指委托将持续有效直到被手动取消。如果用户在交易时没有设置该字段，系统将默认按照GTC策略执行订单。

**立即成交或取消（IOC）**：指如果委托全部成交或部分成交，未成交部分将被立即取消。


**注意：成交也包含自成交**

### 被动委托

选择被动委托的订单，增加了市场的流动性，将按照maker费用执行。

### 隐藏单&冰山单

您可在高级设置中设置隐藏单和冰山单（冰山单是一种特殊形式的隐藏单）。进行限价单和限价止损单交易时，您可选择按照隐藏单或冰山单执行。

隐藏单不会展示在买卖盘上。

与隐藏单不同，冰山单分为可见和隐藏两部分。进行冰山单交易时，需设置可见订单数量。冰山单最小可见数量是总订单量的1/20。

进行撮合时，冰山单的可见部分会首先被撮合，当可见部分被全部撮合后，另一部分隐藏订单将浮出，直至订单全部成交。

**注意：**

1. 系统将对隐藏和冰山单收取taker费用。
2. 如果您同时设定了冰山单和隐藏单，您的订单将默认作为冰山单处理。


### 冻结订单

下单时，系统会根据订单的价格和数量冻结一定的账户金额，作为仓位保证金和交易费。没有冻结的订单不能进行加仓撮合, 只能进行平仓撮合。当撮合引擎发现订单零冻结数量超过反向仓位数量时, 会取消多余零冻结订单, 以保证没有零冻结订单加仓撮合。

交易实际产生的手续费会在订单成交时确定。如果您取消了一个未完全成交的委托，则其相应的剩余的已冻结资金会被释放到可用余额中。

订单的加仓数量需要冻结，平仓数量不需要冻结，平仓单（closeOrder is true）和只减仓订单（reduceOnly is true）不需要冻结。


### CLOSE ORDER 平仓
平仓单会把当前用户的所有仓位修改为 0。平仓单标记为 true 时，不需要传入买卖方向和订单数量参数，系统会根据用户当前仓位的方向和数量动态决定订单的买卖方向和订单数量。由于减仓不需要冻结，所以有也不需要传入杠杆参数。

### CLOSE ONLY 只减仓
被标记为只减仓的订单只会被以减仓的方向撮合（不会增加仓位），当用户仓位数量小于只减仓订单数量时，多余数量的订单数量会被撮合引擎取消掉。

### FORCE HOLD 强制冻结
强制冻结订单费用。下当前仓位的反向订单时，会冻结相应资金以确保订单不会因为没有足够的资金而被撮合引擎自动取消。


### 订单生命周期

当下单请求因请求成功（撮合引擎已收到订单）或（因余额不足、参数不合法等原因）被拒绝时，HTTP 请求会进行响应。下单成功，返回订单ID，订单将被撮合，可能会部分或全部成交。部分成交后，订单剩余为未成交部分会变成等待撮合（**Active**）状态（不包括使用立即成交或取消[**IOC**]的订单）。已完全成交的订单会变成“已完成”（**Done**）状态。

订阅市场数据频道的用户可使用订单ID（orderId）和用户订单ID（clientOid）来识别消息。


### 响应

下单成功后会返回一个订单ID（order id）。下单成功即表示撮合引擎已收到订单。

**未被撮合的订单将继续留在买卖盘上直至被完全撮合或取消。**


## 单个撤单

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

提交以下请求，可撤销单个订单（包括止损单）。


系统收到取消订单的请求后，将向您返回结果。撮合引擎将按照订单顺序处理取消订单的申请。您可检查订单状态或更新推送消息以了解您的请求是否已被处理。

返回订单ID为服务端创建的订单ID，而非客户端创建的ID（**clientOid**）。

如果订单无法被取消（因已成交、已被取消等原因），系统将报错进行错误响应，在返回字段中显示错误原因。


### HTTP 请求
DELETE /api/v1/orders/{order-id}

### 示例
DELETE /api/v1/orders/5cdfc120b21023a909e5ad52

### API权限 ###
该接口需获取**交易权限**。

### 频率限制
此接口针对每个账号请求频率限制为**40次/3s**

## 限价单批量撤单

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

该接口可用于取消全部当前委托（不包括止损单）。返回结果将列出被取消订单的订单ID（orderID）。


### HTTP请求
DELETE /api/v1/orders

### 示例
DELETE /api/v1/orders?symbol=XBTUSDM

### API权限
该接口需获取**交易权限**。

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

### 参数

参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol | String | [可选] 删除指定合约的所有限价单。

使用查询参数可删除指定合约的全部订单。如果没有指定symbol参数，将取消全部限价单。


## 止损单批量撤单

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

删除所有未被触发的止损单。请求成功后，系统将返回已被取消的止损单的订单ID列表。取消已触发止损单，请使用"限价单批量撤单"接口。



### HTTP请求
DELETE /api/v1/stopOrders

### 示例
DELETE /api/v1/stopOrders?symbol=XBTUSDM


### API权限
该接口需获取**交易权限**。

### 参数

参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol | String | [可选] 取消指定合约的所有未触发止损单。

使用查询参数可删除指定合约的全部订单。如果没有指定symbol参数，将取消全部止损单。

## 查询订单列表

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
          "id": "5cdfc138b21023a909e5ad55", //订单编号
          "symbol": "XBTUSDM",  //合约编号
          "type": "limit",   //类型, 市价单或限价单
          "side": "buy",  //买卖方向
          "price": "3600",  //下单价格
          "size": 20000,  //数量
          "value": "56.1167227833",  //订单价值
          "filledValue": "0",  //已经成交订单价值
          "filledSize": 0,  //已经成交订单数量
          "stp": "",  //stp 类型
          "stop": "",  //止损订单类型
          "stopPriceType": "",  //止损订单触发价格类型
          "stopTriggered": false,  //止损订单是否触发标志
          "stopPrice": null,  //止损订单触发价格
          "timeInForce": "GTC",  //timeInForce类型
          "postOnly": false,  //postOnly标志
          "hidden": false,  //隐藏单标志
          "iceberg": false,  //冰山单标志
          "visibleSize": null,  //冰山单可见数量
          "leverage": "20",  //杠杆倍数
          "forceHold": false,  //强制冻结单标志
          "closeOrder": false, //平仓单标志
          "closeOnly": false,  //只减仓单标志
          "clientOid": "5ce24c16b210233c36ee321d",  //客户订单编号
          "remark": null,  //注解
          "isActive": true,  //未完成订单标志
          "cancelExist": false,  //订单存在取消数量标志
          "createdAt": 1558167872000,  //创建时间
          "settleCurrency": "XBT", //结算币种
          "status": "open", //订单状态: “open” 或 “done”
          "updatedAt": 1558167872000, //最新更新时间
          "orderTime": 1558167872000000000 //下单时间纳秒
        }
      ]
    }  
 }
```

结果返回当前所有委托。

### HTTP请求
GET /api/v1/orders

### 示例
GET /api/v1/orders?status=active  
获取所有活动订单  

### API权限
该接口需获取**通用权限**。

### 频率限制
此接口针对每个账号请求频率限制为**30次/3s**

### 参数

参数 | 数据类型 | 含义
--------- | ------- | -----------
status | String |[可选] 订单状态。活跃（**active**）状态或已完成单（**done**）状态。默认设置为“已完成”状态。请求发送成功后，仅返回指定状态的委托列表。
symbol |String|[可选] 仅返回指定的委托列表，如：XBTUSDM。
side | String | [可选] **buy** 或 **sell** 
type | String | [可选] 订单类型，包括：限价单、市价单、限价止损、市价止损。**limit**, **market**, **limit_stop** or **market_stop** 
startAt | long | [可选] 开始时间（毫秒）
endAt | long | [[可选]  截止时间（毫秒） 

请使用查询参数获取指定合约的订单。

请求返回数据使用了**Pagination**分页方式。


#### 订单状态和结算
在买卖盘上，所有限价委托都处于活跃（**Active**）状态，从买卖盘上移除的订单则被标记为已完成（**Done**）状态。订单被成交后到入账，因系统清算可能会有毫秒级别的延迟。


您可发送请求，查询任一状态的订单。如果您未指定状态参数，系统将默认返回“已完结”（**Done**）状态的订单。

查询“活跃”状态的订单，没有时间限制。但查询“已完成”状态的订单时，您只能获取 7 * 24 小时时间范围内的数据（即：查询时，开始时间到结束时间的时间范围不能超过24 * 7小时）。若超出时间范围，系统会报错。如果您只指定了结束时间，没有指定开始时间，系统将按照 24小时的范围自动计算开始时间（开始时间=结束时间-24小时）并返回相应数据，反之亦然。


#### POLLING 轮询
对于高频交易的用户，建议您在本地缓存和维护一份自己的活动委托列表，并使用市场数据流实时更新自己的订单信息。

如果需要低延时获取自己的最近成交历史订单记录, 请使用“24小时内完成订单列表”小节中的接口（Get List of Orders Completed in 24H）。 此接口返回的历史订单可能存在一定的延迟。


## 查询未触发止损订单列表

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
            "id": "5cdfc138b21023a909e5ad55", //订单编号
            "symbol": "XBTUSDM",  //合约编号
            "type": "limit",   //类型, 市价单或限价单
            "side": "buy",  //买卖方向
            "price": "3600",  //下单价格
            "size": 20000,  //数量
            "value": "56.1167227833",  //订单价值
            "filledValue": "0",  //已经成交订单价值
            "filledSize": 0,  //已经成交订单数量
            "stp": "",  //stp 类型
            "stop": "",  //止损订单类型
            "stopPriceType": "",  //止损订单触发价格类型
            "stopTriggered": false,  //止损订单是否触发标志
            "stopPrice": null,  //止损订单触发价格
            "timeInForce": "GTC",  //timeInForce类型
            "postOnly": false,  //postOnly标志
            "hidden": false,  //隐藏单标志
            "iceberg": false,  //冰山单标志
            "visibleSize": null,  //冰山单可见数量
            "leverage": "20",  //杠杆倍数
            "forceHold": false,  //强制冻结单标志
            "closeOrder": false, //平仓单标志
            "closeOnly": false,  //只减仓单标志
            "clientOid": "5ce24c16b210233c36ee321d",  //客户订单编号
            "remark": null,  //注解
            "isActive": true,  //未完成订单标志
            "cancelExist": false,  //订单存在取消数量标志
            "createdAt": 1558167872000,  //创建时间
            "settleCurrency": "XBT", //结算币种
            "status": "open", //订单状态: “open”
            "updatedAt": 1558167872000 //最新更新时间
        }
      ]
    }
 }
```
您可通过该接口查询未触发止损订单列表。已经触发的止损订单通过一般订单接口查询。

### HTTP请求
GET /api/v1/stopOrders

### 示例
GET /api/v1/stopOrders?symbol=XBTUSDM
发送该请求可获取XBTUSDM合约的未触发止损订单

### API权限
该接口需获取**通用权限**。

### 参数

参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol |String|[可选] 仅返回指定的委托列表，如：XBTUSDM。
side | String | [可选]**buy** 或 **sell** 
type | String | [可选]**limit** 或 **market** 
startAt | long | [可选] 开始时间（毫秒）
endAt | long | [可选]  截止时间（毫秒） 

请使用查询参数获取指定合约的订单。

**请求返回数据使用了Pagination分页方式。**

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




## 24小时内完成订单列表

```json
  {
    "code": "200000",
    "data": [
          {
            "id": "5cdfc138b21023a909e5ad55", //订单编号
            "symbol": "XBTUSDM",  //合约编号
            "type": "limit",   //类型, 市价单或限价单
            "side": "buy",  //买卖方向
            "price": "3600",  //下单价格
            "size": 20000,  //数量
            "value": "56.1167227833",  //订单价值
            "filledValue": "56.1167227833",  //已经成交订单价值
            "filledSize": 20000,  //已经成交订单数量
            "stp": "",  //stp 类型
            "stop": "",  //止损订单类型
            "stopPriceType": "",  //止损订单触发价格类型
            "stopTriggered": true,  //止损订单是否触发标志
            "stopPrice": null,  //止损订单触发价格
            "timeInForce": "GTC",  //timeInForce类型
            "postOnly": false,  //postOnly标志
            "hidden": false,  //隐藏单标志
            "iceberg": false,  //冰山单标志
            "visibleSize": null,  //冰山单可见数量
            "leverage": "20",  //杠杆倍数
            "forceHold": false,  //强制冻结单标志
            "closeOrder": false, //平仓单标志
            "closeOnly": false,  //只减仓单标志
            "clientOid": "5ce24c16b210233c36ee321d",  //客户订单编号
            "remark": null,  //注解
            "isActive": false,  //未完成订单标志
            "cancelExist": false,  //订单存在取消数量标志
            "createdAt": 1558167872000,  //创建时间
            "settleCurrency": "XBT", //结算币种
            "status": "done", //订单状态: “open” 或 “done”
            "updatedAt": 1558167872000, //最新更新时间
            "orderTime": 1558167872000000000 //下单时间纳秒
          }
    ]
 }
```

Get a list of 1000 orders in the last 24 hours. 如果需要低延时获取自己的最近成交历史订单, 请使用此接口。 使用此接口可获取过去24小时内最近完成的1000笔订单。

### HTTP请求
GET /api/v1/recentDoneOrders

### 示例
GET /api/v1/recentDoneOrders

### API权限
该接口需获取**通用权限**。

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

## 单个订单详情

```json
  {
    "code": "200000",
    "data": {
      "id": "5cdfc138b21023a909e5ad55", //订单编号
      "symbol": "XBTUSDM",  //合约编号
      "type": "limit",   //类型, 市价单或限价单
      "side": "buy",  //买卖方向
      "price": "3600",  //下单价格
      "size": 20000,  //数量
      "value": "56.1167227833",  //订单价值
      "filledValue": "56.1167227833",  //已经成交订单价值
      "filledSize": 20000,  //已经成交订单数量
      "stp": "",  //stp 类型
      "stop": "",  //止损订单类型
      "stopPriceType": "",  //止损订单触发价格类型
      "stopTriggered": true,  //止损订单是否触发标志
      "stopPrice": null,  //止损订单触发价格
      "timeInForce": "GTC",  //timeInForce类型
      "postOnly": false,  //postOnly标志
      "hidden": false,  //隐藏单标志
      "iceberg": false,  //冰山单标志
      "visibleSize": null,  //冰山单可见数量
      "leverage": "20",  //杠杆倍数
      "forceHold": false,  //强制冻结单标志
      "closeOrder": false, //平仓单标志
      "closeOnly": false,  //只减仓单标志
      "clientOid": "5ce24c16b210233c36ee321d",  //客户订单编号
      "remark": null,  //注解
      "isActive": false,  //未完成订单标志
      "cancelExist": false,  //订单存在取消数量标志
      "createdAt": 1558167872000,  //创建时间
      "settleCurrency": "XBT", //结算币种
      "status": "open", //订单状态: “open” 或 “done”
      "updatedAt": 1558167872000, //最新更新时间
      "orderTime": 1558167872000000000 //下单时间纳秒
    }
}
```
您可通过订单号获取单个订单的详情（包括止损单）。


### HTTP请求
GET /api/v1/orders/{order-id}?clientOid={client-order-id}

### 示例
GET /api/v1/orders/5cdfc138b21023a909e5ad55 (通过 orderId 获取订单信息), </br>
GET /api/v1/orders/byClientOid?clientOid=eresc138b21023a909e5ad59 (通过用户传入的订单 id查询订单信息)


### API权限
该接口需获取**通用权限**。

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

# 成交记录

## 获取成交记录

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
            "symbol": "XBTUSDM",  //合约编号
            "tradeId": "5ce24c1f0c19fc3c58edc47c",  //交易编号
            "orderId": "5ce24c16b210233c36ee321d",  //订单编号
            "side": "sell",  //买卖方向
            "liquidity": "taker",  //流动性类型 taker or maker
            "price": "8302",  //成交价格
            "size": 10,  //成交数量
            "value": "0.001204529",  //成交价值
            "feeRate": "0.0005",  //费率
            "fixFee": "0.00000006",  //固定费用
            "feeCurrency": "XBT",  //收费币种
            "stop": "",  //止损单类型标记
            "fee": "0.0000012022",  //交易费用
            "orderType": "limit",  //订单类型
            "tradeType": "trade",  //交易类型: trade, liquidation, ADL or settlement
            "createdAt": 1558334496000,  //创建时间
            "settleCurrency": "XBT", //结算币种
            "tradeTime": 1558334496000000000 //交易时间纳秒
          }
      ]
    }
}
```

您可通过此接口获取最近成交的订单列表。

### HTTP请求
GET /api/v1/fills

### 示例
GET /api/v1/fills

### API权限
该接口需获取**通用权限**。

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

### 参数


参数 | 数据类型 | 含义
--------- | ------- | -----------
orderId | String |[可选] 如果指定了订单号，会忽略其他参数。
symbol | String |[可选] 合约symbol 
side | String |[可选] **buy** or **sell** 
type | String |[可选] **limit**, **market**, **limit_stop** or **market_stop** 
startAt | long |[可选] 开始时间（毫秒）
endAt | long |[可选]  截止时间（毫秒） 

如果需要低延时获取自己的最近成交历史记录, 请使用24小时成交列表接口。 此接口返回的历史成交可能存在一定的延迟。
请使用查询参数获取指定合约的已成交订单。

**数据时间范围**

您可检索一周时间范围内的数据您范围内检索数据（默认从最近一天开始算起）。 若检索时间范围超过一周，系统将提示您超过时间限制。如果查询只提供开始时间没有提供结束时间，系统将自动计算结束时间（结束时间=开始时间+ 24小时），反之亦然。


**手续费**

KuCoin Futures平台上的订单分为两种类型：**Taker** 和 **Maker**。Taker单会与买卖盘上的已有订单立即成交，而Maker单则相反，会一直留在买卖盘中等待撮合。Taker单消耗了市场的流动性，因此会被收取taker费用，而Maker单增加了市场的流动性，会被收取较低的手续费甚至获得手续费补贴。请注意：市价单、冰山单和隐藏单都会被扣除taker手续费。


下单时，系统会预冻结您账户中的taker费用。流动性（liquidity）字段中的参数说明了订单将会被收取taker还是maker费用。

加仓订单需要预冻费用，减仓订单不冻结任何费用。加仓订单冻结的费用包括仓位保证金、开仓交易费和平仓交易费。订单撮合成功后, 如果加仓会扣除加仓交易费, 如果是平仓会扣取平仓交易费。


## 最近成交记录

```json
  {
    "code": "200000",
    "data": 
    [ {
     "symbol": "XBTUSDM",  //合约编号
     "tradeId": "5ce24c1f0c19fc3c58edc47c",  //交易编号
     "orderId": "5ce24c16b210233c36ee321d",  //订单编号
     "side": "sell",  //买卖方向
     "liquidity": "taker",  //流动性类型 taker or maker
     "price": "8302",  //成交价格
     "size": 10,  //成交数量
     "value": "0.001204529",  //成交价值
     "feeRate": "0.0005",  //费用率
     "fixFee": "0.00000006",  //固定费用
     "feeCurrency": "XBT",  //收费币种
     "stop": "",  //止损单类型标记
     "fee": "0.0000012022",  //交易费用
     "orderType": "limit",  //订单类型
     "tradeType": "trade",  //交易类型, 可能是交易, 强平 或ADL 
     "createdAt": 1558334496000,  //创建时间
     "settleCurrency": "XBT", //结算币种
     "tradeTime": 1558334496000000000 //交易时间纳秒
    }
   ]
  }
```

如果需要低延时获取自己的最近成交历史记录, 请使用此接口。
使用此接口可获取过去24小时内最近完成的1000笔订单。

### HTTP请求
GET /api/v1/recentFills

### 示例
GET /api/v1/recentFills

### API权限
该接口需获取**通用权限**。

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

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

## 活动订单价值统计

```json
{
  "code": "200000",
  "data": {
    "openOrderBuySize": 20,  //未完成买单总数量
    "openOrderSellSize": 0,  //未完成卖单总数量
    "openOrderBuyCost": "0.0025252525",  //未完成买单价值总量
    "openOrderSellCost": "0",  //未完成卖单价值总量
    "settleCurrency": "XBT" //结算币种
    }  
  }
```

此接口可用于统计用户所有活动订单的数量和价值信息

### HTTP请求
GET /api/v1/openOrderStatistics

### 示例
GET /api/v1/openOrderStatistics

### API权限
该接口需获取**通用权限**。

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol |String| 指定合约的活动订单，如：XBTUSDM的活动订单。


# 仓位

## 获取仓位详情

```json
 	{
    'id': '5e81a7827911f40008e80715',                //仓位id
    'symbol': 'XBTUSDTM',  													 //合约symbol
    'autoDeposit': False,  													 //是否自动追加保证金
    'maintMarginReq': 0.005,  											 //维持保证金率
    'riskLimit': 2000000,  													 //风险限额
    'realLeverage': 5.0,  													 //杠杆倍数
    'crossMode': False,  													 	 //是否全仓模式
    'delevPercentage': 0.35,  											 //ADL分位数
    'openingTimestamp': 1623832410892,  						 //开仓时间
    'currentTimestamp': 1623832488929,  						 //当前时间戳
    'currentQty': 1,  											 				 //当前仓位数量
    'currentCost': 40.008,  												 //当前仓位价值
    'currentComm': 0.0240048,  											 //当前仓位总费用
    'unrealisedCost': 40.008,  											 //未实现价值
    'realisedGrossCost': 0.0,  											 //累计已实现毛利价值
    'realisedCost': 0.0240048,  										 //累计已实现仓位价值
    'isOpen': True,  																 //是否开仓
    'markPrice': 40014.93,  												 //标记价格
    'markValue': 40.01493,  												 //标记价值
    'posCost': 40.008,  														 //仓位价值
    'posCross': 0.0,  															 //追加到仓位的保证金
    'posInit': 8.0016,  														 //杠杆保证金
    'posComm': 0.02880576,  												 //破产费用
    'posLoss': 0.0,  																 //资金费用减少的资金
    'posMargin': 8.03040576,  											 //仓位保证金
    'posMaint': 0.23284656,  											 	 //维持保证金
    'maintMargin': 8.03733576, 											 //包含未实现盈亏的仓位保证金
    'realisedGrossPnl': 0.0,  											 //累计已实现毛利
    'realisedPnl': -0.0240048,  										 //已实现盈亏
    'unrealisedPnl': 0.00693,  											 //未实现盈亏
    'unrealisedPnlPcnt': 0.0002,  									 //仓位盈亏率
    'unrealisedRoePcnt': 0.0009,  									 //投资回报率
    'avgEntryPrice': 40008.0,  											 //平均开仓价格
    'liquidationPrice': 32211.0,  									 //强平价格
    'bankruptPrice': 32006.0,  											 //破产价格
    'settleCurrency': 'USDT',  											 //结算币种
	}
```

获取用户指定合约的仓位详情

### HTTP请求

GET /api/v1/position

### 示例
GET /api/v1/position?symbol=XBTUSDM

### API权限
该接口需获取**通用权限**。

### 参数

| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称    |

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

## 获取用户仓位列表

```json
    {
    'id': '5e81a7827911f40008e80715',                //仓位id
    'symbol': 'XBTUSDTM',  													 //合约symbol
    'autoDeposit': False,  													 //是否自动追加保证金
    'maintMarginReq': 0.005,  											 //维持保证金率
    'riskLimit': 2000000,  													 //风险限额
    'realLeverage': 5.0,  													 //杠杆倍数
    'crossMode': False,  													 	 //是否全仓模式
    'delevPercentage': 0.35,  											 //ADL分位数
    'openingTimestamp': 1623832410892,  						 //开仓时间
    'currentTimestamp': 1623832488929,  						 //当前时间戳
    'currentQty': 1,  											 				 //当前仓位数量
    'currentCost': 40.008,  												 //当前仓位价值
    'currentComm': 0.0240048,  											 //当前仓位总费用
    'unrealisedCost': 40.008,  											 //未实现价值
    'realisedGrossCost': 0.0,  											 //累计已实现毛利价值
    'realisedCost': 0.0240048,  										 //累计已实现仓位价值
    'isOpen': True,  																 //是否开仓
    'markPrice': 40014.93,  												 //标记价格
    'markValue': 40.01493,  												 //标记价值
    'posCost': 40.008,  														 //仓位价值
    'posCross': 0.0,  															 //追加到仓位的保证金
    'posInit': 8.0016,  														 //杠杆保证金
    'posComm': 0.02880576,  												 //破产费用
    'posLoss': 0.0,  																 //资金费用减少的资金
    'posMargin': 8.03040576,  											 //仓位保证金
    'posMaint': 0.23284656,  											 	 //维持保证金
    'maintMargin': 8.03733576, 											 //包含未实现盈亏的仓位保证金
    'realisedGrossPnl': 0.0,  											 //累计已实现毛利
    'realisedPnl': -0.0240048,  										 //已实现盈亏
    'unrealisedPnl': 0.00693,  											 //未实现盈亏
    'unrealisedPnlPcnt': 0.0002,  									 //仓位盈亏率
    'unrealisedRoePcnt': 0.0009,  									 //投资回报率
    'avgEntryPrice': 40008.0,  											 //平均开仓价格
    'liquidationPrice': 32211.0,  									 //强平价格
    'bankruptPrice': 32006.0,  											 //破产价格
    'settleCurrency': 'USDT',  											 //结算币种
	}
```

使用该请求，可获取用户所有的仓位列表。

### HTTP请求
GET /api/v1/positions

### 示例
GET /api/v1/positions

### API权限
该接口需获取**通用权限**。

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

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

## 更改自动追加保证金状态
### HTTP请求
POST /api/v1/position/margin/auto-deposit-status

### 示例
POST /api/v1/position/margin/auto-deposit-status

### API权限
该接口需获取**通用权限**。

### 参数
| 参数  | 数据类型    | 含义 |
| ------ | ------- | ----------- |
| symbol | String  | 合约名称    |
| status | boolean | 状态        |

## 手动追加保证金
### HTTP请求
POST /api/v1/position/margin/deposit-margin

### 示例
POST /api/v1/position/margin/deposit-margin

### API权限
该接口需获取**通用权限**。

### 参数

| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称    |
| margin | Number | 保证金数量（增加保证金不能低于0.00001667XBT）|
| bizNo  | String | 业务唯一id  |

# 资金费用

## 查询资金费用历史

```json
  {
    "dataList": [
      {
        "id": 36275152660006,                //id
        "symbol": "XBTUSDM",                  //合约symbol
        "timePoint": 1557918000000,          //时间点(毫秒)
        "fundingRate": 0.000013,             //资金费率
        "markPrice": 8058.27,                //标记价格
        "positionQty": 10,                   //结算时的仓位数
        "positionCost": -0.001241,           //结算时的仓位价值
        "funding": -0.00000464,              //结算的资金费用，正数表示收入；负数表示支出
        "settleCurrency": "XBT"             //结算币种

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
    "hasMore": true                         //是否还有下一页
  }
```

查询资金费用历史

### HTTP请求
GET /api/v1/funding-history

### API权限
该接口需获取**通用权限**。

### 频率限制
此接口针对每个账号请求频率限制为**9次/3s**

### 参数

| 参数     | 数据类型    | 含义                                                  |
| --------- | ------- | -------------- |
| symbol    | String  | 合约symbol      |
| startAt | long    | [可选] 开始时间（毫秒）                      |
| endAt   | long    | [可选]  截止时间（毫秒）  |
| reverse   | boolean | [可选] 是否逆序查询， **true** 或者 **false**，默认为**true** |
| offset    | long    | [可选] 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页                   |
| forward   | boolean | [可选] 是否前向查询，**true**或者**false**，默认为**true** |
| maxCount  | int     | [可选] 最大记录条数，默认为10                          |

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

# 市场数据

市场数据是公共的，不需要验证签名。

# 合约
## 获取开放合约列表

```json
  {
    "code": "200000",
    "data": {
      "baseCurrency": "XBT",  //基础货币
      "fairMethod": "FundingRate", //合理标记方式
      "fundingBaseSymbol": ".XBTINT8H",  //基础货币symbol
      "fundingQuoteSymbol": ".USDINT8H", //计价货币symbol
      "fundingRateSymbol": ".XBTUSDMFPI8H",  //资金费率symbol
      "indexSymbol": ".KXBT",    //指数symbol
      "initialMargin": 0.01, //起始保证金比例
      "isDeleverage": true,   //是否支持自动减仓
      "isInverse": true,  //是否是反向合约
      "isQuanto": false,   //是否quanto
      "lotSize": 1,   //最小合约数量
      "maintainMargin": 0.005,    //维持保证金比例
      "makerFeeRate": -0.00025,  //maker手续费
      "makerFixFee":  -0.0000000200,   //maker手续费固定值
      "markMethod": "FairPrice", //标记方式
      "maxOrderQty": 1000000,   //最大委托数量
      "maxPrice": 1000000.0000000000,  //最大下单价格    
      "maxRiskLimit": 200,  //最大风险限额(以XBT为单位)
      "minRiskLimit": 200,  //最小风险限额(以XBT为单位)
      "multiplier": -1,    //合约乘数
      "quoteCurrency": "USD",  //计价货币
      "riskStep": 100,  //风险限额递增值(以XBT为单位)
      "rootSymbol": "XBT", //合约系列
      "status": "Open", //合约状态
      "symbol": "XBTUSDM", //合约名称
      "takerFeeRate": 0.0005,  //taker手续费
      "takerFixFee": 0.0000000600,   //taker手续费固定值
      "tickSize": 1,  //最小的价格变化
      "type": "FFWCSX",    //合约类型
      "maxLeverage": 100,   //最大可用杠杆倍数
      "volumeOf24h": 14848115, //24 小时成交量
      "turnoverOf24h": 1590.20278373, //24 小时成交额
      "openInterest": "10621721",  //活动仓位数
      "lowPrice": 19445, //24 小时最低成交价
      "highPrice": 23862, //24 小时最高成交价
      "priceChgPct": 1000, //24 小时涨跌幅
      "priceChg": 0.1646 //24 小时涨跌价格
   }
  }
```

获取所有开放的合约信息

### HTTP请求
GET /api/v1/contracts/active

### 示例
GET /api/v1/contracts/active

### 参数
无

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

## 获取合约详细信息
```json
  {
    "code": "200000",
    "data": {
      "baseCurrency": "XBT",  //基础货币
      "fairMethod": "FundingRate", //合理标记方式
      "fundingBaseSymbol": ".XBTINT8H",  //基础货币symbol
      "fundingQuoteSymbol": ".USDINT8H", //计价货币symbol
      "fundingRateSymbol": ".XBTUSDMFPI8H",  //资金费率symbol
      "indexSymbol": ".KXBT",    //指数symbol
      "initialMargin": 0.01, //起始保证金比例
      "isDeleverage": true,   //是否支持自动减仓
      "isInverse": true,  //是否是反向合约
      "isQuanto": false,   //是否quanto
      "lotSize": 1,   //最小合约数量
      "maintainMargin": 0.005,    //维持保证金比例
      "makerFeeRate": -0.00025,  //maker手续费
      "makerFixFee":  -0.0000000200,   //maker手续费固定值
      "markMethod": "FairPrice", //标记方式
      "maxOrderQty": 1000000,   //最大委托数量
      "maxPrice": 1000000.0000000000,  //最大下单价格    
      "maxRiskLimit": 200,  //最大风险限额(以XBT为单位)
      "minRiskLimit": 200,  //最小风险限额(以XBT为单位)
      "multiplier": -1,    //合约乘数
      "quoteCurrency": "USD",  //计价货币
      "riskStep": 100,  //风险限额递增值(以XBT为单位)
      "rootSymbol": "XBT", //合约系列
      "status": "Open", //合约状态
      "symbol": "XBTUSDM", //合约名称
      "takerFeeRate": 0.0005,  //taker手续费
      "takerFixFee": 0.0000000600,   //taker手续费固定值
      "tickSize": 1,  //最小的价格变化
      "type": "FFWCSX",    //合约类型
      "maxLeverage": 100,   //最大可用杠杆倍数
      "volumeOf24h": 14848115, //24 小时成交量
      "turnoverOf24h": 1590.20278373, //24 小时成交量
      "openInterest": "10621721",  //活动仓位数
      "lowPrice": 19445, //24 小时最低成交价
      "highPrice": 23862, //24 小时最高成交价
      "priceChgPct": 1000, //24 小时涨跌幅
      "priceChg": 0.1646 //24 小时涨跌价格
   }
  }
```

使用此接口可获取指定合约的信息


### HTTP请求
GET /api/v1/contracts/{symbol}

### 示例
GET /api/v1/contracts/XBTUSDM

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol | String | 路径参数。合约名称

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

# 行情快照

## 获取实时行情

```json
  {
    "code": "200000",
    "data": {
      "sequence": 1001,				// 顺序号
      "symbol": "XBTUSDM",				// 合约
      "side": "buy",					// 成交方向 - taker
      "size": 10,						// 成交数量
      "price": "7000.0",				// 成交价格
      "bestBidSize": 20,				// 最佳买一价总量
      "bestBidPrice": "7000.0",		// 最佳买一价
      "bestAskSize": 30,				// 最佳卖一价总量
      "bestAskPrice": "7001.0",		// 最佳卖一价
      "tradeId": "5cbd7377a6ffab0c7ba98b26",  // 交易号
      "ts": 1550653727731			   // 成交时间 - 纳秒
    }
  }
```
返回的实时行情数据将包含最近成交价格、最近成交数量、最近交易号、taker方向，成交后的最佳买一价及其数量、成交后最佳卖一价及其数量，以及成交时间等。您也可通过该websocket获取该数据。返回数据中的，顺序号可用于判断websocket推送的消息的连续性。



### HTTP请求
GET /api/v1/ticker

### 示例
GET /api/v1/ticker?symbol=XBTUSDM

### 参数
参数 | 数据类型 | 含义
--------- | ------- | -----------
symbol | string | 合约名称


# 委托买卖盘
## 获取全部买卖盘 - Level 2

```json
{
	"code": "200000",
	"data": {
		"symbol": "XBTUSDM",		// 合约
		"sequence": 100,			// 快照序号
		"asks": [
			["5000.0", 1000],	// 价格、数量
			["6000.0", 1983]		// 价格、数量
		],
		"bids": [
			["3200.0", 800],		// 价格、数量
			["3100.0", 100]		// 价格、数量
		],
    "ts": 1604643655040584408  // 时间戳
	}
}
```

此接口获取指定合约的所有活动委托的快照。
Level 2 买卖盘上的买单和卖单均按照价格汇总，每个价格下仅返回一个根据价格汇总的挂单量。
此接口将返回全部的买卖盘数据。

该功能适用于专业交易员，因为该过程将使用较多服务器资源及流量，访问频率受到了严格控制。
为保证本地买卖盘数据为最新数据，在获取Level 2快照后，请使用[Websocket](#Level-2-市场行情)推送的增量消息来更新Level 2买卖盘。
返回值中，卖盘数据是按照价格从低到高排序的，买盘数据是按照价格从高到低排序的。

### HTTP请求
GET /api/v1/level2/snapshot

### 示例
GET /api/v1/level2/snapshot?symbol=XBTUSDM

### 频率限制
此接口针对每个账号请求频率限制为**30次/3s**

### 参数
| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称    |

## 获取部分买卖盘 - Level 2
```json
{
	"code": "200000",
	"data": {
		"symbol": "XBTUSDM",		// 合约
		"sequence": 100,			// 快照序号
		"asks": [
			["5000.0", 1000],	// 价格、数量
			["6000.0", 1983]		// 价格、数量
		],
		"bids": [
			["3200.0", 800],		// 价格、数量
			["3100.0", 100]		// 价格、数量
		],
    "ts": 1604643655040584408  // 时间戳
	}
}
```
此接口，可获取指定合约的买卖盘数据。

买卖盘上的买单和卖单均按照价格汇总，每个价格下仅返回一个根据价格汇总的挂单量。

此接口，只会返回部分的买卖盘数据，level2_20是指返回买卖方各20条数据，level_100 是指返回买卖方各100条数据。推荐您使用这个接口，因为响应速度更快，流量消耗小。
### HTTP请求
GET /api/v1/level2/depth20

GET /api/v1/level2/depth100
### 示例
GET /api/v1/level2/depth100?symbol=XBTUSDM

### 参数
| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称    |

## Level 2消息拉取
```json
  {
    "code": "200000",
    "data": [
      {
          "symbol": "XBTUSDM",				// 合约
          "sequence": 1,					// 消息顺序号
          "change": "7000.0,sell,10"		// 价格、方向、数量
        },
      {
          "symbol": "XBTUSDM",				// 合约
          "sequence": 2,					// 消息顺序号
          "change": "7000.0,sell,0"		// 价格、方向、数量
      }
    ]
  }
```
如果websocket推送的消息不连续，可通过该请求拉取缺失的消息。start为上一次收到websocket推送的sequence+1，end为本次收到的websocket推送的sequence-1。重放拉取的消息，完成后继续消费websocket消息。如果end和start的差值超过500，则不能直接使用该接口，建议重新构建Level 2的买卖盘。

Level 2拉取消息使用方法：以价格为键值，用消息中的数量覆盖本地的数量。当数量为0时，删除该数量在本地记录中对应的价格。

### HTTP请求
GET /api/v1/level2/message/query

### 示例
GET /api/v1/level2/message/query?symbol=XBTUSDM&start=100&end=200

### 参数
| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称    |
| start | long | 开始顺序号（返回的数据会包含该顺序号）   |
| end | long | 结束顺序号（返回的数据会包含该顺序号）   |



# 历史数据

## 成交历史

```json
  {
    "code": "200000",
    "data": {
			"sequence": 102,					              // 序号
			"tradeId": "5cbd7377a6ffab0c7ba98b26",      // 交易号
			"takerOrderId": "5cbd7377a6ffab0c7ba98b27", // Taker方订单ID
			"makerOrderId": "5cbd7377a6ffab0c7ba98b28", // Maker方订单ID
			"price": "7000.0",                          // 成交价格
			"size": 1,                                // 成交数量
			"side": "buy",                              // 成交方向 - taker
      "ts": 1545904567062140823                   // 成交时间 - 纳秒
		}
  }
```
使用该接口可获取指定合约的最近一百条交易记录

### HTTP请求
GET /api/v1/trade/history

### 示例
GET /api/v1/trade/history?symbol=XBTUSDM

### 参数
| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String | 合约名称     |

### 属性含义

**交易方向**

Taker订单的成交方向。Taker订单指立刻与买卖盘上的已有订单成交的订单类型。



# 指数

## 查询利率列表

```json
  {
    "dataList": [
      {
        "symbol": ".XBTINT",                 //利率symbol
        "granularity": 60000,                //粒度(毫秒)
        "timePoint": 1557996300000,          //时间点(毫秒)
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
    "hasMore": true                          //是否还有下一页
  }
```

查询利率列表

### HTTP请求
GET /api/v1/interest/query

### 示例
GET /api/v1/interest/query?symbol=.XBTINT

### 参数

| 参数     | 数据类型    | 含义    |
| --------- | ------- | ---------------------- |
| symbol    | String  | 利率symbol     |
| startAt | long    | [可选] 开始时间（毫秒）                     |
| endAt   | long    | [[可选]  截止时间（毫秒）              |
| reverse   | boolean | [可选]是否逆序查询, **true**或**false**，默认为**true** |
| offset    | long    | [可选] 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页|
| forward   | boolean | [可选] 是否前向查询，true或false，默认为true |
| maxCount  | int     | [可选] 最大记录条数，默认为10  |



## 查询指数列表

```json
{ 
    "dataList": [
      {
        "symbol": ".KXBT",                   //指数symbol
        "granularity": 1000,                 //粒度(毫秒)
        "timePoint": 1557998570000,          //时间点(毫秒)
        "value": 8016.24,                    //指数值
        "decomposionList": [                 //成分列表
          {
            "exchange": "gemini",            //成分交易所
            "price": 8016.24,                //最近成交价
            "weight": 0.08042611             //权重
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
    "hasMore": true                            //是否还有下一页
  }
```

查询指数列表

### HTTP请求
GET /api/v1/index/query

### 参数
| 参数     | 数据类型    | 含义                                                  |
| --------- | ------- | ------------------------ |
| symbol    | String  | 利率symbol        |
| startAt | long    | [可选] 开始时间（毫秒）               |
| endAt   | long    | [可选]  截止时间（毫秒）   |
| reverse   | boolean | [可选] 是否逆序查询，**true** 或 **false**，默认为**true** |
| offset    | long    | [可选] 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页   |
| forward   | boolean | [可选] 是否前向查询，**true** 或 **false**，默认为**true** |
| maxCount  | int     | [可选] 最大记录条数，默认为10      |

<br/>
<br/>
<br/>
<br/>
<br/>

## 查询当前标记价格

```json
  {
    "symbol": "XBTUSDM",                //合约symbol
    "granularity": 1000,               //粒度(毫秒)
    "timePoint": 1557999585000,        //时间点(毫秒)
    "value": 8052.51,                  //标记价格
    "indexPrice": 8041.95              //指数价格
  }
```

查询当前标记价格

### HTTP请求
GET /api/v1/mark-price/{symbol}/current

### 示例
GET /api/v1/mark-price/XBTUSDM/current

### 参数

| 参数  | 数据类型   | 含义 |
| ------ | ------ | ----------- |
| symbol | String |  合约symbol  |



## 查询溢价指数

```json
  {
    "dataList": [
      {
        "symbol": ".XBTUSDMPI",              //溢价指数symbol
        "granularity": 60000,                //粒度(毫秒)
        "timePoint": 1558000320000,          //时间点(毫秒)
        "value": 0.022585                    //溢价指数值
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
    "hasMore": true                        //是否还有下一页
  }
```

查询溢价指数

### HTTP请求
GET /api/v1/premium/query

### 参数

| 参数     | 数据类型    | 含义                                                  |
| --------- | ------- | -------------------------- |
| symbol    | String  | 溢价指数symbol      |
| startAt | long    | [可选] 开始时间（毫秒）                     |
| endAt   | long    | [可选]  截止时间（毫秒）  |
| reverse   | boolean | [可选] 是否逆序查询, **true** 或者 **false**, 默认为**true** |
| offset    | long    | [可选] 起始偏移量，一般使用上个请求最后一条返回结果的唯一属性，默认返回第一页|
| forward   | boolean | [可选] 是否前向查询, **true**或者**false**, 默认为**true** |
| maxCount  | int     | [可选] 最大记录条数, 默认为10                          |



## 查询当前资金费率

```json
  {
    "symbol": ".XBTUSDMFPI8H",              //资金费率symbol 
    "granularity": 28800000,               //粒度(毫秒)
    "timePoint": 1558000800000,            //时间点(毫秒)
    "value": 0.00375,                      //资金费率
    "predictedValue": 0.00375              //预测资金费率
  }
```
查询当前资金费率

### HTTP请求
GET /api/v1/funding-rate/{symbol}/current

### 示例
GET /api/v1/funding-rate/XBTUSDM/current


### 参数

| 参数  | 数据类型   | 含义    |
| ------ | ------ | -------------- |
| symbol | String | 合约名称|

# 时间
## 获取服务器时间

```json
  {  
    "code":"200000",
    "msg":"success",
    "data":1546837113087
  }
```

获取API服务器时间。这是Unix时间戳。

### HTTP请求
GET /api/v1/timestamp


# 服务状态

## 获取当前服务状态


```json
  {    
    "code": "200000",     
    "data": {
        "status": "open",                //open: 正常运行, close: 服务关闭, cancelonly:只能撤单
        "msg":  "upgrade match engine"   //备注
      }
  }
```

获取当前服务状态


### HTTP Request
GET /api/v1/status


# K线

## 获取合约K线数据

### HTTP请求
GET /api/v1/kline/query

### 示例
GET /api/v1/kline/query?symbol=.KXBT&granularity=480&from=1535302400000&to=1559174400000

### 参数
| 参数     | 数据类型    | 含义                                                  |
| --------- | ------- | -------------------------- |
| symbol    | String  | [必选]symbol|
| granularity | int    | [必选]K线粒度（分钟数）|
| from   | long    | [可选]开始时间（毫秒）|
| to   | long | [可选]结束时间（毫秒）|

### 返回值
```json
{
    "code": "200000",
    "data": [
        [
            1575331200000,//时间
            7495.01,      //开盘价
            8309.67,      //最高价
            7250,         //最低价
            7463.55,      //收盘价
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



### 说明
1.granularity（k线粒度参数）代表分钟数，可选范围：1,5,15,30,60,120,240,480,720,1440,10080。granularity不在该范围的请求将被拒绝<br/>

2.单次请求的最大数据量是200。如果您选择的开始/结束时间和时间粒度导致超过单次请求的最大数据量，您的请求将只会返回200个数据。如果您希望在更大的时间范围内获取足够精细的数据，则需要使用多个开始/结束范围进行多次请求。<br/>

3.如果只提供了开始时间，则查询开始时间到系统当前时间最大200条数据。如果只提供了结束时间，则返回离结束时间最近的200条数据。如果开始时间和结束时间均未提供，则查询离系统当前时间最近的200条数据<br/>


# Websocket
REST API的使用受到了访问频率的限制，因此推荐您使用Websocket获取实时数据。

**推荐您创建一条Websocket连接，多频道订阅获取实时数据。**

## 申请连接令牌

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

在创建Websocket连接前，您需申请一个令牌（Token）。



### 公共令牌 (不需要加签登录)

如果您只订阅公共频道的数据，请按照以下方式请求获取服务器列表和临时公共令牌。

#### HTTP请求
POST /api/v1/bullet-public

### 私有频道（需要验证签名）

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

如您需请求私有频道的数据（如：账户资金变化），请在签名验证后按照以下方式获取Websocket的服务实例和已验签的令牌。


#### HTTP 请求
POST /api/v1/bullet-private


### 返回值

|字段 | 含义|
-----|-----
|pingInterval| 发送消息的间隔时间（毫秒）|
|pingTimeout| 如果在pingTimeout时间后，未收到pong消息，那么连接可能已断开了 |
|endpoint| Websocket建立连接的服务器地址 |
|protocol| 支持的协议 |
|encrypt| 表示是否使用了SSL加密 |
|token | 令牌 |

## 建立连接

```javascript
var socket = new WebSocket("wss://push.kucoin.com/endpoint?token=xxx&[connectId=xxxxx]");
```

成功建立连接后，您将会收到系统向您发出的欢迎（welcome）消息。

```json
  {
    "id":"hQvf8jkno",
    "type":"welcome"
  } 
```

**connectId**：连接ID，是客户端生成的唯一标识。您在创建连接时收到的欢迎（welcome）消息的ID以及错误消息的ID都属于连接ID（connectId）。


## Ping
```json
  {
    "id":"1545910590801",
    "type":"ping"
  }
```

为防止服务器断开TCP连接，客户端需要向服务器发送ping消息以保持连接的活跃性。

在服务器收到ping消息后，系统会向客户端返回一条pong消息。

如果服务器在60秒内没有收到来自客户端的ping消息，连接将被断开。


```json
  {
    "id":"1545910590801",
    "type":"pong"
  }
```

## 订阅数据

```json
  {
    "id": 1545910660739,                          //表示ID的唯一值 
    "type": "subscribe",
    "topic": "/market/ticker:XBTUSDM",  // 被订阅的频道。一些频道支持使用“,”分开订阅多个合约的信息推送。
    "privateChannel": false,                      // 是否使用了私有频道，默认设置为“false”。
    "response": true                              // 服务器是否需要返回该频道推送的信息。默认设置为“false”。
  }
```

使用服务器订阅消息时，客户端应向服务器发送订阅消息。

订阅成功后，当“response”参数为“false”时，系统将向您发出“ack”消息。

```json
  {
    "id":"1545910660739",
    "type":"ack"
  }
```

当订阅频道产生新消息时，系统将向客户端推送消息。了解消息格式，请查看频道介绍。


### 参数
#### ID
ID用于标识请求和ack的唯一字符串。

#### Topic
您订阅的频道内容。

#### PrivateChannel

您可通过privateChannel参数订阅以一些用户私有的topic（如：/contractMarket/level2）。该参数默认设置为“false”。设置为“true”时，则您只能收到与您订阅的topic相关的内容推送。

#### Response
若设置为True, 用户成功订阅后，系统将返回ack消息。

## 退订
用于取消您之前订阅的topic

```json
  {
    "id": "1545910840805",                            // 表示ID的唯一值 
    "type": "unsubscribe",
    "topic": "/market/ticker:XBTUSDM",      //被取消订阅的频道。一些频道支持使用“,”分开取消多个交易对的信息订阅。
    "privateChannel": false, 
    "response": true,                                  //服务器是否需要返回该频道推送的信息。默认设置为“false”。


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

### 参数

#### ID
用于标识请求的唯一字符串。

#### Topic
您订阅的topic内容。

#### PrivateChannel
您可通过privateChannel参数订阅以一些公共topic（如：/contractMarket/tradeOrders）。该参数默认设置为“false”。设置为“true”，您只能收到与您订阅相关的内容推送。

#### Response
若设置为True, 用户成功取消订阅后，系统将返回ack消息。

## 多路复用
 在一条物理连接上，您可开启多条多路复用通道，以订阅不同topic，获取多种数据推送。

例如：
请输入以下指令定开启多条bt1通道
 {"id": "1Jpg30DEdU", "type": "openTunnel", "newTunnelId": "bt1", "response": true}

在指定中添加参数**tunnelId**：
{"id": "1JpoPamgFM", "type": "subscribe", "topic": "/market/ticker:XBTUSDM"，"tunnelId": "bt1", "response": true}

请求成功后，您将收到 **tunnelIId** 对应的消息推送：
{"id": "1JpoPamgFM", "type": "message", "topic": "/market/ticker:XBTUSDM", "subject": "trade.ticker", "tunnelId": "bt1", "data": {...}}

关闭**通道**，请输入以下指令：
{"id": "1JpsAHsxKS", "type": "closeTunnel", "tunnelId": "bt1", "response": true}

##### 限制

1. 多路复用仅限API用户使用。
2. 最多可开启的多路复用通道条数：5条。

## 定序编号
买卖盘数据化、成交历史数据以及快照消息均会默认返回sequence字段。您可以从Level 2和Level 3市场行情数据中的sequence来判断数据是否丢失，连接是否稳定。如果连接不稳定，请按照校准流程进行校准。

## 客户端消息判断逻辑

1. 判断消息类型。当前消息类型有三种消息类型：
    message（常用的推送消息）
    notice（通知）
    command（连续的命令）
2. 通过userId判断消息类型。有userId的消息为私有消息，没有userId的消息为一般消息。
3. 通过topic判断消息类型。可通过topic，来判断消息类型。
4. 通过subject判断消息类型。对于同一个topic下不同类型的消息，可通过subject判断消息类型。


# 公共频道

## 交易实时行情 ticker v2

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/tickerV2:XBTUSDM",
    "response": true                              
  }
```

Topic: **/contractMarket/tickerV2:{symbol}**

```json
  {
    "subject": "tickerV2",
    "topic": "/contractMarket/tickerV2:XBTUSDM",
    "data": {
      "symbol": "XBTUSDM",					// 行情
      "bestBidSize": 795,					// 最佳买一价总数量
      "bestBidPrice": 3200.00,			// 最佳买一价
      "bestAskPrice": 3600.00,			// 最佳卖一价
      "bestAskSize": 284,					// 最佳卖一价总数量
      "ts": 1553846081210004941		// 成交时间 - 纳秒
   }
  }
```
订阅此topic，可获取指定交易对的最佳买一和卖一价（BBO）的数据推送。

每当买卖盘有变化时，推送实时ticker。v2版本推送更具有实时性，推荐接入该版本。

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

## 交易实时行情 ticker

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/ticker:XBTUSDM",
    "response": true                              
  }
```

Topic: **/contractMarket/ticker:{symbol}**

```json
  {
    "subject": "ticker",
    "topic": "/contractMarket/ticker:XBTUSDM",
    "data": {
      "symbol": "XBTUSDM",					// 行情
      "sequence": 45,						// 顺序号，用于判断消息连续
      "side": "sell",						// 最新成交的taker方向
      "price": 3600.00,					// 成交价格
      "size": 16,							// 成交数量
      "tradeId": "5c9dcf4170744d6f5a3d32fb",    // 订单号
      "bestBidSize": 795,					// 最佳买一价总数量
      "bestBidPrice": 3200.00,			// 最佳买一价
      "bestAskPrice": 3600.00,			// 最佳卖一价
      "bestAskSize": 284,					// 最佳卖一价总数量
      "ts": 1553846081210004941		// 成交时间 - 纳秒
   }
  }
```
订阅此topic，可获取指定交易对的最佳买一和卖一价（BBO）的数据推送。

每完成一笔撮合，该渠道就会实时推送一次价格。如果有多个订单在同一时间被撮合，仅推送最近一笔完成撮合的订单事件。

该推送已不推荐使用，获取实时的ticker，请订阅 /contractMarket/tickerV2:{symbol}。

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

## Level 2 市场行情

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/level2:XBTUSDM",
    "response": true                              
  }
```

Topic：**/contractMarket/level2:{symbol}**

订阅此topic，获取Level 2买卖盘数据。

订阅成功后，Websocket系统将向您推送增量数据的消息。

```json
  {
    "subject": "level2",
    "topic": "/contractMarket/level2:XBTUSDM",
    "type": "message",
    "data": {
      "sequence": 18,					// 顺序号，用于判断消息连续
      "change": "5000.0,sell,83"		// 价格、方向、数量
      "timestamp": 1551770400000 
      
      }
  }
```

校准流程：

1. 将Websocket推送的Level 2数据缓存在本地。
2. 通过REST请求拉取[Level 2](#获取全部买卖盘-level-2)买卖盘的快照信息。
3. 回放缓存的Level 2数据流。
4. 将拉取的最新Level 2数据流回放到本地缓存中，以确保最新的Level 2买卖盘数据顺序号与之前的Level 2数据顺序号连续无间断。丢弃掉旧Level 2数据该顺序号之前的数据，更新Level 2数据流。
5. 请根据订单数量对应的顺序号更新Level 2的全部买卖盘数据。如果数量为0，则需要将该数量对应的订单价格从Level 2数据流中移除。如遇其他情况，正常更新买卖盘数据即可。
6. 如果收到的消息的sequence与上一条消息不连续，可通过REST请求(GET /api/v1/level2/message/query), start和end间隔不超过500。
[Level 2](#level-2消息拉取) 的Change属性是一个“price, size, sequence”的字符串值。请注意，size指的是price对应的最新size。当size为0时，需要将其对应的price从买卖盘中删除。

**示例**

通过REST请求（Get Order Book）拉取[Level 2](#获取全部买卖盘-level-2)买卖盘的快照信息。获取的快照信息如下：


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

如上所示，当前拉取的买卖盘快照数据如下：

| 价格   | 数量 | 方向 |
| ------- | ---- | ---- |
| 3988.62 | 8    | 卖4 |
| 3988.61 | 32   | 卖3 |
| 3988.60 | 47   | 卖2 |
| 3988.59 | 3    | 卖1 |
| 3988.51 | 56   | 买1 |
| 3988.50 | 15   | 买2  |
| 3988.49 | 100  | 买3  |
| 3988.48 | 10   | 买4  |

订阅成功后，您将收到如下变更消息：

``` json
  "data": {
    "sequence": 17,
    "change": "3988.50,buy,44"     // 价格、方向、数量
  }
```
``` json
  "data": {
    "sequence": 18,
    "change": "3988.61,sell,0"     // 价格、方向、数量
  }
```

当前买卖盘快照信息的顺序号为16。丢弃买卖盘数据中顺序号小于等于16的数据，回放顺序号为17和18的数据，并更新买卖盘快照信息。现在，您的顺序号变成了18，本地买卖盘已最新。

**变更**

1. **将价格3988.50对应的数量变更为44 （顺序号为17）**
2. **移除价格为3988.61的数据（顺序号为8）**


变更后，当前买卖盘数据为最新数据，具体数据如下：

| 价格   | 数量 | 方向 |
| ------- | ---- | ---- |
| 3988.62 | 8    | 卖3 |
| 3988.60 | 47   | 卖2 |
| 3988.59 | 3    | 卖1 |
| 3988.51 | 56   | 买1  |
| 3988.50 | 44   | 买2  |
| 3988.49 | 100  | 买3  |
| 3988.48 | 10  | 买4  |

## 成交记录 

```json
  {
    "id": 1545910660741,                          
    "type": "subscribe",
    "topic": "/contractMarket/execution:XBTUSDM",
    "response": true                              
  }
```
Topic: **/contractMarket/execution:{symbol}**

每撮合一笔订单，系统就会按照如下格式向您推送消息：

```json
 {
   "topic": "/contractMarket/execution:XBTUSDM",
   "subject": "match",
   "data": {
        "symbol": "XBTUSDM",				// 合约
        "sequence": 36,						// 顺序号，用于判断websocket消息连续
        "side": "buy",						//  taker的方向 
        "matchSize": 1,           // 成交数量
        "size": 1,							// 订单剩余数量
        "price": 3200.00,					// 成交价格
        "takerOrderId": "5c9dd00870744d71c43f5e25",  // taker方订单ID
        "time": 1553846281766256031,		             // 成交时间 - 纳秒
        "makerOrderId": "5c9d852070744d0976909a0c",  // maker方订单ID
        "tradeId": "5c9dd00970744d6f5a3d32fc"        // 交易号
    }
 }
```



## level2的5档全量数据推送频道 

Topic: **/contractMarket/level2Depth5:{symbol}**

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
推送频率为最多100ms一次。


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



## level2的50档全量数据推送频道

Topic: **/contractMarket/level2Depth50:{symbol}**

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
推送频率为最多100ms一次。
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




## 产品行情数据
Topic： **/contract/instrument:{symbol}**
订阅此topic，可获取指定合约产品的行情数据。

```json
 //产品行情数据
  { 
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/instrument:XBTUSDM",   
    "response": true                              
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

### 标记价格、指数价格

```json
  //标记价格、指数价格
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "mark.index.price",
    "data": {
        "granularity": 1000,           //粒度
        "indexPrice": 4000.23,            //指数价格
        "markPrice": 4010.52,           //标记价格
        "timestamp": 1551770400000
    }
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

### 资金费率

```json
 //资金费率
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "funding.rate",
    "data": {
        "granularity": 60000,  //粒度(预测资金费率：1分钟粒度60000; 资金费率: 8小时粒度28800000)
        "fundingRate": -0.002966,     //资金费率
        "timestamp": 1551770400000
    }
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

## 系统公告
topic:  **/contract/announcement**
订阅此topic，可获取系统公告的推送。

```json
 //系统公告
  { 
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/announcement",   
    "response": true                              
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

### 资金费用结算开始

```json
 //资金费用结算开始
  { 
    "topic": "/contract/announcement",
    "subject": "funding.begin",
    "data": {
        "symbol": "XBTUSDM",                   //合约symbol
        "fundingTime": 1551770400000,          //费用时间
        "fundingRate": -0.002966,             //资金费率
        "timestamp": 1551770400000
    }
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

### 资金费用结算结束

```json
  //资金费用结算结束
  { 
    "type":"message",
    "topic": "/contract/announcement",
    "subject": "funding.end",
    "data": {
        "symbol": "XBTUSDM",                   //合约symbol
        "fundingTime": 1551770400000,          //费用时间
        "fundingRate": -0.002966,            //资金费率
        "timestamp": 1551770410000          
    }
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

## 交易统计定时触发事件
每 5 秒定时触发交易统计信息推送。

```json
  //交易统计定时触发事件
  { 
    "topic": "/contractMarket/snapshot:XBTUSDM",
    "subject": "snapshot.24h",
    "data": {
        "volume": 30449670,            //24小时成交量
        "turnover": 845169919063,      //24小时成交额
        "lastPrice": 3551,           //最新成交价
        "priceChgPct": 0.0043,         //24小时涨跌幅
        "ts": 1547697294838004923      //快照时间，精确到纳秒
    }  
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



# 私有消息

## 订单私有消息-按照市场独立推送
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders:XBTUSDM",
   "subject": "symbolOrderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //订单号
       "symbol": "XBTUSDM",  //合约symbol
       "type": "match",  //消息类型，取值列表: "open", "match", "filled", "canceled", "update" 
       "status": "open", //订单状态: "match", "open", "done"
       "matchSize": "", //成交数量 (当类型为"match"时包含此字段) 
       "matchPrice": "",//成交价格 (当类型为"match"时包含此字段) 
       "orderType": "limit", //订单类型, "market"表示市价单", "limit"表示限价单 
       "side": "buy",  // 订单方向，买或卖 
       "price": "3600",  //订单价格
       "size": "20000",  //订单数量
       "remainSize": "20001",  //订单剩余可用于交易的数量
       "filledSize":"20000",  //订单已成交的数量
       "canceledSize": "0",  //  update消息中，订单减少的数量
       "tradeId": "5ce24c16b210233c36eexxxx",  //交易号(当类型为"match"时包含此字段) 
       "clientOid": "5ce24c16b210233c36ee321d", //用户自定义ID 
       "orderTime": 1545914149935808589,  // 下单时间 
       "oldSize ": "15000", // 更新前的数量(当类型为"update"时包含此字段) 
       "liquidity": "maker", // 成交方向，取taker一方的买卖方向 
       "ts": 1545914149935808589 // 时间戳
   }
}
```
**订单状态** 
   "match": 订单为taker时与买卖盘中订单成交，此时该taker订单状态为match；
   "open": 订单存在于买卖盘中；  
   "done": 订单完成；

**消息类型**
   "open": 订单进入买卖盘时发出的消息；  
   "match": 订单成交时发出的消息；
   "filled": 订单因成交后状态变为DONE时发出的消息；
   "canceled": 订单因被取消后状态变为DONE时发出的消息；
   "update": 订单因被修改发出的消息；
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

## 订单私有消息
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders",
   "subject": "orderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //订单号
       "symbol": "XBTUSDM",  //合约symbol
       "type": "match",  //消息类型，取值列表: "open", "match", "filled", "canceled", "update" 
       "status": "open", //订单状态: "match", "open", "done"
       "matchSize": "", //成交数量 (当类型为"match"时包含此字段) 
       "matchPrice": "",//成交价格 (当类型为"match"时包含此字段) 
       "orderType": "limit", //订单类型, "market"表示市价单", "limit"表示限价单 
       "side": "buy",  // 订单方向，买或卖 
       "price": "3600",  //订单价格
       "size": "20000",  //订单数量
       "remainSize": "20001",  //订单剩余可用于交易的数量
       "filledSize":"20000",  //订单已成交的数量
       "canceledSize": "0",  //  update消息中，订单减少的数量
       "tradeId": "5ce24c16b210233c36eexxxx",  //交易号(当类型为"match"时包含此字段) 
       "clientOid": "5ce24c16b210233c36ee321d", //用户自定义ID 
       "orderTime": 1545914149935808589,  // 下单时间 
       "oldSize ": "15000", // 更新前的数量(当类型为"update"时包含此字段) 
       "liquidity": "maker", // 成交方向，取taker一方的买卖方向 
       "ts": 1545914149935808589 // 时间戳
   }
}
```
**订单状态** 
   "match": 订单为taker时与买卖盘中订单成交，此时该taker订单状态为match；
   "open": 订单存在于买卖盘中；  
   "done": 订单完成；

**消息类型**
   "open": 订单进入买卖盘时发出的消息；  
   "match": 订单成交时发出的消息；
   "filled": 订单因成交后状态变为DONE时发出的消息；
   "canceled": 订单因被取消后状态变为DONE时发出的消息；
   "update": 订单因被修改发出的消息；
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


## 止损单生命周期监听事件

```json
  {
       "userId": "5cd3f1a7b7ebc19ae9558591", // 不推荐使用, 后续版本将删除
       "topic": "/contractMarket/advancedOrders", 
       "subject": "stopOrder",
       "data": {
           "orderId": "5cdfc138b21023a909e5ad55", //订单编号
           "symbol": "XBTUSDM",  //合约symbol
           "type": "open",  // 消息类型: open (止损下单成功), triggered (止损单触发), cancel (止损单取消)
           "orderType":"stop", // 订单类型: stop
           "side":"buy", // 订单买卖方向
           "size":"1000", //数量 
           "orderPrice":"9000",  //订单价格
           "stop":"up", //止损类型 ("up" 或 "down")
           "stopPrice":"9100", //止损单触发价格
           "stopPriceType":"TP", //止损单触发价格类型
           "triggerSuccess": true, //触发成功标记, 只有triggered类型消息需要
           "error": "error.createOrder.accountBalanceInsufficient", //错误码, 触发失败时使用
           "createdAt": 1558074652423  //创建时间
           "ts":1558074652423004000  //创建时间戳纳秒
       }
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
<br/>
<br/>
<br/>
<br/>

## 账户资金发生变化
### 委托保证金变更事件

```json
  //委托保证金变更事件
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // 不推荐使用, 后续版本将删除
    "topic": "/contractAccount/wallet",
    "subject": "orderMargin.change",
    "data": {
        "orderMargin": 5923,//当前委托保证金
        "currency":"USDT",//币种
        "timestamp": 1553842862614
    }
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

### 可用余额变更事件

```json
   //可用余额变更事件
  {
    "userId": "xbc453tg732eba53a88ggyt8c", // 不推荐使用, 后续版本将删除
    "topic": "/contractAccount/wallet",
    "subject": "availableBalance.change",
    "data": {
      "availableBalance": 5923, //当前可用余额
      "holdBalance": 2312, //冻结金额
      "currency":"USDT",//币种
      "timestamp": 1553842862614
    }
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

### 提现转出冻结变更事件

```json
   //提现转出冻结变更事件
  {
    "userId": "xbc453tg732eba53a88ggyt8c",  // 不推荐使用, 后续版本将删除
    "topic": "/contractAccount/wallet",
    "subject": "withdrawHold.change",
    "data": {
      "withdrawHold": 5923, //当前提现冻结
      "currency":"USDT",//币种
      "timestamp": 1553842862614
    }
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

## 仓位变化
### 仓位操作引起的仓位变化

```json
  //仓位操作引起的仓位变化
  { 
    "userId": "5c32d69203aa676ce4b543c7",  // 不推荐使用, 后续版本将删除
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
      "realisedGrossPnl": 0E-8,                //累加已实现毛利
      "crossMode": false,                      //是否全仓
      "liquidationPrice": 1000000.0,           //强平价格
      "posLoss": 0E-8,                         //手动追加的保证金
      "avgEntryPrice": 7508.22,                //平均开仓价格
      "unrealisedPnl": -0.00014735,            //未实现盈亏
      "markPrice": 7947.83,                    //标记价格
      "posMargin": 0.00266779,                 //仓位保证金
      "riskLimit": 200,                        //风险限额
      "unrealisedCost": 0.00266375,            //未实现价值
      "posComm": 0.00000392,                   //破产费用
      "posMaint": 0.00001724,                  //维持保证金
      "posCost": 0.00266375,                   //仓位价值
      "maintMarginReq": 0.005,                 //维持保证金比例
      "bankruptPrice": 1000000.0,              //破产价格
      "realisedCost": 0.00000271,              //当前累计已实现仓位价值
      "markValue": 0.00251640,                 //标记价值
      "posInit": 0.00266375,                   //杠杆保证金
      "realisedPnl": -0.00000253,              //已实现盈亏
      "maintMargin": 0.00252044,               //仓位保证金
      "realLeverage": 1.06,                    //杠杆倍数
      "currentCost": 0.00266375,               //当前总仓位价值
      "openingTimestamp": 1558433191000,       //开仓时间
      "currentQty": -20,                       //当前仓位
      "delevPercentage": 0.52,                 //ADL分位数
      "currentComm": 0.00000271,               //当前总费用
      "realisedGrossCost": 0E-8,               //累计已实现毛利价值
      "isOpen": true,                          //是否开仓
      "posCross": 1.2E-7,                      //手动追加的保证金
      "currentTimestamp": 1558506060394,       //当前时间戳
      "unrealisedRoePcnt": -0.0553,            //投资回报率
      "unrealisedPnlPcnt": -0.0553,            //仓位盈亏率
      "settleCurrency": "XBT"                  //结算币种
      }
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

### 标记价格变化引起的仓位变化

```json
 //标记价格变化引起的仓位变化
  { 
    "userId": "5cd3f1a7b7ebc19ae9558591",  // 不推荐使用, 后续版本将删除
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
          "markPrice": 7947.83,                   //标记价格
          "markValue": 0.00251640,                 //标记价值
          "maintMargin": 0.00252044,              //仓位保证金
          "realLeverage": 10.06,                   //杠杆倍数
          "unrealisedPnl": -0.00014735,           //未实现盈亏
          "unrealisedRoePcnt": -0.0553,           //投资回报率
          "unrealisedPnlPcnt": -0.0553,            //仓位盈亏率
          "delevPercentage": 0.52,             //ADL分位数
          "currentTimestamp": 1558087175068,       //当前时间戳
          "settleCurrency": "XBT"                  //结算币种
      }
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
<br/>
<br/>


### 资金费用结算

```json
 //资金费用结算
  { 
    "userId": "xbc453tg732eba53a88ggyt8c",  // 不推荐使用, 后续版本将删除
    "topic": "/contract/position:XBTUSDM",
    "subject": "position.settlement",
    "data": {
        "fundingTime": 1551770400000,          //费用时间
        "qty": 100,                            //仓位数
        "markPrice": 3610.85,                 //结算价格，为8时刻标记价格，四舍五入到最近合法价格
        "fundingRate": -0.002966,             //结算资金费率
        "fundingFee": -296,                   //资金费用
        "ts": 1547697294838004923,             //当前时间(纳秒)
        "settleCurrency": "XBT"                //结算币种
    }
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

# 登录 KuCoin
<a href="https://www.kucoin.com">登录 KuCoin</a>

# General
## Introduction
Thank you for using KuCoin Futures API documentation. This documentation provides a detailed explanation to the transaction functions and the usage of the interfaces to get the market data on Kucoin Futures.

The whole documentation is divided into two parts: 1)**REST API** and 2) **Websocket Feed**. 

 -  The REST API part contains three sections: **User (private) , Trade (private)** and **Market Data (public).**
 -  The Websocket Feed part contains two sections: **Public Channels** and **Private Channels**. 


## Upcoming Changes

**To reinforce the security of the API, KuCoin upgraded the API key to version 2.0, the validation logic has also been changed. It is recommended to [create](https://futures.kucoin.com/api) and update your API key to version 2.0. The API key of version 1.0 will be still valid until May 1, 2021. [Check new signing method](#signing-a-message)**

#### 2021.11.04
* Added an interface for querying contract risk-limit: [GET /api/v1/contracts/risk-limit/{symbol}](#).
* Added an interface for modifying the risk-limit level: [POST /api/v1/position/risk-limit-level/change](#).
* Added Websocket push for positions subject of [position.adjustRiskLimit](#).

#### 2021.08.18
* Remove the BizNo parameter in interface [POST /api/v2/transfer-out](#transfer-funds-to-kucoin-main-account-2).
* Modify the field marginBalance comment in interface [GET /api/v1/account-overview](#get-account-overview).

#### 2021.07.15
* Modify the strategy of [Request Rate Limit](#request-rate-limit).

#### 2021.03.18
* Added field holdBalance to subject:availableBalance.change in the topic of account balance /contractAccount/wallet <br/>

#### 2021.03.02
* The original level-3 interface /contractMarket/level3:{​​​​​​​symbol} is abandoned, please shift to /contractMarket/level3V2:{​​​​​​​​​​​​​​symbol}​​​​​​​​​​​​​​. <br/>
* Added topic /contractMarket/tickerV2:{symbol} for requesting the real-time ticker. <br/>
* Added topic in the private channel of websocket for notifications of futures orders: /contractMarket/tradeOrders:{​​​​​​​​​​​​​​symbol}. <br/>

#### 2021.02.25
* Request frequency of Level-3 order book via GET /api/v2/level3/snapshot is restricted to: 1 request/minute for each IP. <br/>

#### 2021.02.07
* Level-3 interface updates:
/contractMarket/level3:{symbol} will no longer support the contracts released after February 7, 2021 (UTC), please upgrade the interface to /contractMarket/level3v2:{symbol}. <br/>


#### 2021.01.14
* Modified API permission. The transfer permission of withdrawal has been shifted to trade permission, which influences: <br/>
    POST /api/v2/transfer-out  <br/>


#### 2020.12.23
* New field lowPrice (24H Low), highPrice (24H High), priceChgPct (24H Change%) and priceChg (24H Change) will be added to the response from the following interfaces: <br/>
    GET /api/v1/contracts/active  <br/>
    GET /api/v1/contracts/{symbol}  <br/>

#### 2020.12.17
* To reduce the delays in order placing, the system will no longer verify the uniqueness of the clientOId. Orders placed via API with the same clientOId are now working as well.

#### 2020.08.24
* add 20 or 100 depth API for level2

#### June 19, 2020
*  Add channelType field: public(public channel, default), private(private channel), session(session channel) for Websocket.
*  Deprecate ({topic}:privateChannel:{userId}) and userId in private messages after three months.
*  Added following properties in contract info: "volume of 24 hours", "turnover of 24 hours" and "open interest"
    GET /api/v1/contracts/active
    GET /api/v1/contracts/{symbol}

#### June 12, 2020
* The Level3 message format is completely revised, more comprehensive message fields will be provided.
    To receive messages from new Level 3, please subscribe: "/contractMarket/level3v2:{symbol}"
* Added interface for new Level 3 full data 
   GET /api/v2/level3/snapshot
*  Added private message channel: /contractMarket/tradeOrders
*  Added message channel for the 5 best ask/bid full data of Level 2: /contractMarket/level2Depth5:{symbol}
*  Added message channel for the 50 best ask/bid full data of Level 2: /contractMarket/level2Depth50:{symbol}

#### June 03, 2020
* Brand upgrade and change domain name to KuCoin Futures

#### June 01, 2020
* Added an interface to get service status：
    GET /api/v1/status
#### May 13, 2020
* Added an interface to get K line data:
    GET /api/v1/kline/query

#### April 30, 2020
* Update the default value of parameter chain from OMNI to ERC20, for the following interfaces:<br/>
    POST /api/v1/withdrawals

#### April 28, 2020
* Add support for query order by client order id, for the following interfaces:<br/>
    GET /api/v1/orders/{order-id}

#### April 15，2020
* New field "memo" (address ID) is added to the response from GET /api/v1/withdrawal-list
#### March 30, 2020 
The USDT-Margined Contracts is scheduled to be launched on March 30, 2020 on KuCoin Futures and the supported types of crypto will be expanded from the original one (XBT) to two (XBT and USDT). The detailed modifications for API document is as follows:

* New fields including a) settleCurrency (currency used to clear and settle the trades), b) status (order status, include “open” and “done” status), c) updatedAt (the last update time of an order), and d) orderTime (the time placing the order in nanosecond) will be added to the response from the following interfaces:

       GET /api/v1/orders<br/>
       GET /api/v1/orders/{order-id}<br/>
       GET /api/v1/stopOrders<br/>
       GET /api/v1/recentDoneOrders<br/>

* New fields including a) settleCurrency (currency used to clear and settle the trades), and b) tradeTime (execution time in nanosecond) will be added to the response from the following interfaces:

       GET /api/v1/fills<br/>
       GET /api/v1/recentFills<br/>

* New field settleCurrency (currency used to clear and settle the trades) will be added to the response from<br/> GET /api/v1/openOrderStatistics.

* New field settleCurrency (currency used to clear and settle the trades) will be added to the response from <br/>GET /api/v1/funding-history

* New field maxLeverage (maximum contract leverage) will be added to the response from the following interfaces:
     GET /api/v1/contracts/active <br/>
     GET /api/v1/contracts/{symbol}

* New private channel (topic: /contractMarket/advancedOrders, subject: stopOrder) is added for stop orders.

```json
    {
       "userId": "5cd3f1a7b7ebc19ae9558591", // Deprecated 
       "topic": "/contractMarket/advancedOrders", 
       "subject": "stopOrder",
       "data": {
           "orderId": "5cdfc138b21023a909e5ad55", //Order ID
           "symbol": "XBTUSDM",  //Ticker symbol of the contract
           "type": "open",  // Message type: open (place order), triggered (order triggered), cancel (cancel order)
           "orderType":"stop", // Order type: stop order
           "side":"buy", // Buy or sell
           "size":"1000", //Quantity 
           "orderPrice":"9000",  //Order price. For market orders, the value is null
           "stop":"up", //Stop order types (stop limit or stop market)
           "stopPrice":"9100", //Trigger price of stop orders
           "stopPriceType":"TP", //Trigger price type of stop orders
           "triggerSuccess": true, //Mark to show whether the order is triggered. Only for triggered type of orders 
           "error": "error.createOrder.accountBalanceInsufficient", //HTTP error code, which is used only when the trigger of the triggered type of orders fails
           "createdAt": 1558074652423  //Time the order created
           "ts":1558074652423004000  //Filled time - nanosecond
       }
   }
```


* New field settleCurrency (currency used to clear and settle the trades) will be added to the  response from the following interfaces:  

      GET /api/v1/position  
      GET /api/v1/positions  
  
* New field settleCurrency (currency used to clear and settle the trades) will be added to the subject:<br/>
    position.change position.settlement of topic "/contract/position:{symbol}".

*  New field currency (currency) will be added to the query parameters to filter the profit and loss records; New field currency (currency) will be added to the response from the: <br/>
   GET /api/v1/transaction-history   

*  <font color="#dd0000">New parameters including a) memo (for coins without memo, no need to fill the memo field), and b) chain [optional] (for coins available to transfer via multi-chain network, you can specify the protocol in the field. For example, the USDT is able to be deposited or withdrawn via the OMIN, ERC20, and TRC20 protocols) will be added to the response from the interface.<br/>
     POST /api/v1/withdrawals </font>

* New fields currency (currency) will be added to the response from the following interfaces:   

       GET /api/v1/account-overview   
       GET /api/v1/deposit-list    
       GET /api/v1/withdrawal-list   
       GET /api/v1/transfer-list     

* New field currency (currency) will be added to<br/> GET /api/v1/account-overview 

* New interface: <br/>POST /api/v2/transfer-out will be added. 
  
    The new interface is added a currency (currency) parameter to specify the transfer-out currency (XBT/USDT). The original interface POST /api/v1/transfer-out is still available but needs to be upgraded to support the transfer of USDT.


* New field currency (currency) will be added to the subject of topic “/contractAccount/wallet" :
  availableBalance.change  <br/>
  withdrawHold.change   <br/>
  orderMargin.change <br/>
  
* Deprecate level3 partial message query interface. Level3 snapshot query interface is recommended.
  
        GET /api/v1/level3/message/query

<br/>
<br/>


#### 2020.03.05  
Correct the denotation of fields accountEquity and marginBalance. Now accountEquity= unrealisedPNL + marginBalance;


<br/>

## Client Libraries
Client libraries can help you integrate with our API quickly.

**Official Software Development Kit (SDK) of KuCoin Futures**

- [PHP SDK](https://github.com/Kucoin/KuMEX-PHP-SDK)
- [Java SDK](https://github.com/Kucoin/kucoin-futures-java-sdk)

  


##Sandbox
Sandbox is the test environment, used for testing an API connection or web trading. It provides all the functionalities of the live exchange. All funds and transactions there are simulated for testing purposes.

The login session and the API key in the sandbox environment are completely separated from the production environment. You may use the web interface in the sandbox environment to create an API key.

**Notice:** After registering in the sandbox environment, you will receive a nummber amount of fake funds (XBT) automatically released by the system in your account. 

**Sandbox URLs for API test:**

* Website: https://sandbox-futures.kucoin.com 
* REST API: **https://api-sandbox-futures.kucoin.com**  (https://sandbox-api.kumex.com has been Deprecated)

## Request Rate Limit
When a rate limit is exceeded, a status of **429** will be returned.
<aside class="notice">Once the rate limit is exceeded, the system will restrict your use of your IP or account for 10s.</aside>

### REST API
The limit strategy of private endpoints will restrict account by userid. The limit strategy of public endpoints will restrict IP.
<aside class="notice">Note that when an API has a specific rate limit, please refer to the specific limit.</aside>

### WEBSOCKET
**Number of Connections**

* Number of connections per user ID:  **≤ 50**

**Connection Times**

* Connection Limit: **30 per minute**

**Number of Uplink Messages** 

* Message limit sent to server: **100 per 10 seconds**

**Topic Subscription Limit** 

* Subscription limit for each connection: **100 topics**

## Incentive Plan for Market Makers
KuCoin Futures now offers an incentive plan for professional market makers! Join the plan and you can get the following bonus:

 - Commissions
 - Huge rewards for top 1 market maker and extra bonuses for top 10 market makers every month
 - Direct access to the market (via private link provided by KuCoin Futures)

Users with great market making strategies and large trading volume are welcome to join the incentive plan for the long term. If your trading volume has exceeded 5,000 BTC in the last 30 days, please provide the following information to **mm@kucoin.com**, with the title of “Futures Market Maker Application”:

 - Your KuCoin account (email is required, no need to indicate the referral relationship). 
 - Proof of the trading volume in the last 30 days or VIP level on any exchanges. 
 - Brief introduction of your market making strategies and an estimation of the order ratio to the total.

## Priority Lane for VIP
Users with great market making strategies and large trading volume are welcome to join the incentive plan for the long term. If your account balance is greater than 10 BTC, please provide the following information to **vip_futures@kucoin.com** to apply for the market maker position.

 - Your KuCoin account (email is required, no need to indicate the referral relationship).
 - Screenshots of the trading volume of your market making on other exchanges (e.g.: trading volume in 30 days, VIP level, etc.).
 - Brief introduction of your market making strategies.

---
# REST API
## Request
### API Server Address
The REST API provides endpoints for users and trades as well as market data.

A Request URL is made by a Base URL and a specified endpoint of the interface.
Base URL: **https://api-futures.kucoin.com** (https://api.kumex.com has been Deprecated)


### Endpoint of the Interface

Each interface has its own endpoint, which is provided under the **HTTP REQUEST** module. 

For **GET requests**, please append the queried parameters to the endpoint.

E.G. For "[Position](#get-position-details)", the default endpoint of this API is **/api/v1/position**. If you pass the "symbol" parameter (XBTUSDM), the endpoint will become **/api/v1/position?symbol=XBTUSDM** and the final request URL will be **https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM**.

### Requests
All requests and responses are **application/json** content type.  

Unless otherwise stated, all timestamp parameters should be in Unix time milliseconds. e.g. **1544657947759**

### Parameters

For **GET** and **DELETE** requests, all queried parameters need to be included in the request URL. (**e.g. /api/v1/position?symbol=XBTUSDM**)

For **POST** and **PUT** requests, all queried parameters need to be included in the request body in JSON format. (**e.g. {"side":"buy"}**). **Do NOT include any space in JSON strings.**

### Errors
When errors occur, the HTTP error code or system error code will be returned. The body will also contain a message parameter indicating the cause.

#### HTTP Error Code

```json
  {
    "code": "400100",
    "msg": "Invalid Parameter."
  }
```

Code | Meaning
---------- | -------
400 | Bad Request -- Invalid request format
401 | Unauthorized -- Invalid API Key
403 | Forbidden -- The request is forbidden
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access the resource with an invalid method.
415 | Content-Type -- application/json
429 | Too Many Requests -- Access limit breached
500 | Internal Server Error -- We had a problem with our server. Please try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.


#### System Error Code

Code | Meaning
---------- | -------
400001 | Any of KC-API-KEY, KC-API-SIGN, KC-API-TIMESTAMP, KC-API-PASSPHRASE is missing in your request header.
400002 | KC-API-TIMESTAMP Invalid -- Time differs from server time by more than 5 seconds
400003 | KC-API-KEY not exists
400004 | KC-API-PASSPHRASE error
400005 | Signature error -- Please check your signature
400006 | The IP address is not in the API whitelist
400007 | Access Denied -- Your API key does not have sufficient permissions to access the URI
404000 | URL Not Found -- The requested resource could not be found
400100 | Parameter Error -- You tried to access the resource with invalid parameters
411100 | User is frozen -- Please contact us via support center
500000 | Internal Server Error -- We had a problem with our server. Try again later.

If the returned HTTP status code is not 200, the error code will be included in the returned results. If the interface call is successful, the system will return the code and data fields. If not, the system will return the code and msg fields. You can check the error code for details.  

### Success
A successful response is indicated by an **HTTP status code 200** and **system code 200000**. The success response is as follows:  

```json
  {
    "code": "200000"
  }
```

### Pager

KuCoin Futures uses **Pagination** or **HasMore** for all REST requests which return arrays. 

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

**Pagination** allows for fetching results with the current page and is well suited for real time data. Endpoints like /api/v1/deposit-list, /api/v1/orders, /api/v1/fills, return the latest items by default. To retrieve more results, users should specify the currentPage number in the subsequent requests to turn the page based on the data previously returned.

**Example**

GET /api/v1/orders?currentPage=1&pageSize=50

**Parameters**

|Parameter | Default | Description|
|---------- | ------- | ------|
|currentPage | 1 | Current request page.|
|pageSize | 50 | Number of results per request.|



#### HasMore
The **HasMore** pager uses sliding window scheme to obtain paged data by sliding a fixed-sized window on data stream. The returned results will provide field **HasMore** to show if there are more data. The **HasMore** pager is efficient and takes the same amount of time for each sliding which makes **HasMore** pager well suited for the real-time streaming data queries.  


**Parameters**

| Parameter | type    | Description                                |
| --------- | ------- | ------------------------------------------ |
| offset    | -       | Start offset. The unique attribute of the last returned result of the last request. The data of the first page will be returned by default.|
| forward   | boolean | Slide direction. Set to “TRUE” to look up data of the next page |
| maxCount  | int     | The maximum amount for each sliding               |

**Example**

GET /api/v1/interest/query?symbol=.XBTINT&offset=1558079160000&forward=true&maxCount=10

## Types
### Timestamps 
Unless otherwise specified, all timestamps from API are returned in Unix time **milliseconds**(e.g. **1546658861000**).

## Authentication

### Create API KEY
Before being able to sign any requests, you must create an API key via the KuCoin Futures website. Upon creating a key you need to write down 3 pieces of information:

* Key
* Secret
* Passphrase

The **Key** and **Secret** are generated and provided by KuCoin Futures and the **Passphrase** refers to the one you used to create the KuCoin Futures API. Please note that these three pieces of information can not be recovered once lost. If you lost this information, please create a new API KEY. 

### Permissions

You can manage the API permission on [KuCoin Futures](https://futures.kucoin.com)’s official website. The permissions are:

* **General** - Allows a key general permissions. This includes most of the GET endpoints.
* **Trade** -  Allows a key to create/cancel orders and manage positions. 
* **Transfer** - Allows a key to withdraw funds. Enable with caution - API key transfers WILL BYPASS two-factor authentication.


### Create a Request

All REST requests must contain the following headers:

* **KC-API-KEY**   The API key is a string.
* **KC-API-SIGN**   The signature (see Signing a Message).
* **KC-API-TIMESTAMP**   A timestamp for your request.
* **KC-API-PASSPHRASE**   The passphrase you specified when creating the API key.
* **KC-API-KEY-VERSION** You can check the version of API key on the page of [API Management](https://futures.kucoin.com/api)

### Signing a Message

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
        "KC-API-PASSPHRASE": passphrase,
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

**For the header of KC-API-KEY**

1. Use **API-Secret** to encrypt the prehash string **{timestamp+method+endpoint+body}** with sha256 HMAC. The request **body** is a JSON string and need to be the same with the parameters passed by the API.
2. After that, use base64-encode to encrypt the result in step 1 again.

For the **KC-API-PASSPHRASE** of the header:

* For API key-V1.0, please pass requests in plaintext.
* For API key-V2.0, please Specify **KC-API-KEY-VERSION** as **2** --> Encrypt passphrase with HMAC-sha256 via API-Secret --> Encode contents by base64 before you pass the request."

**Notice:**

* The encrypted timestamp shall be consistent with the KC-API-TIMESTAMP field in the request header. 
* The body to be encrypted shall be consistent with the content of the Request Body.  
* The Method should be **UPPER CASE.**
* For GET, DELETE request, the endpoint needs to contain the query string. e.g. **/api/v1/deposit-address?currency=XBT**. The body is " " if there is no request body (typically for GET requests).


```python
#Example to Create a Deposit Address

curl -H "Content-Type:application/json" -H "KC-API-KEY:5c2db93503aa674c74a31734" -H "KC-API-TIMESTAMP:1547015186532" -H "KC-API-PASSPHRASE:QWIxMjM0NTY3OCkoKiZeJSQjQA" -H "KC-API-SIGN:7QP/oM0ykidMdrfNEUmng8eZjg/ZvPafjIqmxiVfYu4=" -H "KC-API-KEY-VERSION:2" 
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
<br/>
<br/>
<br/>
<br/>

----------

### Selecting Timestamp

The **KC-API-TIMESTAMP** header MUST be number of milliseconds since Unix Epoch in UTC.  e.g. 1547015186532

The difference between your timestamp and the API service time must be less than **5 seconds** , or your request will be considered expired and rejected. We recommend using the time endpoint to query for the API server time if you believe there may be time skew between your server and the API server.

---
# User
Signature is required for this part.

# Account
## Get Account Overview
```json
  { 
    "code": "200000",
    "data": {
      "accountEquity": 99.8999305281, //Account equity = marginBalance + Unrealised PNL 
      "unrealisedPNL": 0, //Unrealised profit and loss
      "marginBalance": 99.8999305281, //Margin balance = positionMargin + orderMargin + frozenFunds + availableBalance - unrealisedPNL
      "positionMargin": 0, //Position margin
      "orderMargin": 0, //Order margin
      "frozenFunds": 0, //Frozen funds for withdrawal and out-transfer
      "availableBalance": 99.8999305281 //Available balance
      "currency": "XBT" //currency code
    }
  }
```
### HTTP Request
GET /api/v1/account-overview

### Example
GET /api/v1/account-overview?currency=XBT
### Parameters
Param | Type | Description
--------- | ------- | -----------
currency | String | [Optional] Currecny ,including **XBT,USDT**,Default XBT

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **30 times/3s**.

## Get Transaction History

```json
  {
    "code": "200000",
    "data": {
      "hasMore": false,//Whether there are more pages
      "dataList": [{
        "time": 1558596284040, //Event time
        "type": "RealisedPNL", //Type
        "amount": 0, //Transaction amount
        "fee": null,//Fees
        "accountEquity": 8060.7899305281, //Account equity
        "status": "Pending", //Status. If you have held a position in the current 8-hour settlement period.
        "remark": "XBTUSDM",//Ticker symbol of the contract
        "offset": -1, //Offset
        "currency": "XBT"  //Currency
      },
      {
        "time": 1557997200000,
        "type": "RealisedPNL",
        "amount": -0.000017105,
        "fee": 0,
        "accountEquity": 8060.7899305281,
        "status": "Completed",//Status. Status. Funding period that has been settled.
        "remark": "XBTUSDM",
        "offset": 1,
        "currency": "XBT"
     }]
    }
  }

```
If there are open positions, the status of the first page returned will be **Pending**, indicating the realised profit and loss in the current 8-hour settlement period. Please specify the minimum **offset** number of the current page into the offset field to turn the page.


### HTTP Request
GET /api/v1/transaction-history

### Example
GET /api/v1/transaction-history?offset=1&forward=true&maxCount=50

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

### Parameters
Param | Type | Description
--------- | ------- | -----------
startAt| long | *[Optional]*  Start time (milisecond)
endAt| long | *[Optional]* End time (milisecond)
type | String | *[Optional]* Type **RealisedPNL-Realised profit and loss, Deposit-Deposit, Withdrawal-withdraw, Transferin-Transfer in, TransferOut-Transfer out**
offset | long | *[Optional]* Start offset
maxCount | long | *[Optional]* Displayed size per page. The default size is 50
currency | String | *[Optional]* Currency of transaction history **XBT or USDT**
forward | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default


### Returned Data Types
Field | Description |
--------- | ------- | 
type | RealisedPNL, Deposit, Withdrawal, TransferIn, TransferOut
status | Completed, Pending

# Deposit
## Get Deposit Address

```json
  {
    "code": "200000",
    "data": {
      "address": "0x78d3ad1c0aa1bf068e19c94a2d7b16c9c0fcd8b1",//Deposit address
      "memo": null//Address tag. If the returned value is null, it means that the requested token has no memo. If you are to transfer funds from another platform to KuCoin Futures and if the token to be transferred has memo(tag), you need to fill in the memo to ensure the transferred funds will be sent to the address you specified. 
   } 
}
```

### HTTP Request
GET /api/v1/deposit-address

### Example
GET /api/v1/deposit-address?currency=XBT

### API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
currency | String | Currency,including **XBT,USDT**


## Get Deposits List
```json
  {
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 1,
      "totalPage": 1,
      "items": [{
        "currency": "XBT",//Currency
        "status": "SUCCESS",//Status type: PROCESSING, WALLET_PROCESSING, SUCCESS, FAILURE
        "address": "5CD018972914B66104BF8842",//Deposit address
        "isInner": false,//Inner transfer or not
        "amount": 1,//Deposit amount
        "fee": 0,//Fees for deposit
        "walletTxId": "5CD018972914B66104BF8842",//Wallet TXID
        "createdAt": 1557141673000 //Funds deposit time
      }]
    }
  }
```

The system will return the data of the first page by default.

### HTTP Request
GET /api/v1/deposit-list

### Example
GET /api/v1/deposit-list?currentPage=1&pageSize=50&status=PROCESSING

### API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
startAt | long | *[Optional]* Start time (milisecond)
endAt | long | *[Optional]* End time (milisecond)
status | String | *[Optional]* Status, including **PROCESSING, SUCCESS, and FAILURE**
currency | String | *[Optional]* Currency, including **XBT,USDT**

# Withdrawal
##  Get Withdrawal Limit

```json
 {
    "code": "200000",
    "data": {
      "currency": "XBT",//Currency
      "limitAmount": 2,//24h withdrawal limit
      "usedAmount": 0,//Withdrawal amount over the past 24h.
      "remainAmount": 2,//24h available withdrawal amount 
      "availableAmount": 99.89993052,//Available balance 
      "withdrawMinFee": 0.0005,//Withdrawal fee charges
      "innerWithdrawMinFee": 0,//Inner withdrawal fee charges
      "withdrawMinSize": 0.002,//Min. withdrawal amount
      "isWithdrawEnabled": true,//Available to withdrawal or not
      "precision": 8//Precision of the withdrawal amount
    }
 }
```

### HTTP Request
GET /api/v1/withdrawals/quotas

### Example
GET /api/v1/withdrawals/quotas?currency=XBT

### API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
currency | String | Currency,  including **XBT,USDT**

## Withdraw Funds

```json
  {
   "code": "200000",
    "data": {
    "withdrawalId": "5bffb63303aa675e8bbe18f9" // Withdrawal ID. This ID can be used to cancel the withdrawal
    }
  }
```

### HTTP Request
POST /api/v1/withdrawals

### Example
POST /api/v1/withdrawals

### API Permission
This endpoint requires the **Withdrawal** permission.

### Parameters
Param | Type | Description
--------- | ------- | -----------
currency  | String | Currency,including **XBT,USDT**
address   | String | Withdrawal address
amount | Number | Withdrawal amount
isInner|	boolean| *[Optional]* Inner withdrawal or not. If the withdrawal address is an internal address, you can set to **False** to transfer via blockchain or set to **True** to withdraw through the inner transfer.
remark | String | *[Optional]* Remarks
chain |	String | *[Optional]* The chain name of currency, e.g. The available value for USDT are OMNI, ERC20, TRC20, default is ERC20. This only apply for multi-chain currency, and there is no need for single chain currency.
memo | String | *[Optional]*  Address remark. If there’s no remark, it is empty. When you withdraw from other platforms to the KuCoin Futures, you need to fill in memo(tag). If you do not fill memo (tag), your deposit may not be available, please be cautious.


## Get Withdrawal List
```json
  {
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 10,
      "totalPage": 1,
      "items": [{
        "withdrawalId": "5cda659603aa67131f305f7e",//Withdrawal ID. This ID can be used to cancel the withdrawal
        "currency": "XBT",//Currency
        "status": "FAILURE",//Status
        "address": "3JaG3ReoZCtLcqszxMEvktBn7xZdU9gaoJ",//Withdrawal address
        "memo": "",// Address remark.
        "isInner": true,//Inner withdrawal or not
        "amount": 1,//Withdrawal amount
        "fee": 0,//Withdrawal fee charges
        "walletTxId": "",//Wallet TXID
        "createdAt": 1557816726000,//Withdrawal time
        "remark": "测试.",//Withdrawal remarks
        "reason": "Assets freezing failed."// Reason causing the failure
      }]
    }
  }
```

### HTTP Request
GET /api/v1/withdrawal-list

### Example
GET /api/v1/withdrawal-list?currentPage=1&pageSize=50&status=PROCESSING

### API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
status | String | *[optional]*  **PROCESSING, WALLET_PROCESSING, SUCCESS, FAILURE**
startAt | long | *[optional]*  Start time (milisecond)
endAt | long | *[optional]*  End time (milisecond)
currency | String | *[optional]* Currency, including **XBT,USDT**

## Cancel Withdrawal
Only withdrawal requests of **PROCESSING** status could be canceled.

### HTTP Request
DELETE /api/v1/withdrawals/{withdrawalId}

### HTTP Request
DELETE /api/v1/withdrawals/5cda659603aa67131f305f7e

### API Permission
This endpoint requires the **Withdrawal** permission

### Parameters
Param | Type  | Description
--------- | ------- | -----------
withdrawalId | String | **Path Parameter**. Withdrawal ID, which is returned after requesting to withdraw funds. 

# Transfer
## Transfer Funds to KuCoin-Main Account
```json
  { 
    "code": "200000",
    "data": {
      "applyId": "5bffb63303aa675e8bbe18f9" //Transfer-out request ID
    }  
  }
```
The amount to be transferred will be deducted from the **KuCoin Futures Account**. Please ensure that you have sufficient funds in your KuCoin Futures Account, or the transfer will fail. 
Once the transfer arrives your KuCoin-Main Account, the endpoint will respond and return the **applyId**. This ID could be used to cancel the transfer request. 


### HTTP Request
POST /api/v1/transfer-out [It's deprecated, please use POST /api/v2/transfer-out instead]
### Example
POST /api/v1/transfer-out
### API Permission
This endpoint requires the **Withdrawal** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
bizNo | String | A unique ID generated by the user, to ensure the operation is processed by the system only once. You are suggested to use UUID
amount | Number | Amount to be transfered out


## Get Transfer-Out Request Records

```json
  { 
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 6,
      "totalPage": 1,
      "items": [{
        "applyId": "5cd53be30c19fc3754b60928", //Transfer-out request ID
        "currency": "XBT", //Currency
        "status": "SUCCESS", //Status  PROCESSING, SUCCESS, FAILURE
        "amount": "0.01", //Transaction amount 
        "reason": "", //Reason caused the failure
        "offset": 31986850860000, //Offset
        "createdAt": 1557769977000 //Request application time
      }]
    }   
  }
```
The data of the first page will be queried by default.
## Transfer Funds to KuCoin-Main Account
```json
  { 
    "code": "200000",
    "data": {
      "applyId": "5bffb63303aa675e8bbe18f9" //Transfer-out request ID
    }  
  }
```
The amount to be transferred will be deducted from the **KuCoin Futures Account**. Please ensure that you have sufficient funds in your KuCoin Futures Account, or the transfer will fail. 
Once the transfer arrives your KuCoin-Main Account, the endpoint will respond and return the **applyId**. This ID could be used to cancel the transfer request. 


### HTTP Request
POST /api/v2/transfer-out
### Example
POST /api/v2/transfer-out
### API Permission
This endpoint requires the **"Trade"** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
amount | Number | Amount to be transfered out
currency | String | Currency, including **XBT,USDT**

## Get Transfer-Out Request Records

```json
  { 
    "code": "200000",
    "data": {
      "currentPage": 1,
      "pageSize": 50,
      "totalNum": 6,
      "totalPage": 1,
      "items": [{
        "applyId": "5cd53be30c19fc3754b60928", //Transfer-out request ID
        "currency": "XBT", //Currency
        "status": "SUCCESS", //Status  PROCESSING, SUCCESS, FAILURE
        "amount": "0.01", //Transaction amount 
        "reason": "", //Reason caused the failure
        "offset": 31986850860000, //Offset
        "createdAt": 1557769977000 //Request application time
      }]
    }   
  }
```
The data of the first page will be queried by default.

### HTTP Request
GET /api/v1/transfer-list

### Example
GET /api/v1/transfer-list?currentPage=1&pageSize=50&status=PROCESSING

###API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
startAt | long | *[Optional]*  Start time (milisecond)
endAt | long | *[Optional]* End time (milisecond)
status | String | *[Optional]* Status **PROCESSING, SUCCESS, FAILURE**
currency | String | Currency, including **XBT,USDT**

## Cancel Transfer-Out Request

```json
 { 
    "code": "200000",
    "data":null
  }
```
The transfer-out request could only be canceled under the “PROCESSING” status.

### HTTP Request
DELETE /api/v1/cancel/transfer-out

### Example
DELETE /api/v1/cancel/transfer-out?applyId=5cd53be30c19fc3754b60928

### API Permission
This endpoint requires the **General** permission.

### Parameters
Param | Type  | Description
--------- | ------- | -----------
applyId | String | Transfer ID (Initiate to cancel the transfer-out request)

----
# Trade
Signature is required for this part.

# Orders
## Place an Order

```json
  {
    "code": "200000",
    "data": {
      "orderId": "5bd6e9286d99522a52e458de"
      }
  }
```

You can place two types of orders: **limit** and **market**. Orders can only be placed if your account has sufficient funds. Once an order is placed, your funds will be put on hold for the duration of the order. The amount of funds on hold depends on the order type and parameters specified.


Please be noted that the system would hold the fees from the orders entered the orderbook in advance. Read [Get Fills](#get-fills) to learn more.

**Do NOT include extra spaces in JSON strings**.

### Place Order Limit:

The maximum limit orders for a single contract is **100** per account, and the maximum stop orders for a single contract is **50** per account.


### HTTP Request
POST /api/v1/orders

### API Permission
This endpoint requires the **Trade** permission

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **30 times/3s**.

### Parameters

| Param     | type   | Description  |
| --------- | ------ | -----|
| clientOid | String | Unique order id created by users to identify their orders,  e.g. UUID, Only allows numbers, characters, underline(_), and separator(-) |
| side      | String | **buy** or **sell**                      |
| symbol    | String | a valid contract code. e.g. XBTUSDM                    |
| type      | String | *[optional]*  Either **limit** or **market**
| leverage  | String | Leverage of the order
| remark    | String | *[optional]* remark for the order, length cannot exceed 100 utf8 characters |
| stop      | String | *[optional]* Either **down** or **up**. Requires **stopPrice** and **stopPriceType** to be defined |
| stopPriceType  | String | *[optional]* Either **TP**, **IP** or **MP**, Need to be defined if **stop** is specified.  |
| stopPrice | String | *[optional]* Need to be defined if **stop** is specified. |
| reduceOnly | boolean | *[optional]*  A mark to reduce the position size only. Set to false by default. Need to set the position size when reduceOnly is true. |
| closeOrder | boolean | *[optional]* A mark to close the position. Set to false by default. It will close all the positions when closeOrder is true. |
| forceHold | boolean | *[optional]*  A mark to forcely hold the funds for an order, even though it's an order to reduce the position size. This helps the order stay on the order book and not get canceled when the position size changes. Set to false by default.
|

See **Advanced Description** for more details.

#### LIMIT ORDER PARAMETERS

| Param       | type    | Description                                                  |
| ----------- | ------- | ------------|
| price       | String  | Limit price   |
| size        | Integer  | Order size. Must be a positive number |
| timeInForce | String  | *[optional]* **GTC**, **IOC**(default is **GTC**), read [Time In Force](#time-in-force)  |
| postOnly    | boolean | *[optional]*  Post only flag, invalid when **timeInForce** is **IOC**. When postOnly chose, not allowed choose hidden or iceberg.       |
| hidden      | boolean | *[optional]*  Orders not displaying in order book. When hidden chose, not allowed choose postOnly.|
| iceberg    | boolean | *[optional]*  Only visible portion of the order is displayed in the order book. When iceberg chose, not allowed choose postOnly. |
| visibleSize | Integer  | *[optional]*  The maximum visible size of an iceberg order   |

#### MARKET ORDER PARAMETERS

| Param | type | Description
| --------- | ------- | -----------
| size | Integer | *[optional]*  amount of contract to buy or sell 

### Example
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


### Advanced Description
#### SYMBOL

The symbol must match a contract symbol, e.g. XBTUSDM   

#### CLIENT ORDER ID
Generated by yourself, the optional clientOid field must be a unique id (e.g UUID). Only numbers, characters, underline(_) and separator(-) are allowed. The value will be returned in order detail. You can use this field to identify your orders via the public feed. The client_oid is different from the server-assigned order id. Please do not send a repeated client_oid. **The length of the client_oid cannot exceed 40 characters.**
You should record the server-assigned order_id as it will be used for future query order status.

### TYPE
The order type you specify may decide whether other optional parameters are required, as well as how your order will be executed by the matching engine. If order type is not specified, the order will be a **limit** order by default.

**Price** and **size** are required to be specified for a **limit order**.  The order will be filled at the price specified or better, depending on the market condition.   If a limit order cannot be filled immediately, it will be outstanding in the open order book until matched by another order, or canceled by the user.


A **market order** differs from a limit order in that the execution price is not guaranteed. Market order, however, provides a way to buy or sell specific size of contract without having to specify the price. Market orders will be executed immediately, and no orders will enter the open order book afterwards. Market orders are always considered takers and incur taker fees.


### LEVERAGE

The “leverage” parameter is used to calculate the margin to be frozen for the order. If you are to close the position, this parameter is not required.

### STOP ORDERS
A **stop order** is an order to buy or sell at the market or pre-specified limit price once the contact has traded at or through a pre-specified stopPrice. There are two types of stop orders, **down and up.**

**Stop Order Types**

**down:** Triggers when the price reaches or goes below the stopPrice.

**up:** Triggers when the price reaches or goes above the stopPrice.

**stopPriceType:** There are three types of stop prices for contract, including: TP for trade price, MP for mark price, and IP for index price.
The last trade price is the last price at which an order was filled. This price can be found in the latest match message. 
The mark price and the index price can be obtained through relevant OPEN API for index services.

Note that when triggered, stop orders will be executed as either market or limit orders, depending on the pre-specified type.

When placing a stop order, the system will not pre-freeze the funds in your account. Do note that when triggered, a stop order may be canceled if the available balance is not enough.

### PRICE
The price specified must be a multiple number of the contract tickSize, otherwise the system will report an error when you place the order. The tick size is the smallest price increment in which the prices are quoted. A valid price shall not be higher than the maxPrice in the contract specification. **KuCoin Futures implements IEPR(Immediately Executable Price Range) rule**, in which the price of a buy order cannot be higher than **1.05 * mark price**, and the price of a sell order cannot be lower than **0.95 * mark price**.

Price field is not required for market orders.

### SIZE
The size must be no less than the lotSize for the contract and no larger than the maxOrderQty. It should be a multiple number of lotSize, or the system will report an error when you place the order. Size indicates the amount of contract to buy or sell.
Size is the number or lot size of the contract. Eg. the lot size of XBTUSDTM is 0.001 Bitcoin, the lot size of XBTUSDM is 1 USD.


### TIME IN FORCE
Time in force is a special instruction used when placing an order to indicate how long an order will remain active before it is executed or expires. There are two types, Good Till Canceled **GTC** and Immediate Or Cancel **IOC**.

**GTC** Good Till Canceled: order remains open on the order book until canceled. This is the default type if the field is left empty.

**IOC** Immediate Or Cancel: being matched or not, the remaining size of the order will be instantly canceled instead of entering the order book.

**Note that self trades belong to match as well.**

### POST ONLY
The post-only flag ensures that the trader always pays the maker fee and provides liquidity to the order book. If any part of the order is going to pay taker fee, the order will be fully rejected.

### HIDDEN AND ICEBERG

The **Hidden** and **iceberg Orders** are two **options** in advanced settings (note: the iceberg order is a special form of the hidden order). You may select “Hidden” or “Iceberg” when placing a limit or stop limit order.

A hidden order will enter but not display on the orderbook.

Different from the hidden order, an **iceberg order** is divided into visible portion and invisible portion. When placing an iceberg order, you need to set the **visible size**. The minimum visible size is 1/20 of the order size. The minimum visible size shall be greater than the minimum order size, or an error will occur.

In a matching event, the visible portion of an iceberg order will be executed first, and another visible portion will pop up until the order is fully filled.

**Note**: 1)The system will charge **taker fees** for **Hidden** and **iceberg Orders**. 2)If both "Iceberg" and "Hidden" are selected, your order will be filled as an **iceberg Order** by default.

### HOLDS
When placing an order, the system will freeze certain amount of funds in your account for position margin and transaction fees based on the order price and quantity. If not, the order can only be one to reduce the position. If the total amount of these orders exceeds the position size, the system will cancel the extra no-fund-frozen orders to ensure they won’t be executed. 

Actual fees are determined when the order is executed. If you cancel a partially filled or unfilled order, any remaining funds will be released from hold and become available.

Different from when an order reduces the position size, certain amount of funds need to be frozen when an order increases the position size. No funds need to be frozen when closeOrder is set to TRUE, or when reduceOnly is set to TRUE.

The execution of the order will incur  transaction fees. If a partially filled or unfilled order is canceled, the system will unfreeze the remained frozen funds in your account.

### CLOSE ORDER
If closeOrder is set to TRUE, the system will close the position and the position size will become 0. Side, Size and Leverage fields can be left empty and the system will determine the side and size automatically.

### CLOSE ONLY
If set to TRUE, only the orders reducing the position size will be executed. If the reduce-only order size exceeds the position size, the extra size will be canceled.

### FORCE HOLD
The system will forcely freeze certain amount of funds for this order, including orders whose direction is opposite to the current positions. This feature is to ensure that the order won’t be canceled by the matching engine in such a circumstance that not enough funds are frozen for the order.

### ORDER LIFECYCLE
The HTTP Request will respond when an order is either rejected (insufficient funds, invalid parameters, etc) or received (accepted by the matching engine). A success response with order id indicates that the order has been received. Orders may be execute either partially or fully. After a partial execution, the remaining size of the order will be in **active** state (excluding IOC orders). A completely filled order will be in **done** state.

Users listening to streaming market data are encouraged to use the order id and clientOid field to identify their received messages in the feed.

### RESPONSE
A successful order will be assigned an order id. A successful order is defined as one that has been accepted by the matching engine.

**Open orders will remain open until they are either filled or canceled.**

## Cancel an Order

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

Cancel an order (including a stop order). 

You will receive success message once the system has received the cancellation request. The cancellation request will be processed by matching engine in sequence. To know if the request has been processed, you may check the order status or update message from the pushes.

The **order id** is the server-assigned order id，not the specified clientOid.

If the order can not be canceled (already filled or previously canceled, etc), then an error response will indicate the reason in the **message** field.

### HTTP Request
DELETE /api/v1/orders/{order-id}

### Example
DELETE /api/v1/orders/5cdfc120b21023a909e5ad52

### API Permission ###
This endpoint requires the **Trade** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **40 times/3s**.

## Limit Order Mass Cancelation

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

Cancel all open orders (excluding stop orders). The response is a list of orderIDs of the canceled orders.

### HTTP Request
DELETE /api/v1/orders

### Example
DELETE /api/v1/orders?symbol=XBTUSDM

### API Permission
This endpoint requires the **Trade** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

### PARAMETERS

Param | Type | Description
--------- | ------- | -----------
symbol | String | *[optional]* Cancel all limit orders for a specific contract only

You can delete specific symbol using query parameters. If not specified, all the limit orders will be deleted.

## Stop Order Mass cancelation

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

Cancel all untriggered stop orders. The response is a list of orderIDs of the canceled stop orders. To cancel triggered stop orders, please use 'Limit Order Mass Cancelation'.


### HTTP Request
DELETE /api/v1/stopOrders

### Example
DELETE /api/v1/stopOrders?symbol=XBTUSDM


### API Permission
This endpoint requires the **Trade** permission.

### PARAMETERS
You can delete specific symbol using query parameters.

Param | Type | Description
--------- | ------- | -----------
symbol | String | *[optional]* Cancel all untriggered stop orders for a specific contract only


## Get Order List

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
          "id": "5cdfc138b21023a909e5ad55", //Order ID
          "symbol": "XBTUSDM",  //Ticker symbol of the contract
          "type": "limit",   //Order type, market order or limit order
          "side": "buy",  //Transaction side
          "price": "3600",  //Order price
          "size": 20000,  //Order quantity
          "value": "56.1167227833",  //Order value
          "filledValue": "0",  //Value of the executed orders
          "filledSize": 0,  //Executed order quantity
          "stp": "",  //Self trade prevention types
          "stop": "",  //Stop order type (stop limit or stop market) 
          "stopPriceType": "",  //Trigger price type of stop orders 
          "stopTriggered": false,  //Mark to show whether the stop order is triggered
          "stopPrice": null,  //Trigger price of stop orders
          "timeInForce": "GTC",  //Time in force policy type 
          "postOnly": false,  // Mark of post only
          "hidden": false,  //Mark of the hidden order
          "iceberg": false,  //Mark of the iceberg order 
          "visibleSize": null,  //Visible size of the iceberg order
          "leverage": "20",  //Leverage of the order
          "forceHold": false,  //A mark to forcely hold the funds for an order 
          "closeOrder": false, //A mark to close the position 
          "reduceOnly": false,  //A mark to reduce the position size only
          "clientOid": "5ce24c16b210233c36ee321d",  //Unique order id created by users to identify their orders  
          "remark": null,  //Remark of the order 
          "isActive": true,  //Mark of the active orders
          "cancelExist": false,  //Mark of the canceled orders
          "createdAt": 1558167872000,  //Time the order created
          "settleCurrency": "XBT", //settlement currency
          "status": "open", //order status: “open” or “done”
          "updatedAt": 1558167872000, //last update time
          "orderTime": 1558167872000000000 //order create time in nanosecond
        }
      ]
    }  
 }
```

List your current orders.

### HTTP Request
GET /api/v1/orders

### Example
GET /api/v1/orders?status=active  
Submit the request to get all the active orders.

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **30 times/3s**.

### PARAMETERS
You can request for specific orders using query parameters.

Param | Type | Description
--------- | ------- | -----------
status | String |*[optional]* **active** or **done**, done as default. Only list orders for a specific status
symbol |String|*[optional]* Symbol of the contract
side | String | *[optional]* **buy** or **sell** 
type | String | *[optional]* **limit**, **market**, **limit_stop** or **market_stop** 
startAt | long | *[optional]* Start time (milisecond)
endAt | long | *[optional]* End time (milisecond)

**This request is paginated.**

#### Order Status and Settlement
Any limit order on the exchange order book is in **active** status. Orders removed from the order book will be marked with **done** status. After an order becomes done, there may be a few milliseconds latency before it’s fully settled.

You can check the orders in any **status**. If the status parameter is not specified, orders of done status will be returned by default.

When you query orders in **active** status, there is no time limit. However, when you query orders in done status, the start and end time range cannot exceed 24*7 hours. An error will occur if the specified time window exceeds the range. If you specify the end time only, the system will automatically calculate the start time as end time minus 24 hours, and vice versa.

#### POLLING
For high-volume trading, it is highly recommended that you maintain your own list of open orders and use one of the streaming market data feeds to keep it updated. You should poll the open orders endpoint to obtain the current state of any open order.

If you need to get your recent trade history with low latency, you may query the endpoint Get List of Orders Completed in 24h.


## Get Untriggered Stop Order List

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
            "id": "5cdfc138b21023a909e5ad55", //Order ID
            "symbol": "XBTUSDM",  //Ticker symbol of the contract 
            "type": "limit",   //Order type, market order or limit order
            "side": "buy",  //Transaction side
            "price": "3600",  //Order price
            "size": 20000,  //Order quantity
            "value": "56.1167227833",  //Order value
            "filledValue": "0",  //Value of the executed orders
            "filledSize": 0,  //Executed order quantity
            "stp": "",  //Self trade prevention types
            "stop": "",  //Stop order type (stop limit or stop market) 
            "stopPriceType": "",  //Trigger price type of stop orders
            "stopTriggered": false,  //Mark to show whether the stop order is triggered
            "stopPrice": null,  //Trigger price of stop orders
            "timeInForce": "GTC",  //Time in force policy type
            "postOnly": false,  //Mark of post only
            "hidden": false,  //Mark of the hidden order
            "iceberg": false,  //Mark of the iceberg order
            "visibleSize": null,  //Visible size of the iceberg order
            "leverage": "20",  //Leverage of the order
            "forceHold": false,  //A mark to forcely hold the funds for an order
            "closeOrder": false, //A mark to close the position
            "reduceOnly": false,  //A mark to reduce the position size only
            "clientOid": "5ce24c16b210233c36ee321d",  //Unique order id created by users to identify their orders 
            "remark": null,  //Remark of the order
            "isActive": true,  //Mark of the active orders
            "cancelExist": false,  //Mark of the canceled orders
            "createdAt": 1558167872000,  //Time the order created
            "settleCurrency": "XBT", //settlement currency
            "status": "open", //order status: “open” or “done”
            "updatedAt": 1558167872000 //last update time
        }
      ]
    }
 }
```
Get the un-triggered stop orders list.

### HTTP Request
GET /api/v1/stopOrders

### Example
GET /api/v1/stopOrders?symbol=XBTUSDM

Query this endpoint to get the untriggered stop orders of the position in XBTUSDM.

### API Permission
This endpoint requires the **General** permission.

### PARAMETERS
You can request for specific orders using query parameters.

Param | Type | Description
--------- | ------- | -----------
symbol |String|*[optional]* Symbol of the contract
side | String | *[optional]* **buy** or **sell** 
type | String | *[optional]* **limit**, **market** 
startAt | long | *[optional]* Start time (milisecond)
endAt | long | *[optional]* End time (milisecond)

**This request is paginated.**

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Get List of Orders Completed in 24h 

```json
  {
    "code": "200000",
    "data":[
       {
            "id": "5cdfc138b21023a909e5ad55", //Order ID
            "symbol": "XBTUSDM",  //Ticker symbol of the contract
            "type": "limit",   //Order type, market order or limit order
            "side": "buy",  //Transaction side
            "price": "3600",  //Order price
            "size": 20000,  //Order quantity
            "value": "56.1167227833",  //Order value
            "filledValue": "56.1167227833",  //Value of the executed orders
            "filledSize": 20000,  //Executed order quantity
            "stp": "",  //Self trade prevention types
            "stop": "",  //Stop order type (stop limit or stop market) 
            "stopPriceType": "",  //Trigger price type of stop orders
            "stopTriggered": true,  //Mark to show whether the stop order is triggered
            "stopPrice": null,  //Trigger price of stop orders
            "timeInForce": "GTC",  //Time in force policy type
            "postOnly": false,  //Mark of post only
            "hidden": false,  //Mark of the hidden order
            "iceberg": false,  //Mark of the iceberg order
            "visibleSize": null,  // Visible size of the iceberg order
            "leverage": "20",  //Leverage of the order
            "forceHold": false,  //A mark to forcely hold the funds for an order
            "closeOrder": false, //A mark to close the position
            "reduceOnly": false,  //A mark to reduce the position size only
            "clientOid": "5ce24c16b210233c36ee321d",  //Unique order id created by users to identify their orders
            "remark": null,  //Remark of the order
            "isActive": false,  //Mark of the active orders
            "cancelExist": false,  //Mark of the canceled orders
            "createdAt": 1558167872000,  //Time the order created
            "settleCurrency": "XBT", //settlement currency
            "status": "done", //order status: “open” or “done”
            "updatedAt": 1558167872000, //last update time
            "orderTime": 1558167872000000000 //order create time in nanosecond
          }
      ]
 }
```

Get a list of recent 1000 orders in the last 24 hours. 

If you need to get your recent traded order history with low latency, you may query this endpoint.

### HTTP Request
GET /api/v1/recentDoneOrders

### Example
GET /api/v1/recentDoneOrders

### API Permission
This endpoint requires the **General** permission.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Get Details of a Single Order

```json
  {
    "code": "200000",
    "data": {
      "id": "5cdfc138b21023a909e5ad55", //Order ID
      "symbol": "XBTUSDM",  //Ticker symbol of the contract
      "type": "limit",   //Order type, market order or limit order
      "side": "buy",  //Transaction side
      "price": "3600",  //Order price 
      "size": 20000,  //Order quantity
      "value": "56.1167227833",  //Order value
      "filledValue": "56.1167227833",  //Value of the executed orders
      "filledSize": 20000,  //Executed order quantity
      "stp": "",  //Self trade prevention types
      "stop": "",  //Stop order type (stop limit or stop market)
      "stopPriceType": "",  //Trigger price type of stop orders
      "stopTriggered": true,  //Mark to show whether the stop order is triggered
      "stopPrice": null,  //Trigger price of stop orders
      "timeInForce": "GTC",  //Time in force policy types
      "postOnly": false,  //Mark of post only
      "hidden": false,  //Mark of the hidden order
      "iceberg": false,  //Mark of the iceberg order
      "visibleSize": null,  //Visible size of the iceberg order
      "leverage": "20",  //Leverage of the order
      "forceHold": false,  //A mark to forcely hold the funds for an order
      "closeOrder": false, //A mark to close the position
      "reduceOnly": false,  //A mark to reduce the position size only
      "clientOid": "5ce24c16b210233c36ee321d",  //Unique order id created by users to identify their orders
      "remark": null,  //Remarks of the order
      "isActive": false,  //Mark of the active orders
      "cancelExist": false,  //Mark of the canceled orders
      "createdAt": 1558167872000,  //Time the order created
      "settleCurrency": "XBT", //settlement currency
      "status": "done", //order status: “open” or “done”
      "updatedAt": 1558167872000, //last update time
      "orderTime": 1558167872000000000 //order create time in nanosecond
    }
}
```

Get a single order by order id (including a stop order).

### HTTP Request
GET /api/v1/orders/{order-id}?clientOid={client-order-id}

### Example
GET /api/v1/orders/5cdfc138b21023a909e5ad55 (get order by orderId), </br>
GET /api/v1/orders/byClientOid?clientOid=eresc138b21023a909e5ad59 (get order by clientOid)

### API Permission
This endpoint requires the **General** permission.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Fills

## Get Fills

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
            "symbol": "XBTUSDM",  //Ticker symbol of the contract
            "tradeId": "5ce24c1f0c19fc3c58edc47c",  //Trade ID
            "orderId": "5ce24c16b210233c36ee321d",  // Order ID 
            "side": "sell",  //Transaction side
            "liquidity": "taker",  //Liquidity- taker or maker 
            "price": "8302",  //Filled price  
            "size": 10,  //Filled amount
            "value": "0.001204529",  //Order value
            "feeRate": "0.0005",  //Floating fees
            "fixFee": "0.00000006",  //Fixed fees
            "feeCurrency": "XBT",  //Charging currency  
            "stop": "",  //A mark to the stop order type
            "fee": "0.0000012022",  //Transaction fee
            "orderType": "limit",  //Order type
            "tradeType": "trade",  //Trade type (trade, liquidation, ADL or settlement)
            "createdAt": 1558334496000,  //Time the order created
            "settleCurrency": "XBT", //settlement currency
            "tradeTime": 1558334496000000000 //trade time in nanosecond
          }
      ]
    }
}
```

Get a list of recent fills.

### HTTP Request
GET /api/v1/fills

### Example
GET /api/v1/fills

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

### PARAMETERS
You can request fills for specific orders using query parameters. If you need to get your recent trade history with low latency, please query endpoint Get List of Orders Completed in 24H. The requested data is not real-time.  

Param | Type | Description
--------- | ------- | -----------
orderId | String |*[optional]* List fills for a specific order only (If you specify orderId, other parameters can be ignored)
symbol | String |*[optional]* Symbol of the contract
side | String |*[optional]* **buy** or **sell** 
type | String |*[optional]* **limit**, **market**, **limit_stop** or **market_stop** 
startAt | long |*[optional]* Start time (milisecond)
endAt | long |*[optional]* End time (milisecond)

**This request uses pagination.**

**Data Time Range**
The system allows you to retrieve data up to one week (start from the last day by default). If the time period of the queried data exceeds one week (time range from the start time to end time exceeded 24*7 hours), the system will prompt to remind you that you have exceeded the time limit. If you only specified the start time, the system will automatically calculate the end time (end time = start time + 24 hours). On the contrary, if you only specified the end time, the system will calculate the start time (start time= end time - 24 hours) the same way.

**Fee**

Orders on KuCoin Futures platform are classified into two types， **taker** and **maker**. A **taker** order matches other resting orders on the exchange order book, and gets executed immediately after order entry. A **maker** order, on the contrary, stays on the exchange order book and awaits to be matched. Taker orders will be charged taker fees, while maker orders will receive maker rebates. Please note that market orders, **iceberg orders** and **hidden orders** are always charged taker fees. 

The system will pre-freeze a predicted taker fee when you place an order.The liquidity field indicates if the fill was charged taker or maker fees.

The system will pre-freeze the predicted fees (including the maintenance margin needed for the position, entry fees and fees to close positions) if you added the position, and will not pre-freeze fees if you reduced the position. After the order is executed, if you added positions, the system will deduct entry fees from your balance, if you closed positions, the system will deduct the close fees. 

### PAGINATION
Fills are returned sorted by descending fill time.

**This request is paginated.**

## Recent Fills

```json
  {
    "code": "200000",
    "data":[
     {
         "symbol": "XBTUSDM",  //Ticker symbol of the contract
         "tradeId": "5ce24c1f0c19fc3c58edc47c",  //Trade ID
         "orderId": "5ce24c16b210233c36ee321d",  //Order ID
         "side": "sell",  //Transaction side
         "liquidity": "taker",  // Liquidity-taker or maker
         "price": "8302",  //Filled price
         "size": 10,  //Filled amount
         "value": "0.001204529",  //Order value
         "feeRate": "0.0005",  //Floating rate
         "fixFee": "0.00000006",  //Fixed fees
         "feeCurrency": "XBT",  //Charging currency
         "stop": "",  //A mark to the stop order type
         "fee": "0.0000012022",  //Transaction fee
         "orderType": "limit",  //Order type
         "tradeType": "trade",  //Trade type (tradek, liquidation or ADL )
         "createdAt": 1558334496000,  //Time the order created
         "settleCurrency": "XBT", //settlement currency
         "tradeTime": 1558334496000000000 //trade time in nanosecond
     }  
    ] 
  }
```

Get a list of recent 1000 fills in the last 24 hours. If you need to get your recent traded order history with low latency, you may query this endpoint.

### HTTP Request
GET /api/v1/recentFills

### Example
GET /api/v1/recentFills

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

## Active Order Value Calculation

```json
{
  "code": "200000",
  "data": {
        "openOrderBuySize": 20,  //Total number of the unexecuted buy orders 
        "openOrderSellSize": 0,  //Total number of the unexecuted sell orders
        "openOrderBuyCost": "0.0025252525",  //Value of all the unexecuted buy orders
        "openOrderSellCost": "0",   //Value of all the unexecuted sell orders
        "settleCurrency": "XBT" //settlement currency
    }  
  }
```
You can query this endpoint to get the the total number and value of the all your active orders.

### HTTP Request
GET /api/v1/openOrderStatistics

### Example
GET /api/v1/openOrderStatistics?symbol=XBTUSDM

### API Permission
This endpoint requires the **General** permission.

### PARAMETERS
Param | Type | Description
--------- | ------- | -----------
symbol |String| Symbol of the contract


# Positions

## Get Position Details

```json
  {
    'code': '200000',
    'data': {
    'id': '5e81a7827911f40008e80715',                //Position ID
    'symbol': 'XBTUSDTM',                            //Symbol
    'autoDeposit': False, 													 //Auto deposit margin or not
    'maintMarginReq': 0.005, 												 //Maintenance margin requirement
    'riskLimit': 2000000, 													 //Risk limit 
    'realLeverage': 5.0, 														 //Leverage o the order
    'crossMode': False, 														 //Cross mode or not
    'delevPercentage': 0.35, 												 //ADL ranking percentile
    'openingTimestamp': 1623832410892, 							 //Open time
    'currentTimestamp': 1623832488929, 							 //Current timestamp
    'currentQty': 1, 																 //Current postion quantity
    'currentCost': 40.008, 													 //Current postion value
    'currentComm': 0.0240048, 											 //Current commission
    'unrealisedCost': 40.008, 											 //Unrealised value
    'realisedGrossCost': 0.0, 											 //Accumulated realised gross profit value
    'realisedCost': 0.0240048, 											 //Current realised position value
    'isOpen': True, 																 //Opened position or not
    'markPrice': 40014.93, 													 //Mark price
    'markValue': 40.01493, 													 //Mark value
    'posCost': 40.008, 															 //Position value
    'posCross': 0.0, 																 //added margin
    'posInit': 8.0016, 															 //Leverage margin
    'posComm': 0.02880576, 													 //Bankruptcy cost
    'posLoss': 0.0, 													 			 //Funding fees paid out
    'posMargin': 8.03040576, 												 //Position margin
    'posMaint': 0.23284656, 												 //Maintenance margin
    'maintMargin': 8.03733576, 											 //Position margin
    'realisedGrossPnl': 0.0, 												 //Accumulated realised gross profit value
    'realisedPnl': -0.0240048, 											 //Realised profit and loss
    'unrealisedPnl': 0.00693, 											 //Unrealised profit and loss
    'unrealisedPnlPcnt': 0.0002, 										 //Profit-loss ratio of the position
    'unrealisedRoePcnt': 0.0009, 										 //Rate of return on investment
    'avgEntryPrice': 40008.0, 											 //Average entry price
    'liquidationPrice': 32211.0, 										 //Liquidation price
    'bankruptPrice': 32006.0, 											 //Bankruptcy price
    'settleCurrency': 'USDT', 											 //Currency used to clear and settle the trades
  }

```

Get the position details of a specified position.

### HTTP Request
GET /api/v1/position


### Example
GET /api/v1/position?symbol=XBTUSDM

### API Permission
This endpoint requires the **General** permission.

### PARAMETERS

| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Symbol of the contract   |

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Get Position List                   

```json
   {
    'code': '200000',
    'data': {
    'id': '5e81a7827911f40008e80715',                //Position ID
    'symbol': 'XBTUSDTM',                            //Symbol
    'autoDeposit': False, 													 //Auto deposit margin or not
    'maintMarginReq': 0.005, 												 //Maintenance margin requirement
    'riskLimit': 2000000, 													 //Risk limit 
    'realLeverage': 5.0, 														 //Leverage o the order
    'crossMode': False, 														 //Cross mode or not
    'delevPercentage': 0.35, 												 //ADL ranking percentile
    'openingTimestamp': 1623832410892, 							 //Open time
    'currentTimestamp': 1623832488929, 							 //Current timestamp
    'currentQty': 1, 																 //Current postion quantity
    'currentCost': 40.008, 													 //Current postion value
    'currentComm': 0.0240048, 											 //Current commission
    'unrealisedCost': 40.008, 											 //Unrealised value
    'realisedGrossCost': 0.0, 											 //Accumulated realised gross profit value
    'realisedCost': 0.0240048, 											 //Current realised position value
    'isOpen': True, 																 //Opened position or not
    'markPrice': 40014.93, 													 //Mark price
    'markValue': 40.01493, 													 //Mark value
    'posCost': 40.008, 															 //Position value
    'posCross': 0.0, 																 //added margin
    'posInit': 8.0016, 															 //Leverage margin
    'posComm': 0.02880576, 													 //Bankruptcy cost
    'posLoss': 0.0, 													 			 //Funding fees paid out
    'posMargin': 8.03040576, 												 //Position margin
    'posMaint': 0.23284656, 												 //Maintenance margin
    'maintMargin': 8.03733576, 											 //Position margin
    'realisedGrossPnl': 0.0, 												 //Accumulated realised gross profit value
    'realisedPnl': -0.0240048, 											 //Realised profit and loss
    'unrealisedPnl': 0.00693, 											 //Unrealised profit and loss
    'unrealisedPnlPcnt': 0.0002, 										 //Profit-loss ratio of the position
    'unrealisedRoePcnt': 0.0009, 										 //Rate of return on investment
    'avgEntryPrice': 40008.0, 											 //Average entry price
    'liquidationPrice': 32211.0, 										 //Liquidation price
    'bankruptPrice': 32006.0, 											 //Bankruptcy price
    'settleCurrency': 'USDT', 											 //Currency used to clear and settle the trades
  }
```

Get the position details of a specified position.

### HTTP Request
GET /api/v1/positions

### Example
GET /api/v1/positions

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Enable/Disable of Auto-Deposit Margin

### HTTP Request
POST /api/v1/position/margin/auto-deposit-status

### Example
POST /api/v1/position/margin/auto-deposit-status

### API Permission
This endpoint requires the **General** permission.

### PARAMETERS

| Param  | Type    | Description |
| ------ | ------- | ----------- |
| symbol | String  | Symbol of the contract   |
| status | boolean | Status        |


## Add Margin Manually 

### HTTP Request
POST /api/v1/position/margin/deposit-margin

### Example
POST /api/v1/position/margin/deposit-margin

### API Permission
This endpoint requires the **General** permission.

### PARAMETERS
| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Ticker symbol of the contract    |
| margin | Number | Margin amount (min. margin amount≥0.00001667XBT） |
| bizNo  | String | A unique ID generated by the user, to ensure the operation is processed by the system only once |

# Funding Fees
## Get Funding History

```json
  {
    "dataList": [
      {
        "id": 36275152660006,                //id
        "symbol": "XBTUSDM",                  //Symbol
        "timePoint": 1557918000000,          //Time point (milisecond) 
        "fundingRate": 0.000013,             //Funding rate
        "markPrice": 8058.27,                //Mark price
        "positionQty": 10,                   //Position size  
        "positionCost": -0.001241,           //Position value at settlement period
        "funding": -0.00000464,              //Settled funding fees. A positive number means that the user received the funding fee, and vice versa.
        "settleCurrency": "XBT"             //settlement currency
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
    "hasMore": true                         //Whether there are more pages
  }
```
Submit request to get the funding history.

### HTTP Request
GET /api/v1/funding-history

### Example
GET /api/v1/funding-history?symbol=XBTUSDM

### API Permission
This endpoint requires the **General** permission.

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **9 times/3s**.

### PARAMETERS

| Param     | Type    | Description                                                  |
| --------- | ------- | -------------------- |
| symbol    | String  | Symbol of the contract  |
| startAt | long    | *[optional]* Start time (milisecond) |
| endAt   | long    | *[optional]* End time (milisecond)|
| reverse   | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default|
| offset    | long    | *[optional]*  Start offset. The unique attribute of the last returned result of the last request. The data of the first page will be returned by default.                  |
| forward   | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default |
| maxCount  | int     | *[optional]* Max record count. The default record count is 10                          |


# Market Data
Signature is not required for this part.

# Symbol

## Get Open Contract List

```json
  {
    "code": "200000",
    "data": {
      "baseCurrency": "XBT",  //Base currency
      "fairMethod": "FundingRate", //Fair price marking method
      "fundingBaseSymbol": ".XBTINT8H",  //Ticker symbol of the based currency
      "fundingQuoteSymbol": ".USDINT8H", //Ticker symbol of the quote currency
      "fundingRateSymbol": ".XBTUSDMFPI8H",  //Funding rate symbol
      "indexSymbol": ".KXBT",    //Index symbol
      "initialMargin": 0.01, //Initial margin requirement
      "isDeleverage": true,   //Enabled ADL or not
      "isInverse": true,  //Reverse contract or not
      "isQuanto": false,   //Whether quanto or not
      "lotSize": 1,   //Minimum lot size
      "maintainMargin": 0.005,    //Maintenance margin requirement
      "makerFeeRate": -0.00025,  //Maker fees
      "makerFixFee": -0.0000000200,   //Fixed maker fees
      "markMethod": "FairPrice", //Marking method
      "maxOrderQty": 1000000,   //Maximum order quantity
      "maxPrice": 1000000.0000000000,  //Maximum order price   
      "maxRiskLimit": 200,  //Maximum risk limit (unit: XBT)
      "minRiskLimit": 200,  //Minimum risk limit (unit: XBT)
      "multiplier": -1,    //Contract multiplier
      "quoteCurrency": "USD",  //Quote currency
      "riskStep": 100,  //Risk limit increment value (unit: XBT)
      "rootSymbol": "XBT", //Contract group
      "status": "Open", //Contract status
      "symbol": "XBTUSDM", //Ticker symbol of the contract
      "takerFeeRate": 0.0005,  //Taker fees
      "takerFixFee": 0.0000000600,   //Fixed taker fees
      "tickSize": 1,  //Minimum price changes
      "type": "FFWCSX",  //Type of the contract
      "maxLeverage": 100,   //maximum leverage 
      "volumeOf24h": 14848115, //volume of 24 hours
      "turnoverOf24h": 1590.20278373, //turnover of 24 hours
      "openInterest": "10621721",  //open interest
      "lowPrice": 19445, //24H Low
      "highPrice": 23862, //24H High
      "priceChgPct": 1000, //24H Change%
      "priceChg": 0.1646 //24H Change      
    }
  }
```

Submit request to get the info of all open contracts.

### HTTP Request
GET /api/v1/contracts/active

### Example
GET /api/v1/contracts/active

### PARAMETERS
N/A


<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## Get Order Info. of the Contract

```json
  {
    "code": "200000",
    "data": {
      "baseCurrency": "XBT",  //Base currency
      "fairMethod": "FundingRate", //Fair price marking method
      "fundingBaseSymbol": ".XBTINT8H",  //Ticker symbol of the based currency
      "fundingQuoteSymbol": ".USDINT8H", //Ticker symbol of the quote currency
      "fundingRateSymbol": ".XBTUSDMFPI8H",  //Funding rate symbol
      "indexSymbol": ".KXBT",    //Index symbol
      "initialMargin": 0.01, //Initial margin requirement
      "isDeleverage": true,   //Enabled ADL or not
      "isInverse": true,  //Reverse contract or not
      "isQuanto": false,   //Whether quanto or not
      "lotSize": 1,   //Minimum lot size
      "maintainMargin": 0.005,    //Maintenance margin requirement
      "makerFeeRate": -0.00025,  //Maker fees
      "makerFixFee": -0.0000000200,   //Fixed maker fees
      "markMethod": "FairPrice", //Marking method
      "maxOrderQty": 1000000,   //Maximum order quantity
      "maxPrice": 1000000.0000000000,  //Maximum order price   
      "maxRiskLimit": 200,  //Maximum risk limit (unit: XBT)
      "minRiskLimit": 200,  //Minimum risk limit (unit: XBT)
      "multiplier": -1,    //Contract multiplier
      "quoteCurrency": "USD",  //Quote currency
      "riskStep": 100,  //Risk limit increment value (unit: XBT)
      "rootSymbol": "XBT", //Contract group
      "status": "Open", //Contract status
      "symbol": "XBTUSDM", //Ticker symbol of the contract
      "takerFeeRate": 0.0005,  //Taker fees
      "takerFixFee": 0.0000000600,   //Fixed taker fees
      "tickSize": 1,  //Minimum price changes
      "type": "FFWCSX",    //Type of the contract
      "maxLeverage": 100,   //maximum leverage 
      "lowPrice": 19445, //24H Low
      "highPrice": 23862, //24H High
      "priceChgPct": 1000, //24H Change%
      "priceChg": 0.1646 //24H Change  
    }
  }
```

Submit request to get info of the specified contract.

### HTTP Request
GET /api/v1/contracts/{symbol}

### Example
GET /api/v1/contracts/XBTUSDM

### PARAMETERS
Param | Type | Description
--------- | ------- | -----------
symbol | String | **Path Parameter**. Symbol of the contract

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

# Get Ticker

## Get Real-Time Ticker

```json
  {
    "code": "200000",
    "data": {
      "sequence": 1001,				//Sequence number
      "symbol": "XBTUSDM",				//Symbol
      "side": "buy",					//Side of liquidity taker
      "size": 10,						//Filled quantity
      "price": "7000.0",				//Filled price
      "bestBidSize": 20,				//Best bid size
      "bestBidPrice": "7000.0",		//Best bid
      "bestAskSize": 30,				//Best ask size
      "bestAskPrice": "7001.0",		//Best ask
      "tradeId": "5cbd7377a6ffab0c7ba98b26",  //Transaction ID
      "ts": 1550653727731			   //Filled time - nanosecond
    }
  }
```
The real-time ticker includes the last traded price, the last traded size, transaction ID, the side of liquidity taker, the best bid price and size, the best ask price and size as well as the transaction time of the orders. These messages can also be obtained through Websocket. The Sequence Number is used to judge whether the messages pushed by Websocket is continuous.



### HTTP Request
GET /api/v1/ticker

### Example
GET /api/v1/ticker?symbol=XBTUSDM

### PARAMETERS
Param | Type | Description
--------- | ------- | -----------
symbol | String | Symbol of the contract

# Order Book
## Get Full Order Book - Level 2

```json
  {
    "code": "200000",
    "data": {
      "symbol": "XBTUSDM",		//Symbol
      "sequence": 100,			//Ticker sequence number
      "asks": [
        ["5000.0", 1000],	//Price, quantity
        ["6000.0", 1983]		//Price, quantity
      ],
      "bids": [
        ["3200.0", 800],		//Price, quantity
        ["3100.0", 100]		//Price, quantity
      ],
      "ts": 1604643655040584408  // timestamp
    }
  }
```

Get a snapshot of aggregated open orders for a symbol.

Level 2 order book includes all bids and asks (**aggregated by price**). This level returns only one aggregated size for each price (as if there was only one single order for that price).

This API will return data with **full depth**.

It is generally used by professional traders because it uses more server resources and traffic, and we have strict access frequency control.

To maintain up-to-date Order Book, please use [Websocket](#level-2-market-data) incremental feed after retrieving the Level 2 snapshot.

In the returned data, the sell side is sorted low to high by price and the buy side is sorted high to low by price. 

### HTTP Request
GET /api/v1/level2/snapshot

### Example
GET /api/v1/level2/snapshot?symbol=XBTUSDM

### REQUEST RATE LIMIT
This API is restricted for each account, the request rate limit is **30 times/3s**.

### Parameters
| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Symbol of the contract |

## Get Part Order Book - Level 2

```json
  {
    "code": "200000",
    "data": {
      "symbol": "XBTUSDM",		//Symbol
      "sequence": 100,			//Ticker sequence number
      "asks": [
        ["5000.0", 1000],	//Price, quantity
        ["6000.0", 1983]		//Price, quantity
      ],
      "bids": [
        ["3200.0", 800],		//Price, quantity
        ["3100.0", 100]		//Price, quantity
      ],
      "ts": 1604643655040584408  // timestamp
    }
  }
```

Get a snapshot of aggregated open orders for a symbol.

Level 2 order book includes all bids and asks (**aggregated by price**). This level returns only one aggregated size for each price (as if there was only one single order for that price).

This API will return data with **20 or 100 depth**.

It is generally used by professional traders because it uses more server resources and traffic, and we have strict access frequency control.

To maintain up-to-date Order Book, please use [Websocket](#level-2-market-data) incremental feed after retrieving the Level 2 snapshot.

In the returned data, the sell side is sorted low to high by price and the buy side is sorted high to low by price. 

### HTTP Request
GET /api/v1/level2/depth20

GET /api/v1/level2/depth100

### Example
GET /api/v1/level2/depth100?symbol=XBTUSDM


### Parameters
| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Symbol of the contract |

## Level 2 Pulling Messages

```json
  {
    "code": "200000",
    "data": [
      {
          "symbol": "XBTUSDM",				//Symbol
          "sequence": 1,					//Message sequence number
          "change": "7000.0,sell,10"		//Price, side, quantity
        },
      {
          "symbol": "XBTUSDM",				//Symbol
          "sequence": 2,					//Message sequence number
          "change": "7000.0,sell,0"		//Price, side, quantity
      }
    ]
  }
```
If the messages pushed by Websocket is not continuous, you can submit the following request and re-pull the data to ensure that the sequence is not missing. In the request, the start parameter is the sequence number of your last received message plus 1, and the end parameter is the sequence number of your current received message minus 1. After re-pulling the messages and applying them to your local exchange order book, you can continue to update the order book via Websocket incremental feed. If the difference between the end and start parameter is more than 500, please stop using this request and we suggest you to rebuild the Level 2 orderbook.


Level 2 message pulling method: Take price as the key value and overwrite the local order quantity with the quantity in messages. If the quantity of a certain price in the pushed message is 0, please delete the corresponding data of that price.

### HTTP Request
GET /api/v1/level2/message/query

### Example
GET /api/v1/level2/message/query?symbol=XBTUSDM&start=100&end=200

### Parameters
| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Symbol of the contract   |
| start | long | Start sequence number (included in the returned data)   |
| end | long | End sequence number (included in the returned data)   |



# Historical Data

## Transaction History

```json
  {
    "code": "200000",
    "data": {
			"sequence": 102,					              //Sequence number
			"tradeId": "5cbd7377a6ffab0c7ba98b26",      //Transaction ID
			"takerOrderId": "5cbd7377a6ffab0c7ba98b27", //Taker order ID
			"makerOrderId": "5cbd7377a6ffab0c7ba98b28", //Maker order ID
			"price": "7000.0",                          //Filled price
			"size": 1,                                //Filled quantity
			"side": "buy",                              //Side-taker    			"ts": 1545904567062140823                   //Filled time - nanosecond
		}
  }
```
List the last 100 trades for a symbol.

### HTTP Request
GET /api/v1/trade/history

### Example
GET /api/v1/trade/history?symbol=XBTUSDM

### Parameters
| Param  | Type   | Description |
| ------ | ------ | ----------- |
| symbol | String | Symbol of the contract   |

### Description
**SIDE**
The trade side indicates the taker order side. A taker order is the order that was matched with orders opened on the order book.


# Index
## Get Interest Rate List

```json
  {
    "dataList": [
      {
        "symbol": ".XBTINT",                 //Symbol of the Bitcoin Lending Rate  
        "granularity": 60000,                //粒Granularity (milisecond)
        "timePoint": 1557996300000,          //Time point (milisecond)
        "value": 0.0003                      //Interest rate value
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
    "hasMore": true                          //Whether there are more pages
  }
```

Check interest rate list.

### HTTP Request
GET /api/v1/interest/query

### Example
GET /api/v1/interest/query?symbol=.XBTINT

### PARAMETERS

| Param     | Type    | Description                                                  |
| --------- | ------- | --------------------- |
| symbol    | String  | Symbol of the contract   |
| startAt | long    | *[optional]* Start time (milisecond)|
| endAt   | long    | *[optional]*  End time (milisecond)    |
| reverse   | boolean | *[optional]* This parameter functions to judge whether the lookup is reverse. **True** means “yes”. **False** means no. This parameter is set as **True** by default.  |
| offset    | long    | *[optional]* Start offset. The unique attribute of the last returned result of the last request. The data of the first page will be returned by default.|
| forward   | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default|
| maxCount  | int     | *[optional]* Max record count. The default record count is 10    |


## Get Index List 

```json
{ 
    "dataList": [
      {
        "symbol": ".KXBT",                   //Symbol of Bitcoin spot
        "granularity": 1000,                 //Granularity (milisecond)
        "timePoint": 1557998570000,          //Time point (milisecond)
        "value": 8016.24,                    //Index Value
        "decomposionList": [                 //Component List
          {
            "exchange": "gemini",            //Exchange
            "price": 8016.24,                //Last traded price
            "weight": 0.08042611             //Weight
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
    "hasMore": true                            //Whether there are more pages
  }
```

Check index list

### HTTP Request
GET /api/v1/index/query

### PARAMETERS
| Param     | Type    | Description                                                  |
| --------- | ------- | ----------------------|
| symbol    | String  | Symbol of the contract   |
| startAt | long    | *[optional]*  Start time (milisecond) |
| endAt   | long    | *[optional]*  End time (milisecond) |
| reverse   | boolean | *[optional]* This parameter functions to judge whether the lookup is reverse. **True** means “yes”.         **False** means no. This parameter is set as **True** by default |
| offset    | long    | *[optional]* Start offset. The unique attribute of the last returned result of the last request. The data of the first page will be returned by default. |
| forward   | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default |
| maxCount  | int     | *[optional]* Max record count. The default record count is 10               |

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Get Current Mark Price

```json
  {
    "symbol": "XBTUSDM",                //Symbol
    "granularity": 1000,               //Granularity (milisecond)
    "timePoint": 1557999585000,        //Time point (milisecond)
    "value": 8052.51,                  //Mark price
    "indexPrice": 8041.95              //Index price
  }
```

Check the current mark price.

### HTTP Request
GET /api/v1/mark-price/{symbol}/current

### Example
GET /api/v1/mark-price/XBTUSDM/current


### PARAMETERS

| Param  | Type   | Description |
| ------ | ------  | ----------- |
| symbol | String | **Path Parameter**. Symbol of the contract  |



## Get Premium Index

```json
  {
    "dataList": [
      {
        "symbol": ".XBTUSDMPI",              //Premium index symbol
        "granularity": 60000,                //Granularity (milisecond)   
        "timePoint": 1558000320000,          //Time point (milisecond)
        "value": 0.022585                    //Premium index
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
    "hasMore": true                        //Whether there are more pages
  }
```

Submit request to get premium index.

### HTTP Request
GET /api/v1/premium/query


### Example
GET /api/v1/premium/query?symbol=XBTUSDM


### PARAMETERS

| Param     | Type    | Description                                                  |
| --------- | ------- | --------------------------- |
| symbol    | String  | Symbol of the contract       |
| startAt | long    | *[optional]* Start time (milisecond) |
| endAt   | long    | *[optional]* End time (milisecond)          |
| reverse   | boolean | *[optional]* This parameter functions to judge whether the lookup is reverse. **True** means “yes”. **False** means no. This parameter is set as **True** by default|
| offset    | long    | *[optional]* Start offset. The unique attribute of the last returned result of the last request. The data of the first page will be returned by default.|
| forward   | boolean | *[optional]* This parameter functions to judge whether the lookup is forward or not. **True** means “yes” and **False** means “no”. This parameter is set as true by default |
| maxCount  | int     | *[optional]* Max record count. The default record count is 10                      |


## Get Current Funding Rate

```json
  {
    "symbol": ".XBTUSDMFPI8H",              //Funding Rate Symbol
    "granularity": 28800000,               //Granularity (milisecond)   
    "timePoint": 1558000800000,            //Time point (milisecond)
    "value": 0.00375,                      //Funding rate 
    "predictedValue": 0.00375              //Predicted funding rate
  }
```
Submit request to check the current mark price.

### HTTP Request
GET /api/v1/funding-rate/{symbol}/current

### Example
GET /api/v1/funding-rate/XBTUSDM/current


### PARAMETERS

| Param  | Type   | Description    |
| ------ | ------ | -------------- |
| symbol | String | **Path Parameter**. Symbol of the contract. |


# Time

## Server Time

```json
  {  
    "code":"200000",
    "msg":"success",
    "data":1546837113087
  }
```

Get the API server time. This is the Unix timestamp.

### HTTP Request
GET /api/v1/timestamp


# Service Status

## Get the service status


```json
  {    
    "code": "200000",     
    "data": {
        "status": "open",                //open, close, cancelonly
        "msg":  "upgrade match engine"   //remark for operation
      }
  }
```

Get the service status.

### HTTP Request
GET /api/v1/status



# K Chart

## Get K Line Data of Contract

### HTTP Request
GET /api/v1/kline/query

### Example
GET/api/v1/kline/query?symbol=.KXBT&granularity=480&from=1535302400000&to=1559174400000

### Parameters
| Param     | Type    | Description                                                  |
| --------- | ------- | -------------------------- |
| symbol    | String  | [Required]Symbol|
| granularity | int    | [Required]Granularity (minute)|
| from   | long    | [Optional]Start time (milisecond)|
| to   | long | [Optional]End time (milisecond)|

### Response
```json
{
    "code": "200000",
    "data": [
        [
            1575331200000,//Time
            7495.01,      //Entry price
            8309.67,      //Highest price
            7250,         //Lowest price
            7463.55,      //Close price
            0             //Trading volume
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



### Notice
1. The granularity (granularity parameter of K-line) represents the number of minutes, the available granularity scope is: 1,5,15,30,60,120,240,480,720,1440,10080. Requests beyond the above range will be rejected.

2. The maximum size per request is 200. If the specified start/end time and the time granularity exceeds the maximum size allowed for a single request, the system will only return 200 pieces of data for your request. If you want to get fine-grained data in a larger time range, you will need to specify the time ranges and make multiple requests for multiple times.

3. If you’ve specified only the start time in your request, the system will return 200 pieces of data from the specified start time to the current time of the system; If only the end time is specified, the system will return 200 pieces of data closest to the end time; If neither the start time nor the end time is specified, the system will return the 200 pieces of data closest to the current time of the system.

# Websocket
While there is a strict access frequency control for REST API, we highly recommend that API users utilize Websocket to get the real-time data.

**The recommended way is to just create a websocket connection and subscribe to multiple channels.**

## Apply for Connection Token

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

You need to apply for one of the two tokens below to create a websocket connection.


### Public Channels (No authentication is required):

If you only use public channels (e.g. all public market data), please make request as follows to obtain the server list and temporary public token:

#### HTTP Request
POST /api/v1/bullet-public

### Private Channels (Authentication request required):

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

For private channels and messages (e.g. account balance notice), please make request as follows after authorization to obtain the server list and authorized token.


#### HTTP Request
POST /api/v1/bullet-private


### Response

|Field | Description|
-----|-----
|pingInterval| Recommended to send ping interval in millisecond|
|pingTimeout| After such a long time(millisecond), if you do not receive pong, it will be considered as disconnected. |
|endpoint| Websocket server address for establishing connection|
|protocol| Protocol supported|
|encrypt| Indicate whether SSL encryption is used |
|token| token|

## Create Connection

```javascript
var socket = new WebSocket("wss://push.kucoin.com/endpoint?token=xxx&[connectId=xxxxx]");
```
When the connection is successfully established, the system will send a welcome message. 

```json
  {
    "id":"hQvf8jkno",
    "type":"welcome"
  } 
```

**connectId**: the connection id, a unique value taken from the client side. Both the id of the welcome message and the id of the error message are connectId.


## Ping
```json
  {
    "id":"1545910590801",
    "type":"ping"
  }
```

To prevent the TCP link being disconnected by the server, the client side needs to send ping messages to the server to keep alive the link.

After the ping message is sent to the server, the system would return a pong message to the client side.

If the server has not received the ping from the client for **60 seconds** , the connection will be disconnected.

```json
  {
    "id":"1545910590801",
    "type":"pong"
  }
```

## Subscribe Topics

```json
  {
    "id": 1545910660739,                          //The id should be an unique value
    "type": "subscribe",
    "topic": "/market/ticker:XBTUSDM",  //Subscribed topic. Some topics support to divisional subscribe the informations of multiple trading pairs through ",".
    "privateChannel": false,                      //Adopted the private channel or not. Set as false by default.
    "response": true                              //Whether the server needs to return the receipt information of this subscription or not. Set as false by default.
  }
```

To subscribe channel messages from a certain server, the client side should send subscription message to the server.

If the subscription succeeds, the system will send ack messages to you, when the response is set as true.


```json
  {
    "id":"1545910660739",
    "type":"ack"
  }
```
While there are topic messages generated, the system will send the corresponding messages to the client side. For details about the message format, please check the definitions of topics.

### Parameters
#### ID
ID is unique string to mark the request which is same as id property of ack.

#### Topic
The topic you want to subscribe to.

#### Private Channel
For some specific topics (e.g. /contractMarket/level2), **privateChannel** is available. The default value of **privateChannel** is **False**. If the **privateChannel** is set to **true**, the user will only receive messages related himself on the topic. 

#### Response
If the response is set as ture, the system will return the ack messages after the subscription succeed.

## Unsubscribe Topics
Unsubscribe from topics you have subscribed to.

```json
  {
    "id": "1545910840805",                            //The id should be an unique value
    "type": "unsubscribe",
    "topic": "/market/ticker:XBTUSDM",      //Topic needs to be unsubscribed. Some topics support to divisional unsubscribe the informations of multiple trading pairs through ",".
    "privateChannel": false, 
    "response": true,                                  //Whether the server needs to return the receipt information of this subscription or not. Set as false by default.
  }
```

```json
  {
    "id": "1545910840805",
    "type": "ack"
  }
```

### Parameters

#### ID
Unique string to mark the request.

#### Topic
The topic you want to subscribe.

#### PrivateChannel
For some specific **public** topics (e.g. /contractMarket/level2), **privateChannel** is available. The default value of **privateChannel** is **False**. If the **privateChannel** is set to **true**, the user will only receive messages related himself on the topic.

#### Response
If the response is set as ture, the system would return the ack messages after the unsubscription succeed.

## Multiplex
 In one physical connection, you could open different multiplex tunnels to subscribe different **topic**s for different data.

For Example, enter command below to open **bt1** multiple tunnel :
 {"id": "1Jpg30DEdU", "type": "openTunnel", "newTunnelId": "bt1", "response": true}

Add “**tunnelId**” in the command: 
{"id": "1JpoPamgFM", "type": "subscribe", "topic": "/market/ticker:KCS-BTC"，"tunnelId": "bt1", "response": true}

You would then, receive messages corresponded to id **tunnelIId**:  
{"id": "1JpoPamgFM", "type": "message", "topic": "/market/ticker:KCS-BTC", "subject": "trade.ticker", "tunnelId": "bt1", "data": {...}}

To close the **tunnel**, you could enter command below: 
{"id": "1JpsAHsxKS", "type": "closeTunnel", "tunnelId": "bt1", "response": true}

##### Limitations
1. The multiplex **tunnel** is provided for API users only. 
2. The maximum multiplex tunnels available: **5**.

## Sequence Number
The sequence field exists in order book, trade history and snapshot messages by default and the Level 3 and Level 2 data works to ensure the full connection of the sequence. If the sequence is non-sequential, please enable the calibration logic.

## General Logic for Message Judgement in Client Side
1. Judge message type. There are three types of messages at present: message (the commonly used messages for push), notice (the notices general used), and command (consecutive command).
2. Judge messages by userId. Messages with userId are private messages, messages without userId are common messages.
3. Judge messages by topic. You could judge the message type via topic.
4. Judge messages by subject. For the same type of messages with the same topic, you could judge the type of messages via their subjects.

# Public Channels

## Get Real-Time Symbol Ticker v2

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
      "symbol": "XBTUSDM",				//Market of the symbol
      "bestBidSize": 795,				// Best bid size
      "bestBidPrice": 3200.00,			// Best bid
      "bestAskPrice": 3600.00,			// Best ask
      "bestAskSize": 284,				// Best ask size
      "ts": 1553846081210004941		    // Filled time - nanosecond
   }
  }
```
Subscribe this topic to get the realtime push of BBO changes.
After subscription, when there are changes in the order book, the system will push the real-time ticker symbol information to you. 
It is recommended to use the new topic for timely information. 


<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Get Real-Time Symbol Ticker

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
      "symbol": "XBTUSDM",					//Market of the symbol
      "sequence": 45,						//Sequence number which is used to judge the continuity of the pushed messages
      "side": "sell",						//Transaction side of the last traded taker order
      "price": 3600.00,					//Filled price
      "size": 16,							//Filled quantity
      "tradeId": "5c9dcf4170744d6f5a3d32fb",    //Order ID
      "bestBidSize": 795,					//Best bid size
      "bestBidPrice": 3200.00,			//Best bid 
      "bestAskPrice": 3600.00,			//Best ask size
      "bestAskSize": 284,					//Best ask
      "ts": 1553846081210004941		//Filled time - nanosecond
   }
  }
```
Subscribe this topic to get the realtime push of BBO changes.

The ticker channel provides real-time price updates whenever a match happens. If multiple orders are matched at the same time, only the last matching event will be pushed.

It is not recommended to use this topic any more. For real-time ticker information, please subscribe /contractMarket/tickerV2:{symbol}.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Level 2 Market Data

```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/contractMarket/level2:XBTUSDM",
    "response": true                              
  }
```

Topic:**/contractMarket/level2:{symbol}**

Subscribe this topic to get Level 2 order book data.

The websocket system will send the incremental feed to you.

```json
  {
    "subject": "level2",
    "topic": "/contractMarket/level2:XBTUSDM",
    "type": "message",
    "data": {
      "sequence": 18,					//Sequence number which is used to judge the continuity of pushed messages 
      "change": "5000.0,sell,83"		//Price, side, quantity
      "timestamp": 1551770400000 

      }
  }
```

Calibration procedure：

1. After receiving the websocket Level 2 data flow, cache the data.
2. Initiate REST [Level 2 snapshot] (#get-full-order-book-level-2) request to get the snapshot data of Level 2 order book.
3. Playback the cached Level 2 data flow.
4. Apply the new Level 2 data flow to the local snapshot to ensure that the sequence of the new Level 2 update lines up with the sequence of the previous Level 2 data. Discard all the message prior to that sequence, and then playback the change to snapshot.
5. Update the Level 2 full data based on the sequence according to the size. If the size equals to 0, you can update the sequence and remove the price of which the size is 0 out of Level 2. For other cases, please update the price and size.
6. If the sequence of the newly pushed message does not line up to the sequence of the last message, you could pull through REST [Level 2 message](#level-2-pulling-messages) request to get the updated messages. Please note that the difference between the start and end parameters cannot exceed 500.

The change property of Level 2 updates is a string value of "price,size,sequence". Please note that size is the updated size at that price Level. A size of "0" indicates that the price Level can be removed.

**Example**

Get the snapshot of the order book through **REST** request [Level 2 snapshot](#get-full-order-book-level-2)  to build a local order book. Suppose we get the data as following:

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

Thus, the current order book is as following:

| Price   | Size | Side |
| ------- | ---- | ---- |
| 3988.62 | 8    | Sell 4|
| 3988.61 | 32   | Sell 3|
| 3988.60 | 47   | Sell 2|
| 3988.59 | 3    | Sell 1|
| 3988.51 | 56   | Buy 1 |
| 3988.50 | 15   | Buy 2 |
| 3988.49 | 100  | Buy 3 |
| 3988.48 | 10   | Buy 4 |

After subscribing you will receive change message as following:

``` json
  "data": {
    "sequence": 17,
    "change": "3988.50,buy,44"     //Price, side, quantity

  }
```
``` json
  "data": {
    "sequence": 18,
    "change": "3988.61,sell,0"     //Price, side, quantity

  }
```

In the beginning, the sequence of the order book is **16**. Discard the feed data of sequence that is below or equals to **16**, and apply playback the sequence **[17,18]** to update the snapshot of the order book. Now the sequence of your order book is **18** and your local order book is up-to-date.

**Diff:**
1. **Update size of 3988.50 to 44 (Sequence 17)**
2. **Remove 3988.61 (Sequence 18)**

Now your order book is up-to-date and the final data is as following:

| Price   | Size | Side |
| ------- | ---- | ---- |
| 3988.62 | 8    | Sell 3|
| 3988.60 | 47   | Sell 2|
| 3988.59 | 3    | Sell 1|
| 3988.51 | 56   | Buy 1 |
| 3988.50 | 44   | Buy 2 |
| 3988.49 | 100  | Buy 3 |
| 3988.48 | 10  | Buy 4 |

## Execution data 

```json
  {
    "id": 1545910660741,                          
    "type": "subscribe",
    "topic": "/contractMarket/execution:XBTUSDM",
    "response": true                              
  }
```
Topic: **/contractMarket/execution:{symbol}**

For each order executed, the system will send you the match messages in the format as following.

```json
 {
   "topic": "/contractMarket/execution:XBTUSDM",
   "subject": "match",
   "data": {
        "symbol": "XBTUSDM",				//Symbol
        "sequence": 36,						//Sequence number which is used to judge the continuity of the pushed messages  
        "side": "buy",						// Side of liquidity taker
        "matchSize": 1,            //Filled quantity
        "size": 1,							//unFilled quantity
        "price": 3200.00,					// Filled price
        "takerOrderId": "5c9dd00870744d71c43f5e25",  //Taker order ID
        "time": 1553846281766256031,		             //Filled time - nanosecond
        "makerOrderId": "5c9d852070744d0976909a0c",  //Maker order ID
        "tradeId": "5c9dd00970744d6f5a3d32fc"        //Transaction ID
    }
 }
```



## Message channel for the 5 best ask/bid full data of Level 2

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
Returned for every 100 milliseconds at most.


<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## Message channel for the 50 best ask/bid full data of Level 2

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
Returned for every 100 milliseconds at most.




## Contract Market Data 

```json
  //Contract Market Data 
  {
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/instrument:XBTUSDM",   
    "response": true                              
  }
```

Topic: **/contract/instrument:{symbol}**

Subscribe this topic to get the market data of the contract.

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

### Mark Price & Index Price

```json
  //Mark Price & Index Price
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "mark.index.price",
    "data": {
        "granularity": 1000,           //Granularity
        "indexPrice": 4000.23,            //Index price
        "markPrice": 4010.52,           //Mark price
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

### Funding Rate

```json
//Funding Rate
  { 
    "topic": "/contract/instrument:XBTUSDM",
    "subject": "funding.rate",
    "data": {
        "granularity": 60000,  //Granularity (predicted funding rate: 1-min granularity: 60000; Funding rate: 8-hours granularity: 28800000. )
        "fundingRate": -0.002966,     //Funding rate
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

## System Annoucements
Subscribe this topic to get the system announcements.
topic:  **/contract/announcement**

```json
  //System Annoucements
  { 
    "id": 1545910660742,                          
    "type": "subscribe",
    "topic": "/contract/announcement",   
    "response": true                              
  }
```



### Start Funding Fee Settlement

```json
 //Start Funding Fee Settlement
  { 
    "topic": "/contract/announcement",
    "subject": "funding.begin",
    "data": {
        "symbol": "XBTUSDM",                   //Symbol
        "fundingTime": 1551770400000,          //Funding time
        "fundingRate": -0.002966,             //Funding rate
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

### End Funding Fee Settlement

```json
  //End Funding Fee Settlement
  { 
    "type":"message",
    "topic": "/contract/announcement",
    "subject": "funding.end",
    "data": {
        "symbol": "XBTUSDM",                   //Symbol
        "fundingTime": 1551770400000,          //Funding time
        "fundingRate": -0.002966,            //Funding rate
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

## Transaction Statistics Timer Event
The transaction statistics will be pushed to users every 5 seconds.
Topic: **/contractMarket/snapshot:{symbol}**

```json
//Transaction Statistics Timer Event
  { 
    "topic": "/contractMarket/snapshot:XBTUSDM",
    "subject": "snapshot.24h",
    "data": {
        "volume": 30449670,            //24h Volume
        "turnover": 845169919063,      //24h Turnover
        "lastPrice": 3551,           //Last price
        "priceChgPct": 0.0043,         //24h Change
        "ts": 1547697294838004923      //Snapshot time (nanosecond)
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

# Private Channels

## Trade Orders - According To The Market
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders:XBTUSDM",
   "subject": "symbolOrderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //Order ID
       "symbol": "XBTUSDM",  //symbol
       "type": "match",  // Message Type: "open", "match", "filled", "canceled", "update" 
       "status": "open", // Order Status: "match", "open", "done"
       "matchSize": "", // Match Size (when the type is "match")
       "matchPrice": "",// Match Price (when the type is "match")
       "orderType": "limit", // Order Type, "market" indicates market order, "limit" indicates limit order
       "side": "buy",  // Trading direction,include buy and sell
       "price": "3600",  // Order Price
       "size": "20000",  // Order Size
       "remainSize": "20001",  // Remaining Size for Trading
       "filledSize":"20000",  // Filled Size
       "canceledSize": "0",  //  In the update message, the Size of order reduced
       "tradeId": "5ce24c16b210233c36eexxxx",  // Trade ID (when the type is "match")
       "clientOid": "5ce24c16b210233c36ee321d", // clientOid
       "orderTime": 1545914149935808589,  // Order Time
       "oldSize ": "15000", // Size Before Update (when the type is "update")
       "liquidity": "maker", //  Trading direction, buy or sell in taker
       "ts": 1545914149935808589 // Timestamp
   }
}
```
**Order Status**
“match”: when taker order executes with orders in the order book, the taker order status is “match”;
“open”: the order is in the order book;
“done”: the order is fully executed successfully;

**Message Type**
“open”: when the order enters into the order book;
“match”: when the order has been executed;
“filled”: when the order has been executed and its status was changed into DONE;
“canceled”: when the order has been cancelled and its status was changed into DONE;
 “update”: when the order has been updated; 

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## Trade Orders 
```json
{
   "type": "message",
   "topic": "/contractMarket/tradeOrders",
   "subject": "orderChange",
   "channelType": "private",
   "data": {
       "orderId": "5cdfc138b21023a909e5ad55", //Order ID
       "symbol": "XBTUSDM",  //symbol
       "type": "match",  // Message Type: "open", "match", "filled", "canceled", "update" 
       "status": "open", // Order Status: "match", "open", "done"
       "matchSize": "", // Match Size (when the type is "match")
       "matchPrice": "",// Match Price (when the type is "match")
       "orderType": "limit", // Order Type, "market" indicates market order, "limit" indicates limit order
       "side": "buy",  // Trading direction,include buy and sell
       "price": "3600",  // Order Price
       "size": "20000",  // Order Size
       "remainSize": "20001",  // Remaining Size for Trading
       "filledSize":"20000",  // Filled Size
       "canceledSize": "0",  //  In the update message, the Size of order reduced
       "tradeId": "5ce24c16b210233c36eexxxx",  // Trade ID (when the type is "match")
       "clientOid": "5ce24c16b210233c36ee321d", // clientOid
       "orderTime": 1545914149935808589,  // Order Time
       "oldSize ": "15000", // Size Before Update (when the type is "update")
       "liquidity": "maker", //  Trading direction, buy or sell in taker
       "ts": 1545914149935808589 // Timestamp
   }
}
```
**Order Status**
“match”: when taker order executes with orders in the order book, the taker order status is “match”;
“open”: the order is in the order book;
“done”: the order is fully executed successfully;

**Message Type**
“open”: when the order enters into the order book;
“match”: when the order has been executed;
“filled”: when the order has been executed and its status was changed into DONE;
“canceled”: when the order has been cancelled and its status was changed into DONE;
 “update”: when the order has been updated; 

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## Stop Order Lifecycle Event
Topic: **/contractMarket/advancedOrders**

```json
  {
       "userId": "5cd3f1a7b7ebc19ae9558591", // Deprecated, will detele later 
       "topic": "/contractMarket/advancedOrders", 
       "subject": "stopOrder",
       "data": {
           "orderId": "5cdfc138b21023a909e5ad55", //Order ID
           "symbol": "XBTUSDM",  //Ticker symbol of the contract
           "type": "open",  // Message type: open (place order), triggered (order triggered), cancel (cancel order)
           "orderType":"stop", // Order type: stop order
           "side":"buy", // Buy or sell
           "size":"1000", //Quantity 
           "orderPrice":"9000",  //Order price. For market orders, the value is null
           "stop":"up", //Stop order types 
           "stopPrice":"9100", //Trigger price of stop orders
           "stopPriceType":"TP", //Trigger price type of stop orders
           "triggerSuccess": true, //Mark to show whether the order is triggered. Only for triggered type of orders 
           "error": "error.createOrder.accountBalanceInsufficient", //error code, which is used only when the trigger of the triggered type of orders fails
           "createdAt": 1558074652423  //Time the order created
           "ts":1558074652423004000  // timestamp - nanosecond
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

## Account Balance Events

Topic: **/contractAccount/wallet**

### Order Margin Event

```json
  //Order Margin Event
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // Deprecated, will detele later 
    "topic": "/contractAccount/wallet",
    "subject": "orderMargin.change",
    "data": {
        "orderMargin": 5923,//Current order margin
        "currency":"USDT",//Currency
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

### Available Balance Event

```json
//Available Balance Event
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // Deprecated, will detele later 
    "topic": "/contractAccount/wallet",
    "subject": "availableBalance.change",
    "data": {
      "availableBalance": 5923, //Current available amount
      "holdBalance": 2312, //Frozen amount
      "currency":"USDT",//Currency
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

### Withdrawal Amount & Transfer-Out Amount Event

```json
//Withdrawal Amount & Transfer-Out Amount Event
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // Deprecated, will detele later 
    "topic": "/contractAccount/wallet",
    "subject": "withdrawHold.change",
    "data": {
      "withdrawHold": 5923, // Current frozen amount for withdrawal
      "currency":"USDT",//Currency
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

## Position Change Events
Topic: **/contract/position:{symbol}**

### Position Changes Caused Operations

```json
  //Position Changes Caused Operations
  { 
    "userId": "5c32d69203aa676ce4b543c7", // Deprecated, will detele later 
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
      "realisedGrossPnl": 0E-8,                //Accumulated realised profit and loss
      "crossMode": false,                      //Cross mode or not
      "liquidationPrice": 1000000.0,           //Liquidation price
      "posLoss": 0E-8,                         //Manually added margin amount 
      "avgEntryPrice": 7508.22,                //Average entry price
      "unrealisedPnl": -0.00014735,            //Unrealised profit and loss
      "markPrice": 7947.83,                    //Mark price
      "posMargin": 0.00266779,                 //Position margin
      "riskLimit": 200,                        //Risk limit
      "unrealisedCost": 0.00266375,            //Unrealised value
      "posComm": 0.00000392,                   //Bankruptcy cost 
      "posMaint": 0.00001724,                  //Maintenance margin
      "posCost": 0.00266375,                   //Position value
      "maintMarginReq": 0.005,                 //Maintenance margin rate
      "bankruptPrice": 1000000.0,              //Bankruptcy price
      "realisedCost": 0.00000271,              //Currently accumulated realised position value
      "markValue": 0.00251640,                 //Mark value
      "posInit": 0.00266375,                   //Position margin     
      "realisedPnl": -0.00000253,              //Realised profit and losts
      "maintMargin": 0.00252044,               //Position margin 
      "realLeverage": 1.06,                    //Leverage of the order
      "currentCost": 0.00266375,               //Current position value
      "openingTimestamp": 1558433191000,       //Open time
      "currentQty": -20,                       //Current position
      "delevPercentage": 0.52,                 //ADL ranking percentile
      "currentComm": 0.00000271,               //Current commission
      "realisedGrossCost": 0E-8,               //Accumulated reliased gross profit value
      "isOpen": true,                          //Opened position or not
      "posCross": 1.2E-7,                      //Manually added margin
      "currentTimestamp": 1558506060394,       //Current timestamp
      "unrealisedRoePcnt": -0.0553,            //Rate of return on investment
      "unrealisedPnlPcnt": -0.0553,            //Position profit and loss ratio
      "settleCurrency": "XBT"                  //Currency used to clear and settle the trades
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

### Position Changes Caused by Mark Price

```json
  //Position Changes Caused by Mark Price
  { 
    "userId": "5cd3f1a7b7ebc19ae9558591", // Deprecated, will detele later 
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
          "markPrice": 7947.83,                   //Mark price
          "markValue": 0.00251640,                 //Mark value
          "maintMargin": 0.00252044,              //Position margin
          "realLeverage": 10.06,                   //Leverage of the order
          "unrealisedPnl": -0.00014735,           //Unrealised profit and lost
          "unrealisedRoePcnt": -0.0553,           //Rate of return on investment
          "unrealisedPnlPcnt": -0.0553,            //Position profit and loss ratio
          "delevPercentage": 0.52,             //ADL ranking percentile
          "currentTimestamp": 1558087175068,      //Current timestamp
          "settleCurrency": "XBT"                 //Currency used to clear and settle the trades
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


### Funding Settlement

```json
  //Funding Settlement
  { 
    "userId": "xbc453tg732eba53a88ggyt8c", // Deprecated, will detele later 
    "topic": "/contract/position:XBTUSDM",
    "subject": "position.settlement",
    "data": {
        "fundingTime": 1551770400000,          //Funding time
        "qty": 100,                            //Position size   
        "markPrice": 3610.85,                 //Settlement price
        "fundingRate": -0.002966,             //Funding rate
        "fundingFee": -296,                   //Funding fees
        "ts": 1547697294838004923,             //Current time (nanosecond)
        "settleCurrency": "XBT"                //Currency used to clear and settle the trades
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

# Sign Up for KuCoin
<a href="https://www.kucoin.com">Sign Up for KuCoin</a>
# General
## Introduction
Thank you for using KuCoin Futures API documentation. This documentation provides a detailed explanation to the transaction functions and the usage of the interfaces to get the market data on Kucoin Futures.

The whole documentation is divided into two parts: 1)**REST API** and 2) **Websocket Feed**. 

 -  The REST API part contains three sections: **User (private) , Trade (private)** and **Market Data (public).**
 -  The Websocket Feed part contains two sections: **Public Channels** and **Private Channels**. 


## Upcoming Changes

**To reinforce the security of the API, KuCoin upgraded the API key to version 2.0, the validation logic has also been changed. It is recommended to [create](https://futures.kucoin.com/api) and update your API key to version 2.0. The API key of version 1.0 will be still valid until May 1, 2021. [Check new signing method](#signing-a-message)**

#### 2022.03.16
* Deprecate [GET /api/v1/level2/message/query](#level-2-pulling-messages-deprecated) endpoint
* Added endpoint return value description.
* Add [POST /api/v3/transfer-out](#transfer-funds-to-kucoin-main-account-or-kucoin-trade-account) endpoint
* Add [POST /api/v1/transfer-in](#transfer-funds-to-kucoin-futures-account) endpoint

#### 2022.02.07
* New response field maintainMargin,riskLimitLevel have been added to the [GET /api/v1/position](#get-position-details) endpoint

#### 2021.12.07
* Modify comment in interface ["topic": "/contract/position:XBTUSDM"](#position-change-events).

#### 2021.11.19
* Added interface of risk limit level: [GET /v1/contracts/risk-limit/{symbol}](#obtain-futures-risk-limit-level) , [POST /v1/position/risk-limit-level/change](#adjust-risk-limit-level) <br/>
* Added adjustment result of risk limit level to subject: [position.adjustRiskLimit](#adjustment-result-of-risk-limit-level). in the topic of position: /contract/position:{symbol} 

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
- [Node.js SDK](https://github.com/Kucoin/kucoin-futures-node-sdk)
- [Python SDK](https://github.com/Kucoin/kucoin-futures-python-sdk)
- [Go SDK](https://github.com/Kucoin/kucoin-futures-go-sdk)


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
* `code: 1015`是指从该IP的请求过多，触发了cloudflare限制, 触发后限制时间`30s`。建议不同(子)账号使用不同的IP去请求，避免相互影响。
* `{"code":"200002","msg":"Too many requests in a short period of time, please retry later"}`是指账号特定接口超过请求限制, 触发后限制时间为`10s`。您可以使用多个子账号来避免这个问题。
* `{"code":"429000","msg":"Too Many Requests"}`是指服务器短时间收到过多请求，由于服务器过载保护拒绝了此次请求。您可以直接重试请求。

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


## Endpoint of the Interface

Each interface has its own endpoint, which is provided under the **HTTP REQUEST** module. 

For **GET requests**, please append the queried parameters to the endpoint.

E.G. For "[Position](#get-position-details)", the default endpoint of this API is **/api/v1/position**. If you pass the "symbol" parameter (XBTUSDM), the endpoint will become **/api/v1/position?symbol=XBTUSDM** and the final request URL will be **https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM**.

## Requests
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
1015 | cloudflare frequency limit according to IP, block 30s
40010 | Unavailable to place orders. Your identity information/IP/phone number shows you're at a country/region that is restricted from this service.
100001 | There are invalid parameters
100002 | systemConfigError
100003 | Contract parameter invalid
100004 | Order is in not cancelable state
100005 | contractRiskLimitNotExist
200001 | The query scope for Level 2 cannot exceed xxx
200002 | Too many requests in a short period of time, please retry later--kucoin business layer request frequency limit, block 10s
200002 | The query scope for Level 3 cannot exceed xxx
200003 | The symbol parameter is invalid.
300000 | request parameter illegal
300001 | Active order quantity limit exceeded (limit: xxx, current: xxx)
300002 | Order placement/cancellation suspended, please try again later.
300003 | Balance not enough, please first deposit at least 2 USDT before you start the battle
300004 | Stop order quantity limit exceeded (limit: xxx, current: xxx)
300005 | xxx risk limit exceeded
300006 | The close price shall be greater than the bankruptcy price. Current bankruptcy price: xxx.
300007 | priceWorseThanLiquidationPrice
300008 | Unavailable to place the order, there's no contra order in the market.
300009 | Current position size: 0, unable to close the position.
300010 | Failed to close the position
300011 | Order price cannot be higher than xxx
300012 | Order price cannot be lower than xxx
300013 | Unable to proceed the operation, there's no contra order in order book.
300014 | The position is being liquidated, unable to place/cancel the order. Please try again later.
300015 | The order placing/cancellation is currently not available. The Contract/Funding is under the settlement process. When the process is completed, the function will be restored automatically. Please wait patiently and try again later.
300016 | The leverage cannot be greater than xxx.
300017 | Unavailable to proceed the operation, this position is for Futures Brawl
300018 | clientOid parameter repeated
400001 | Any of KC-API-KEY, KC-API-SIGN, KC-API-TIMESTAMP, KC-API-PASSPHRASE is missing in your request header.
400002 | KC-API-TIMESTAMP Invalid -- Time differs from server time by more than 5 seconds
400003 | KC-API-KEY not exists
400004 | KC-API-PASSPHRASE error
400005 | Signature error -- Please check your signature
400006 | The IP address is not in the API whitelist
400007 | Access Denied -- Your API key does not have sufficient permissions to access the URI
400100 | Parameter Error -- You tried to access the resource with invalid parameters
404000 | URL Not Found -- The requested resource could not be found
411100 | User is frozen -- Please contact us via support center
429000 | Too Many Requests -- Trigger the total traffic limit of this interface of KuCoin server, you can retry the request
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

* **General** - Allows a key general permission. This includes most of the GET endpoints.
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

For the **KC-API-SIGN** of the header:

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

## 枚举定义

* 买卖方向(`side`):
    - `BUY` 买/做多
    - `SELL` 卖/做空
</br></br>
* 订单类型(`orderType`, `type`):
    - `LIMIT` 限价单
    - `MARKET` 市价单
    - `STOP` 限价止损
    - `TAKE_PROFIT` 限价止盈
    - `STOP_MARKET` 市价止损
    - `TAKE_PROFIT_MARKET`  市价止盈
</br></br>
* 下单类型(`placeType`):
    - `DEFAULT`	默认
    - `OTOCO` 订单止盈止损
    - `STEP_REDUCE_POSITION`	阶梯减仓
    - `LIQUIDATION_TAKE_OVER`	强平接管
    - `ADL_TRIGGER`	ADL触发用户
    - `ADL_LUCKY`	ADL幸运用户
</br></br>
* 触发价格方式(`workingType`):
    - `MP`	标记价格
    - `TP`	最新成交价
</br></br>
* 有效方式(`timeInForce`):
    - `GTC`: 用户主动取消才过期
    - `IOC`:立即成交可成交的部分，然后取消剩余部分，不进入买卖盘；
    - `FOK`：如果下单不能全部成交，则取消；
</br></br>
* 资金记录类型(`type`):
    - `CONSUME`-消费,
    - `FUNDING`-资金费用,
    - `FEE`-手续费,
    - `REALIZED_PNL`-已实现盈亏,
    - `TRANSFER_IN`-转入,
    - `TRANSFER_OUT`-转出,
    - `SON_MOTHER_ACCOUNT_TRANSFER`-合约子母账号划转,
    - `TRIAL_RECYCLE`-体验金回收,
    - `REWARD`-发奖,
    - `DEDUCTION_REFUND`-抵扣返还;
</br></br>
* 付款账户类型(`payAccountType`):
    - `MAIN`-储蓄账户
    - `TRADE`-交易账户
</br></br>
* 接收账户类型(`recAccountType`):
    - `MAIN`-储蓄账户
    - `TRADE`-币币账户
</br></br>
* 划转状态(`status`):
    - `APPLY`-待处理
    - `PROCESSING`-处理中
    - `PENDING_APPROVAL`-待审核
    - `APPROVED`-审核通过
    - `REJECTED`-审核拒绝
    - `PENDING_CANCEL`-待退款
    - `CANCEL`-取消
    - `SUCCESS`-成功
</br></br>
* 转出状态(`status`):
    - `PROCESSING`-处理中
    - `SUCCESS`-成功
    - `FAILURE`-失败
</br></br>
* 保证金模式(`marginType`):
    - `ISOLATED`-逐仓
    - `CROSSED`-全仓
</br></br>
* 仓位方向(`side`):
    - `BOTH`-单向持仓
    - `LONG`-做多方向
    - `SHORT`-做空方向
</br></br>
* 平仓类型(`type`):
    - `CLOSE_LONG`-手动平多
    - `CLOSE_SHORT`-手动平多
    - `LIQUID_LONG`-强制平多
    - `LIQUID_SHORT`-强制平空
    - `ADL_LONG`-adl平多
    - `ADL_SHORT`-adl平空
</br></br>
* 合约类型(`type`):
    - `FFWCSX`-永续
    - `FFICSX`-交割
</br></br>
* 合约状态(`status`):
  - `Open`-已上线
  - `PrepareSettled`-准备结算
  - `BeingSettled`-结算中
  - `Paused`-已暂停
  - `CancelOnly`只能撤单

---
# User
Signature is required for this part.

# 用户配置
## 修改用户自动追加保证金状态
```json
{
  "code": "200000",
  "data": true
} 
```
### HTTP Request
`POST /api/v2/user-config/change-auto-deposit`
### API Permission
该接口需要`交易权限`
### Parameters
Param	| Type	| Mandatory	| Description |
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
autoDeposit | Boolean | 是 | 是否开启自动追加保证金（设置为`true`时，达到强平价格时会尝试自动追加保证金）

## 获取用户全局杠杆
```json
{
  "code": "200000",
  "data": {
    "symbol": "ETHUSDTM",
    "leverage": "5",
    "maxRiskLimit": "10000000"
  }
}
```
### HTTP Request
`GET /api/v2/user-config/leverage`
### API Permission
该接口需要`通用权限`
### Parameters
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### RESPONSES
Field | Description
--------- | -------
symbol | 合约symbol | 
leverage | 用户当前杠杆 | 
maxRiskLimit | 该杠杆下持仓最大限额 | 

## 获取用户全局杠杆(所有合约)
```json
{
  "code": "200000",
  "data": [
    {
    "symbol": "ETHUSDTM",
    "leverage": "5",
    "maxRiskLimit": "10000000"
    },
    {
    "symbol": "BTCUSDTM",
    "leverage": "5",
    "maxRiskLimit": "20000000"
    }
  ]
}
```
### HTTP Request
`GET /api/v2/user-config/leverage`
### API Permission
该接口需要`通用权限`
### Parameters
`N/A`
### RESPONSES
Field | Description
--------- | -------
symbol | 合约symbol | 
leverage | 用户当前杠杆 | 
maxRiskLimit | 该杠杆下持仓最大限额 | 

## 修改用户全局杠杆
```json
{
  "code": "200000",
  "data": {
    "symbol": "ETHUSDTM",
    "leverage": "5",
    "maxRiskLimit": "10000000"
  }
}
```
### HTTP Request
`POST /api/v2/user-config/adjust-leverage`
### API Permission
该接口需要`交易权限`
### Parameters
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
leverage | Integer | 是 | 杠杆（正数）|
### RESPONSES
Field | Description
--------- | -------
symbol | 合约symbol | 
leverage | 用户当前杠杆 | 
maxRiskLimit | 该杠杆下持仓最大限额 | 

# 账户
## 获取账户概览
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": [
        {
            "currency": "USDT", //币种
            "walletBalance": "21098384.5602084900", //钱包余额
            "positionMargin": "310.7995729500", //仓位保证金
            "orderMargin": "7177.2558034300", //订单保证金
            "availableBalance": "21090896.5048321100", //可用余额
            "unrealisedPNL": "2.2572000000", //仓位未实现盈亏
            "accountEquity": "21098386.8174084900", //该币种账户总权益 = 钱包余额 + 仓位未实现盈亏
            "holdBalance": "0.0000000000", //划转冻结金额
            "availableTransferBalance": "21090896.5048321100" //可转出金额
        },
        {
            "currency": "ETH",
            "walletBalance": "100.0000000000",
            "positionMargin": "0.0000000000",
            "orderMargin": "0.0000000000",
            "availableBalance": "100.0000000000",
            "unrealisedPNL": "0",
            "accountEquity": "100.0000000000",
            "holdBalance": "0.0000000000",
            "availableTransferBalance": "100.0000000000"
        },
        {
            "currency": "XRP",
            "walletBalance": "10000.0000000000",
            "positionMargin": "0.0000000000",
            "orderMargin": "0.0000000000",
            "availableBalance": "10000.0000000000",
            "unrealisedPNL": "0",
            "accountEquity": "10000.0000000000",
            "holdBalance": "0.0000000000",
            "availableTransferBalance": "10000.0000000000"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/account-overview`
### API Permission
该接口需要`通用权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s`
### PARAMETERS
Param	| Type	| Mandatory	| Description
---|---|---|---
currency|String|NO|币种 BTC、USDT、ETH、XRP、DOT,不传则查询所有币种
### RESPONSES
Field|Description
---|---
currency|币种
walletBalance|钱包余额
positionMargin|仓位保证金
orderMargin|订单保证金
availableBalance|可用余额
unrealisedPNL|未实现盈亏
accountEquity|合约账户总权益。`合约账户总权益` = `钱包余额` + `未实现盈亏`
holdBalance|转出冻结
availableTransferBalance|可转金额

## 查询资金记录
```json
{
    "success": true,
    "code": "200", //200代表成功，其他代表失败
    "msg": "success",
    "retry": false,
    "data": [
        {
            "time": 1650348854000, //业务发生时间
            "currency": "USDT", //币种
            "type": "FEE", //业务类型
            "amount": "-0.00408000", //交易金额
            "walletBalance": "99999.70744444", //钱包余额
            "remark": "ETHUSDTM" //交易说明
        },
        {
            "time": 1650348854000,
            "currency": "USDT",
            "type": "REALIZED_PNL",
            "amount": "0.00408000",
            "walletBalance": "0.55080000",
            "remark": "ETHUSDTM"
        },
        {
            "time": 1650538206000,
            "currency": "USDT",
            "type": "FEE",
            "amount": "-0.54400000",
            "walletBalance": "100000.54400000",
            "remark": "ETHUSDTM"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/transaction-history`
### API Permission
该接口需要`通用权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s`
### PARAMETERS  
Param	| Type	| Mandatory	| Description
---|---|---|---
limit|Integer|NO|记录数，默认`100`，最大`1000`
currency|String|NO|币种
type|String|NO|流水类型
startAt|Long|NO|开始时间
endAt|Long|NO|结束时间  

<aside class="notice">
如果未传<code>startAt</code>,默认返回最近7天数据。
</aside>

* 流水类型(`type`)说明：
    * `CONSUME`-消费,
    * `FUNDING`-资金费用,
    * `FEE`-手续费,
    * `REALIZED_PNL`-已实现盈亏,
    * `TRANSFER_IN`-转入,
    * `TRANSFER_OUT`-转出,
    * `SON_MOTHER_ACCOUNT_TRANSFER`-合约子母账号划转,
    * `TRIAL_RECYCLE`-体验金回收,
    * `REWARD`-发奖,  
    * `DEDUCTION_REFUND`-抵扣返还,
    * `INSURANCE_CROSS_POSITION_PAY`-保险基金穿仓支付;



### RESPONSES  
Field|Description
---|---
time|业务发生时间
currency|币种
type|类型
amount|交易金额
walletBalance|钱包余额
remark|说明

# 划转
## 转出到KuCoin储蓄/币币账户
``` json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "applyId": "30626177500585984", //转出申请id，可以使用该id撤回申请
        "bizNo": "30626172928794624", //业务唯一编码
        "payAccountType": "CONTRACT", //付款账户类型
        "payTag": "DEFAULT", //付款账户子类型
        "remark": "", //用户备注
        "recAccountType": "MAIN", //收款账户类型
        "recTag": "DEFAULT", //收款账户子类型
        "recRemark": "", //收款账户流水备注
        "recSystem": "KUCOIN", //收款服务方
        "status": "APPLY", //状态
        "currency": "USDT", //币种
        "amount": "22.0000000000", //划转金额
        "fee": "0.0000000000", //划转手续费
        "sn": 30626177504780288, //唯一序列号
        "reason": "", //失败原因
        "createdAt": 1650773821000, //创建时间
        "updatedAt": 1650773822000 //更新时间
    }
}
```
### HTTP Request
`POST /api/v2/transfer-out`
### API Permission
该接口需要`交易权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s `
### PARAMETERS  
Param	| Type	| Mandatory	| Description
---|---|---|---
amount|Number|YES|划转金额
currency|String|YES|币种，如USDT、BTC
recAccountType|String|YES|接收账户类型，只能是`MAIN`-储蓄账户、`TRADE`-币币账户
### RESPONSES  
Field|Description
---|---
applyId|转出申请id，可以使用该id撤回申请
bizNo|业务主键id
payAccountType|付款账户类型
payTag|付款账户子类型
remark|用户备注
recAccountType|收款账户类型
recTag|收款账户子类型
recRemark|收款账户流水备注
recSystem|收款服务方
status|状态:`APPLY`-待处理,`PROCESSING`-处理中,`PENDING_APPROVAL`-待审核,`APPROVED`-审核通过,`REJECTED`-审核拒绝,`PENDING_CANCEL`-待退款,`CANCEL`-取消,`SUCCESS`-成功
currency|币种
amount|划转金额
fee|划转手续费
sn|序列号
reason|失败原因
createdAt|创建时间
updatedAt|更新时间



## 查询转出申请记录
``` json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": [
        {
            "applyId": "30626177500585984", //申请id
            "currency": "USDT", //币种
            "recRemark": "", //收款账户流水备注
            "recSystem": "KUCOIN", //收款服务方
            "status": "PROCESSING", //状态
            "amount": "22.0000000000", //划转金额
            "reason": "", //失败原因
            "sn": 30626177504780288, //唯一序列号
            "createdAt": 1650773821000, //业务发生时间
            "remark": "" //用户备注
        },
        {
            "applyId": "8djl4jt07pc",
            "currency": "USDT",
            "recRemark": "",
            "recSystem": "KUCOIN",
            "status": "PROCESSING",
            "amount": "22.0000000000",
            "reason": "",
            "sn": 30624803597586433,
            "createdAt": 1650773493000,
            "remark": ""
        }
    ]
}
```
### HTTP Request
`GET /api/v2/transfer-list`
### API Permission
该接口需要`通用权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s`
### PARAMETERS  
Param	| Type	| Mandatory	| Description
---|---|---|---
startAt|Long|NO|业务发生起始时间
endAt|Long|NO|业务发生截止时间
limit|Integer|NO|记录数，默认`100`，最大`1000`
currency|String|NO|币种
status|String|NO|状态，`PROCESSING`-处理中，`SUCCESS`-成功, `FAILURE`-失败

<aside class="notice">
如果不传<code>startAt</code>和<code>endAt</code>,默认返回最近7天数据。
</aside>

### RESPONSES
Field|Description
---|---
applyId|转出申请Id
currency|币种
recRemark|收款账户流水备注
recSystem|收款服务方
status|状态
amount|转出金额
reason|失败原因
sn|序列号
createdAt|时间
remark|付款账户备注


## 取消转出
```json
{
    "success": true,
    "code": "200", //200代表成功，其他代表失败
    "msg": "success",
    "retry": false,
    "data": null
}
```
### HTTP Request
`DELETE /api/v2/cancel/transfer-out`
### API Permission
该接口需要`通用权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s`
### PARAMETERS  
Param	| Type	| Mandatory	| Description
---|---|---|---
applyId|String|YES|转出申请id
### RESPONSES
当`code`为`200`代表成功，其他代表失败

## 资金转入合约账户
``` json
{
    "code": "200", //code为200代表转入成功，否则代表失败
    "msg": "",
    "retry": true,
    "success": true
}
```
### HTTP Request
`POST /api/v2/transfer-in`
### API Permission
该接口需要`交易权限`
### REQUEST RATE LIMIT
此接口针对每个账号请求频率限制为`30次/3s`
### PARAMETERS  
Param	| Type	| Mandatory	| Description
---|---|---|---
amount|Number|YES|交易金额
currency|String|YES|币种
payAccountType|String|YES|付款账户类型：只能是`MAIN`-储蓄账户，`TRADE`-交易账户
### RESPONSES
当`code`为`200`代表成功，其他代表失败



---
# 交易
此部分需进行签名验证。

# 订单

## 下单
```json
//response
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "orderId": "25880469442662400"
    }
}
```
### HTTP Request
`POST /api/v2/order`
### API Permission
该接口需要`交易权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------
 symbol       | STRING   | YES      | 交易对                                                       
 side         | ENUM     | YES      | 买卖方向 `SELL`, `BUY`                                           
 price        | DECIMAL  | NO       | 价格                                                         
 type         | ENUM     | YES      | 订单类型 `LIMIT`, `MARKET`, `STOP`, `TAKE_PROFIT`, `STOP_MARKET`, `TAKE_PROFIT_MARKET`
 size         | INT      | NO       | 下单数量,使用`closeOrder`不支持此参数                          
 clientOid    | STRING   | NO       | 唯一的订单ID，可用于识别订单。如：UUID,只能包含数字、字母、下划线（_）或 分隔线（-） 
 reduceOnly   | BOOLEAN  | NO       | [可选] 只减仓标记, 默认值是 `false` 。值为`true`时，需要设置平仓数量 
 closeOrder   | BOOLEAN  | NO       | [可选] 平仓单标记, 默认值是 `false` 。值为`true`时，代表仓位全平 
 hidden       | BOOLEAN  | NO       | [可选] 订单不会在买卖盘中展示。选择`hidden`，不允许选择`postOnly`。 
 visibleSize  | INT      | NO       | [可选] 隐藏单最大可展示的数量。                              
 stopPrice    | DECIMAL  | NO       | 触发价, 仅 `STOP`, `STOP_MARKET`, `TAKE_PROFIT`, `TAKE_PROFIT_MARKET` 需要此参数 
 postOnly     | BOOLEAN  | NO       | [可选] 只挂单的标识。选择`postOnly`，不允许选择`hidden`。当订单时效为`IOC`策略时，该参数无效。 
 workingType  | STRING   | NO       | [可选] 止损单触发价类型，包括`TP`和`MP`                          
 timeInForce  | STRING   | NO       | 有效方法                                                     
 clientTimestamp| TIMESTAMP | NO | [可选]客户端下单时间
 allowMaxTimeWindow| INT | NO | [可选]允许服务器接收到请求时间与`clientTimestamp`最大时间间隔

根据下单`type`的不同， 对应需要的参数如下：

| Type                               | 参数                          |
| ---------------------------------- | ---------------------------- |
| `LIMIT`                            | `size`, `price`              |
| `MARKET`                           | `size`                       |
| `STOP`,`TAKE_PROFIT`               | `size`, `price`, `stopPrice `|
| `STOP_MARKET`,`TAKE_PROFIT_MARKET` | `size`, `stopPrice`          |

* 条件单触发说明：
    * `STOP`, `STOP_MARKET` 止损单：
        * 买入: 最新合约价格/标记价格高于等于触发价`stopPrice`
        * 卖出: 最新合约价格/标记价格低于等于触发价`stopPrice`
    * `TAKE_PROFIT`, `TAKE_PROFIT_MARKET` 止盈单：
        * 买入: 最新合约价格/标记价格低于等于触发价`stopPrice`
        * 卖出: 最新合约价格/标记价格高于等于触发价`stopPrice`
<br/><br/>
* `clientTimestamp`和`allowMaxTimeWindow`参数说明：当这两个参数`同时`传递才会生效
      * 验证逻辑：
          * 1. 服务器接收到请求时间: `receiveTime`
          * 2. 服务器接收到请求时间(`receiveTime`)与客户端下单时间(`clientTimestamp`)间隔:`receiveTime` - `clientTimestamp` = `timeWindow``
          * 3. 当`clientTimestamp` <= `receiveTime`同时`timeWindow` <= `allowMaxTimeWindow`时，下单请求才会被接受,否则下单失败返回错误码`300019`
<br/><br/>

### RESPONSES
Field | Description
--------- | -----------
orderId | 订单id 

## 单个撤单
```json
{
	"success": true,
	"code": "200",
	"msg": "success",
	"retry": false,
	"data": null
}
```

### HTTP Request
`DELETE /api/v2/order`
### API Permission
该接口需要`交易权限`
### PARAMETERS

Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------
symbol | STRING | YES | 合约symbol
orderId | STRING | NO | 订单id 
clientOid | STRING | NO | 用户自定义orderId 

<aside class="notice">
<code>orderId</code>和<code>clientOid</code>二选一，如果都传，默认使用<code>orderId</code>.
</aside>



## 批量撤单
```json
{
	"success": true,
	"code": "200",
	"msg": "success",
	"retry": false,
	"data": {
		"orderIds": [
			"24521602255290368",
			"24522071262363648",
			"24522186836410368"
		]
	}
}
```
### HTTP Request
`DELETE /api/v2/orders`
### API Permission
该接口需要`交易权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------
symbol | STRING | YES | 合约symbol
### RESPONSES
Field | Description
--------- | -----------
orderIds | 成功撤掉的订单id 


## 查询成交记录
```json
{
      "success": true,
      "code": "200",
      "msg": "success",
      "retry": false,
      "data": [
          {
              "id": "24074404942053376",
              "orderId": "23518930911887360",
              "symbol": "ETHUSDTM",
              "time": "1649211785419",
              "side": "BUY",
              "size": 1,
              "price": "3895.0",
              "fee": "0.00779",
              "feeRate": "0.00020",
              "placeType": "DEFAULT",
              "orderType": "LIMIT",
              "feeCurrency": "USDT",
              "pnl": "0.0",
              "pnlCurrency": "USDT",
              "value": "38.95",
              "maker": true,
              "forceTaker": "false"
          }
      ]
  }
```
### HTTP Request
`GET /api/v2/orders/historical-trades`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------
| symbol | String | YES | 合约symbol	|
| startAt | Long  | NO | 开始时间（毫秒）	|
| endAt | Long | NO | 截止时间（毫秒）	|
| limit | Integer | NO | 默认 `50`; 最大 `1000`.	|
| fromId | Long | NO |从哪一条成交id开始返回. 缺省返回最近的成交记录	|
### RESPONSES
Field | Description
| ------ | ------ |
| id | 交易记录ID |
| orderId | 订单ID |
| symbol | 合约交易对名字 |
| time | 撮合时间 |
| side | 方向：`BUY`:买/做多 `SELL`:卖/做空 |
| size | 数量 |
| price | 价格 |
| fee | 手续费 |
| feeRate | 手续费率 |
| feeCurrency | 手续费币种 |
| pnl | 盈亏 |
| pnlCurrency | 盈亏币种 |
| value | 价值 |
| orderType | 订单类型 |
| placeType | 下单类型：`DEFAULT`、`OTOCO`、`STEP_REDUCE_POSITION`、`LIQUIDATION_TAKE_OVER`、`ADL_TRIGGER`、`ADL_LUCKY` |
| maker | 是否maker |
| forceTaker | 是否强制作为taker处理|


## 查询单个订单详情
### HTTP Request
`GET /api/v2/order/detail`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Description
| :------- | -------- | -- |
| orderId | String |	 订单ID |
| clientOid | String | 用户自定义orderId |
### RESPONSES
Field | Description
| ------ | ------ |
| id | 订单ID |
| symbol | 合约名称 |
| type | 订单类型 |
| side | 方向：BUY:买/做多 SELL:卖/做空 |
| price | 订单价格 |
| size | 数量 |
| dealSize | 订单成交数量 |
| dealValue | 成交价值 |
| workingType | 触发价格方式：MP（标记价格）、TP（最新成交价） |
| stopPrice | 触发价格，订单止盈止损 止损价格 |
| timeInForce | GTC: 用户主动取消才过期, IOC:立即成交可成交的部分，然后取消剩余部分，不进入买卖盘； FOK：如果下单不能全部成交，则取消； |
| postOnly | 是否只做maker |
| hidden | 是否是隐藏单 |
| leverage | 杠杆 |
| closeOrder | 是否平仓单 |
| visibleSize | 隐藏单对应的访问数量 |
| remark | 订单说明 |
| orderTime | 下单时间 |
| reduceOnly | 是否只减仓 |
| status | 订单状态 |
| placeType | 下单类型：`DEFAULT`、`OTOCO`、`STEP_REDUCE_POSITION`、`LIQUIDATION_TAKE_OVER`、`ADL_TRIGGER、ADL_LUCKY` |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |



## 查询活跃订单
### HTTP Request
`GET /api/v2/orders/active`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description 
--------- | ------- | -----------| -----------
| symbol | String | YES | 合约symbol	|

### RESPONSES
Field | Description
| ------ | ------ |
| id | 订单ID |
| symbol | 合约名称 |
| type | 订单类型 |
| side | 方向：`BUY`:买/做多 `SELL`:卖/做空 |
| price | 订单价格 |
| size | 数量 |
| dealSize | 订单成交数量 |
| dealValue | 成交价值 |
| workingType | 触发价格方式：`MP`（标记价格）、`TP`（最新成交价） |
| stopPrice | 触发价格，订单止盈止损 止损价格 |
| timeInForce | `GTC`: 用户主动取消才过期, `IOC`:立即成交可成交的部分，然后取消剩余部分，不进入买卖盘； `FOK`：如果下单不能全部成交，则取消； |
| postOnly | 是否只做maker |
| hidden | 是否是隐藏单 |
| leverage | 杠杆 |
| closeOrder | 是否平仓单 |
| visibleSize | 隐藏单对应的访问数量 |
| remark | 订单说明 |
| orderTime | 下单时间 |
| reduceOnly | 是否只减仓 |
| status | 订单状态 |
| placeType | 下单类型：`DEFAULT`、`OTOCO`、`STEP_REDUCE_POSITION`、`LIQUIDATION_TAKE_OVER`、`ADL_TRIGGER`、`ADL_LUCKY` |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |



## 查询全部活跃订单
### HTTP Request
`GET /api/v2/orders/all-active`
### API Permission
该接口需要`通用权限`
### PARAMETERS
`N/A`
### RESPONSES
Field | Description
| ------ | ------ |
| id | 订单ID |
| symbol | 合约名称 |
| type | 订单类型 |
| side | 方向：BUY:买/做多 SELL:卖/做空 |
| price | 订单价格 |
| size | 数量 |
| dealSize | 订单成交数量 |
| dealValue | 成交价值 |
| workingType | 触发价格方式：`MP`（标记价格）、`TP`（最新成交价） |
| stopPrice | 触发价格，订单止盈止损 止损价格 |
| timeInForce | `GTC`: 用户主动取消才过期, `IOC`:立即成交可成交的部分，然后取消剩余部分，不进入买卖盘； `FOK`：如果下单不能全部成交，则取消； |
| postOnly | 是否只做maker |
| hidden | 是否是隐藏单 |
| leverage | 杠杆 |
| closeOrder | 是否平仓单 |
| visibleSize | 隐藏单对应的访问数量 |
| remark | 订单说明 |
| orderTime | 下单时间 |
| reduceOnly | 是否只减仓 |
| status | 订单状态 |
| placeType | 下单类型：`DEFAULT`、`OTOCO`、`STEP_REDUCE_POSITION`、`LIQUIDATION_TAKE_OVER`、`ADL_TRIGGER`、`ADL_LUCKY` |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |


## 查询历史订单
```json
{
      "success": true,
      "code": "200",
      "msg": "success",
      "retry": false,
      "data": [
          {
              "id": "23528093465444352",
              "symbol": "ETHUSDTM",
              "type": "LIMIT",
              "side": "SELL",
              "price": "3491.0",
              "size": 1,
              "dealSize": 1,
              "dealValue": "34.95",
              "workingType": "",
              "stopPrice": null,
              "timeInForce": "GTC",
              "postOnly": false,
              "hidden": null,
              "leverage": 5,
              "closeOrder": false,
              "visibleSize": 0,
              "remark": null,
              "orderTime": "61567287853208",
              "reduceOnly": false,
              "status": "FINISHED",
              "placeType": "DEFAULT",
              "takeProfitPrice": null,
              "cancelSize": 0,
              "clientOid": "",
              "avgPrice": "3491.0"
          }
      ]
  }
```
### HTTP Request
`GET /api/v2/orders/history`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Description
| :------- | -------- | ---------- |
| symbol | String | 合约symbol	|
| startAt | Long  | 开始时间（毫秒）	|
| endAt | Long  | 截止时间（毫秒）	|
| fromId | Long | [可选] 从哪一条成交id开始返回.	|
| limit | Integer | 默认 `50`; 最大 `1000`.	|
### RESPONSES
Field | Description
| ------ | ------ |
| id | 订单ID |
| symbol | 合约名称 |
| type | 订单类型 |
| side | 方向：`BUY`:买/做多 `SELL`:卖/做空 |
| price | 订单价格 |
| size | 数量 |
| dealSize | 订单成交数量 |
| dealValue | 成交价值 |
| workingType | 触发价格方式：`MP`（标记价格）、`TP`（最新成交价） |
| stopPrice | 触发价格，订单止盈止损 止损价格 |
| timeInForce | `GTC`: 用户主动取消才过期, `IOC`:立即成交可成交的部分，然后取消剩余部分，不进入买卖盘； `FOK`：如果下单不能全部成交，则取消； |
| postOnly | 是否只做maker |
| hidden | 是否是隐藏单 |
| leverage | 杠杆 |
| closeOrder | 是否平仓单 |
| visibleSize | 隐藏单对应的访问数量 |
| remark | 订单说明 |
| orderTime | 下单时间 |
| reduceOnly | 是否只减仓 |
| status | 订单状态 |
| placeType | 下单类型：`DEFAULT`、`OTOCO`、`STEP_REDUCE_POSITION`、`LIQUIDATION_TAKE_OVER`、`ADL_TRIGGER`、`ADL_LUCKY` |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |

# 仓位

## 获取某个合约的仓位
```json
{
    "code": "200000",
    "data": [
        {
            "symbol": "BTCUSDM",
            "qty": 7,
            "leverage": 5,
            "marginType": "ISOLATED",
            "side": "BOTH",
            "autoDeposit": false,
            "entryPrice": "45494.04",
            "entryValue": "-0.0001538663",
            "margin": "0.0000306810",
            "totalMargin": "0.00002603090000000000",
            "liquidationPrice": "38771.12",
            "unrealisedPnl": "-0.00000465010000000000",
            "markPrice": "44159.35",
            "riskRate": "0.1537",
            "maintenanceMarginRate": "0.0250000000",
            "maintenanceMargin": "0.000004",
            "adlPercentile": "1"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/symbol-position`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol | 
qty | 仓位数量（负数表示做空，正数表示做多） | 
leverage | 全局杠杆 | 
marginType | 保证金模式（`ISOLATED`-逐仓，`CROSSED`-全仓） | 
side | 仓位方向（`BOTH`，`LONG`，`SHORT`），当持仓模式为单向仓位时（目前只支持单向持仓），该合约只能有一个仓位，`side=BOTH`。当持仓模式为双向仓位时，该合约可以有两个反向的仓位,`side=LONG或SHORT` | 
autoDeposit | 是否自动追加保证金（为`true`时，强平触发自动追加保证金） | 
entryPrice | 平均开仓价格 | 
entryValue | 开仓价值 | 
margin | 仓位保证金 | 
totalMargin | 仓位总保证金（包含了未实现盈亏） | 
liquidationPrice | 强平价格 | 
unrealisedPnl | 未实现盈亏 | 
riskRate | 仓位风险率（`<1`，数值越大，风险越高） | 
maintenanceMarginRate | 仓位维持保证金率 | 
maintenanceMargin | 维持保证金 | 
adlPercentile | adl排名信息 | 

## 获取所有合约的仓位
```json
{
    "code": "200000",
    "data": [
        {
            "symbol": "BTCUSDM",
            "qty": 7,
            "leverage": 5,
            "marginType": "ISOLATED",
            "side": "BOTH",
            "autoDeposit": false,
            "entryPrice": "45494.04",
            "entryValue": "-0.0001538663",
            "margin": "0.0000306810",
            "totalMargin": "0.00002603090000000000",
            "liquidationPrice": "38771.12",
            "unrealisedPnl": "-0.00000465010000000000",
            "markPrice": "44159.35",
            "riskRate": "0.1537",
            "maintenanceMarginRate": "0.0250000000",
            "maintenanceMargin": "0.000004",
            "adlPercentile": "1"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/all-position`
### API Permission
该接口需要`通用权限`
### PARAMETERS
`N/A`
### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol | 
qty | 仓位数量（负数表示做空，正数表示做多） | 
leverage | 全局杠杆 | 
marginType | 保证金模式（`ISOLATED`-逐仓，`CROSSED`-全仓） | 
side | 仓位方向（`BOTH`，`LONG`，`SHORT`），当持仓模式为单向仓位时（目前只支持单向持仓），该合约只能有一个仓位，`side=BOTH`。当持仓模式为双向仓位时，该合约可以有两个反向的仓位,`side=LONG或SHORT` | 
autoDeposit | 是否自动追加保证金（为`true`时，强平触发自动追加保证金） | 
entryPrice | 平均开仓价格 | 
entryValue | 开仓价值 | 
margin | 仓位保证金 | 
totalMargin | 仓位总保证金（包含了未实现盈亏） | 
liquidationPrice | 强平价格 | 
unrealisedPnl | 未实现盈亏 | 
riskRate | 仓位风险率（`<1`，数值越大，风险越高） | 
maintenanceMarginRate | 仓位维持保证金率 | 
maintenanceMargin | 维持保证金 | 
adlPercentile | adl排名信息 | 

## 增加仓位保证金
```json
{
    "code": "200000",
    "data": true
}
```
### HTTP Request
`POST /api/v2/change-margin`
### API Permission
该接口需要`交易权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
positionSide | String | 是 | 目前只支持`BOTH`
amount | BigDecimal | 是 | 增减保证金数量，负数提取保证金，正数为增加仓位保证金
### RESPONSES
Field | Description
--------- | -----------|
data | `true` 表示增减保证金成功 | 

## 仓位盈亏历史
```json
{
    "code": "200000",
    "data": [
        {
            "symbol": "LINKUSDTM",
            "settleCurrency": "USDT",
            "type": "CLOSE_LONG",
            "leverage": 5,
            "pnl": "-0.006",
            "roe": "-0.0098",
            "openTime": 1649299430000,
            "closeTime": 1649299436000,
            "openPrice": "15.37",
            "closePrice": "15.34"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/close-pnl-his`
### API Permission
该接口需要`通用权限`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 否 | 合约symbol
type | String | 否 | 平仓类型
settleCurrency | String | 否 | 合约结算币种
startAt | String | 否 | 开始时间（时间跨度最多`3`个月）
endAt | String | 否 | 截止时间（时间跨度最多`3`个月）
limit | int | 是 | 记录数（默认`50`，最大`1000`）

* `type`平仓类型说明：
    * `CLOSE_LONG`（手动平多）
    * `CLOSE_SHORT`（手动平空）
    * `LIQUID_LONG`（强制平多）
    * `LIQUID_SHORT`（强制平空）
    * `ADL_LONG`（adl平多）
    * `ADL_SHORT`（adl平空）


### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol| 
settleCurrency | 合约结算币种 | 
type | 平仓类型 | 
leverage | 平仓时的全局杠杆 | 
pnl | 平仓盈亏 | 
roe | 收益率 | 
openTime | 开仓时间 | 
closeTime |平仓时间  | 
openPrice | 开仓价格 | 
closePrice | 平仓价格 | 

---
# 市场数据
市场数据是公共的，不需要验证签名。

# 合约

## 获取所有开放合约信息
```json
{
    "code": "200000",
    "data": [
        {
            "symbol": "BTCUSDTM",
            "type": "FFWCSX",
            "firstOpenDate": 1585555200000,
            "expireDate": null,
            "settleDate": null,
            "baseCurrency": "BTC",
            "quoteCurrency": "USDT",
            "settleCurrency": "USDT",
            "maxOrderQty": 1000000,
            "maxPrice": 1000000,
            "lotSize": 1,
            "tickSize": 1,
            "indexPriceTickSize": 0.01,
            "multiplier": 0.001,
            "makerFeeRate": 0.0002,
            "takerFeeRate": 0.0006,
            "settlementFeeRate": 0.0006,
            "isInverse": false,
            "fundingBaseSymbol": ".BTCINT8H",
            "fundingQuoteSymbol": ".USDTINT8H",
            "fundingRateSymbol": ".BTCUSDTMFPI8H",
            "indexSymbol": ".KBTCUSDT",
            "settlementSymbol": "",
            "status": "Open"
        }
    ]
}
```
### HTTP Request
`GET /api/v2/contracts/active`
### PARAMETERS
`N/A`
### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol | 
type | 合约类型：`FFWCSX`(永续)，`FFICSX`(交割） | 
firstOpenDate | 首次开放时间 | 
expireDate | 合约到期时间（永续合约用不过期） | 
settleDate | 交割合约交割时间 | 
baseCurrency | 基础货币 | 
quoteCurrency | 计价货币 | 
settleCurrency | 结算货币 | 
maxOrderQty | 最大委托数量 | 
maxPrice | 最大委托价格 | 
lotSize | 最小合约数量 | 
tickSize | 最小的价格变化 | 
indexPriceTickSize | 指数价格变化步长 | 
multiplier | 合约乘数 | 
makerFeeRate | maker手续费率 | 
takerFeeRate | taker手续费率 | 
settlementFeeRate | 交割合约交割时手续费率 | 
isInverse | 是否是币本位合约 | 
fundingBaseSymbol | 资金费用基础货币symbol | 
fundingQuoteSymbol | 资金费用计价货币symbol | 
fundingRateSymbol | 资金费率symbol | 
indexSymbol | 指数symbol | 
settlementSymbol | 结算symbol | 
status | 状态：`Open`(已上线)`、PrepareSettled`(准备结算)、`BeingSettled`（结算中）、`Paused`(已暂停)、`CancelOnly`(只能撤单) |  


## 获取某个合约
```json
{
    "code": "200000",
    "data": {
        "symbol": "BTCUSDTM",
        "type": "FFWCSX",
        "firstOpenDate": 1585555200000,
        "expireDate": null,
        "settleDate": null,
        "baseCurrency": "BTC",
        "quoteCurrency": "USDT",
        "settleCurrency": "USDT",
        "maxOrderQty": 1000000,
        "maxPrice": 1000000,
        "lotSize": 1,
        "tickSize": 1,
        "indexPriceTickSize": 0.01,
        "multiplier": 0.001,
        "makerFeeRate": 0.0002,
        "takerFeeRate": 0.0006,
        "settlementFeeRate": 0.0006,
        "isInverse": false,
        "fundingBaseSymbol": ".BTCINT8H",
        "fundingQuoteSymbol": ".USDTINT8H",
        "fundingRateSymbol": ".BTCUSDTMFPI8H",
        "indexSymbol": ".KBTCUSDT",
        "settlementSymbol": "",
        "status": "Open"
    }
}
```
### HTTP Request
`GET /api/v1/contracts/{symbol}`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol | 
type | 合约类型：`FFWCSX`(永续)，`FFICSX`(交割） | 
firstOpenDate | 首次开放时间 | 
expireDate | 合约到期时间（永续合约用不过期） | 
settleDate | 交割合约交割时间 | 
baseCurrency | 基础货币 | 
quoteCurrency | 计价货币 | 
settleCurrency | 结算货币 | 
maxOrderQty | 最大委托数量 | 
maxPrice | 最大委托价格 | 
lotSize | 最小合约数量 | 
tickSize | 最小的价格变化 | 
indexPriceTickSize | 指数价格变化步长 | 
multiplier | 合约乘数 | 
makerFeeRate | maker手续费率 | 
takerFeeRate | taker手续费率 | 
settlementFeeRate | 交割合约交割时手续费率 | 
isInverse | 是否是币本位合约 | 
fundingBaseSymbol | 资金费用基础货币symbol | 
fundingQuoteSymbol | 资金费用计价货币symbol | 
fundingRateSymbol | 资金费率symbol | 
indexSymbol | 指数symbol | 
settlementSymbol | 结算symbol | 
status | 状态：`Open`(已上线)`、PrepareSettled`(准备结算)、`BeingSettled`（结算中）、`Paused`(已暂停)、`CancelOnly`(只能撤单) | 

## 获取合约的风险限额列表
```json
{
    "code": "200000",
    "data": [
        {
            "symbol": "ADAUSDTM",
            "level": 1,
            "maxRiskLimit": 500,
            "minRiskLimit": 0,
            "maxLeverage": 20,
            "initialMarginRate": 0.05,
            "maintenanceMarginRate": 0.025
        },
        {
            "symbol": "ADAUSDTM",
            "level": 2,
            "maxRiskLimit": 1000,
            "minRiskLimit": 500,
            "maxLeverage": 2,
            "initialMarginRate": 0.5,
            "maintenanceMarginRate": 0.25
        }
    ]
}
```
### HTTP Request
`GET /api/v2/contracts/risk-limit/{symbol}`
### PARAMETERS
Param	| Type	| Mandatory	| Description
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### RESPONSES
Field | Description
--------- | -----------|
symbol | 合约symbol | 
level | 限额等级 | 
maxRiskLimit | 该等级最大风险限额 | 
minRiskLimit | 该等级最小风险限额 | 
maxLeverage | 该等级最大可用杠杆 | 
initialMarginRate | 该等级初始保证金率 | 
maintenanceMarginRate | 仓位价值处于该等级限额时，用到的维持保证金率 | 


## 查询资金费用结算历史
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "dataList": [
            {
                "id": "2783173922521088",
                "uid": "4456",
                "symbol": "SHIBUSDTM",
                "timePoint": "1644910900000",
                "fundingRate": "0.0010000000",
                "markPrice": "0.0000305200",
                "qty": "-1",
                "entryValue": "-3.0520000000",
                "funding": "0.0030520000",
                "settleCurrency": "USDT"
            }
        ],
        "hasMore": true
    }
}
```
### HTTP Request
`GET /api/v2/funding-history`
### PARAMETERS
| Param    | Type | Description |
| :------ | -------- | ------------------------------ |
| symbol  | String   | 合约symbol                                            |
| startAt | Long     | [可选] 开始时间（毫秒）                               |
| endAt   | Long     | [可选] 截止时间（毫秒）                               |
| limit   | Integer  | 默认 `50`; 最大 `1000`.                             |
| fromId  | Long     | [可选] 从哪一条成交`id`开始返回. 缺省返回最近的成交记录 |
### RESPONSES
Field | Description
| ------ | ------ |
| id | id |
| uid | 用户uid |
| symbol | 合约symbol |
| timePoint | 时间点(毫秒) |
| fundingRate | 资金费率 |
| markPrice | 标记价格 |
| qty | 结算时的仓位数 |
| entryValue | 结算时的仓位价值 |
| funding | 结算的资金费用，正数表示收入；负数表示支出 |
| settleCurrency | 结算币种 |
| hasMore | 是否还有下一页 |

## 获取合约K线数据
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": [
        [
            "1649642340000", //时间
            15.748, //开盘价
            15.748, //最高价
            15.736, //最低价
            15.736, //收盘价
            656 //成交量
        ]
    ]
}
```
### HTTP Request
`GET /api/v2/kline/query`
### PARAMETERS
| Param    | Type | Description |
| :------- | -------- | -----|
| from   |  Long  | 起始时间（毫秒） |
| to   | Long | 结束时间（毫秒） |
| granularity   | Integer   | 代表分钟数，可选范围：`1`,`5`,`15`,`30`,`60`,`120`,`240`,`480`,`720`,`1440`,`10080` |
| symbol   | String   | 合约symbol |

## 查询资金费率列表
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "dataList": [
            {
                "symbol": "SHIBUSDTM",
                "granularity": "28800000",
                "timePoint": "1649160000000",
                "value": "0.000100"
            }
        ],
        "hasMore": false
    }
}
```
### HTTP Request
`GET /api/v2/contract/{symbol}/funding-rates`
### PARAMETERS
| Param    | Type | Description |
| :------- | -------- | --------- |
| symbol   | String   | 合约symbol |
| startAt | Long   | 开始时间 |
| endAt | Long   | 结束时间 |
| fromId | Long | [可选] 从哪一条成交`id`开始返回.	|
| limit | Integer | 默认 `50`; 最大 `1000`.	|
### RESPONSES
Field | Description
| ------ | ------ |
| symbol | 合约symbol |
| granularity | 时间粒度 |
| timePoint | 时间点(毫秒) |
| value | 资金费率 |


# 委托买卖盘

## 获取买卖盘
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "contractId": 206,
        "symbol": "LINKUSDTM",
        "ts": 1649746178803,
        "sequence": 102,
        "asks": [
            ["13.965",202],
            ["13.970",101]
        ],
        "bids": [
            ["13.942",164],
            ["13.937",164],
            ["13.928",178]
        ]
    }
}
```
返回买卖盘数据
### HTTP Request
`GET /api/v2/order-book`
### PARAMETERS
Param	| Type	| Mandatory	| Description
| ------ | ------ | -------- | ----------------------------------- |
| symbol | 字符串 | Y        | 合约名称                            |
| limit  | 数字   | N        | 买卖盘档数，默认`500`，取值范围`1`~`1000` |
### RESPONSES
Field | Description
| ---------- | -------- |
| contractId | 合约ID   |
| symbol     | 合约名称 |
| ts         | 时间戳   |
| sequence   | 快照序号 |
| asks       | 卖盘     |
| bids       | 买盘     |

## 最优挂单
```json
{
  "success": true,
  "code": "200",
  "msg": "success",
  "retry": false,
  "data": {
    "symbol": "LINKUSDTM",
    "askPrice": "13.965",
    "askSize": 202,
    "bidPrice": "13.942",
    "bidSize": 164
  }
}
```
返回买卖盘最佳买一卖一价
### HTTP Request
`GET /api/v2/ticker/bookTicker`
### PARAMETERS
Param	| Type	| Mandatory	| Description
| ------ | ------ | -------- | -------- |
| symbol | 字符串 | Y        | 合约名称 |
### RESPONSES
Field | Description
| -------- | ---------- |
| symbol   | 合约名称   |
| askPrice | 最佳卖一价 |
| askSize  | 卖一数量   |
| bidPrice | 最佳买一价 |
| bidSize  | 买一数量   |


# 行情快照

## 获取最新成交价
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": {
        "symbol": "LINKUSDTM",
        "ts": 1649744712820,
        "price": "13.774",
    }
}
```
返回最新成交价
### HTTP Request
`GET /api/v2/ticker/price`
### PARAMETERS
Param	| Type	| Mandatory	| Description
| ------ | ------ | -------- | -------- |
| symbol | 字符串 | Y        | 合约名称 |
### RESPONSES
Field | Description
| ------ | ---------- |
| symbol | 合约名称   |
| ts     | 成交时间戳 |
| price  | 成交价格   |




## 获取最近成记录
```json
{
    "success": true,
    "code": "200",
    "msg": "success",
    "retry": false,
    "data": [
      {
        "symbol": "ETHUSDTM",
        "matchSide": "buy",
        "price": "2929.90",
        "size": 10,
        "ts": 1649744581941
      }
    ]
  }
```
返回最近成交记录
### HTTP Request
`GET /api/v2/trades`
### PARAMETERS
Param	| Type	| Mandatory	| Description
| ------ | ------ | -------- | --------------------------------- |
| symbol | 字符串 | Y        | 合约名称                            |
| limit  | 数字   | N        | 返回记录条数，默认`20`，范围`1`~`1000`|
### RESPONSES
Field | Description
| ------ | ----------------------------- |
| symbol | 合约名称                       |
| ts     | 成交时间戳                     |
| side   | taker方向，枚举值：`buy`,`sell` |
| price  | 成交价格                       |
| size   | 成交数量                       |


# 时间
## 获取服务器时间

```json
{
    "code": "200000",
    "msg": "success",
    "data": 1546837113087
}
```

获取API服务器时间。这是Unix时间戳。

### HTTP Request
`GET /api/v1/timestamp`

### RESPONSES
Field | Description
--------- | -------
data | 服务器时间, Unix时间戳。


# 服务状态

## 获取当前服务状态
```json
{
    "code": "200000",
    "data": {
        "status": "open", //open: 正常运行, close: 服务关闭, cancelonly:只能撤单
        "msg": "upgrade match engine" //备注
    }
}
```
获取当前服务状态
### HTTP Request
`GET /api/v1/status`

### RESPONSES
Field | Description
--------- | -------
status | 服务状态：`open`（正常运行）、`close`（服务关闭）、`cancelonly`（只能撤单）
msg | 备注

---
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
`POST /api/v1/bullet-public`

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
`POST /api/v1/bullet-private`


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

## 交易实时行情 ticker
```json
  {
    "id": 1545910660740,                          
    "type": "subscribe",
    "topic": "/futuresMarket/ticker:XBTUSDM",
    "response": true                              
  }
```
```json
{
    "subject": "ticker",
    "topic": "/futuresMarket/ticker:XBTUSDM",
    "data": {
        "symbol": "XBTUSDM", // 行情
        "bestBidSize": 795, // 最佳买一价总数量
        "bestBidPrice": 3200.00, // 最佳买一价
        "bestAskPrice": 3600.00, // 最佳卖一价
        "bestAskSize": 284, // 最佳卖一价总数量
        "ts": 1650447469782 // 成交时间 - 毫秒
    }
}
```

Topic: `/futuresMarket/ticker:{symbol}`

* 推送频率: `实时推送`

订阅此topic，可获取指定交易对的最佳买一和卖一价（BBO）的数据推送。
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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
//订阅示例
{
    "id": 1545910660740,
    "type": "subscribe",
    "topic": "/futuresMarket/level2:XBTUSDM",
    "response": true
}
```
```json
//返回示例
{
    "subject": "level2",
    "topic": "/futuresMarket/level2:XBTUSDM",
    "type": "message",
    "data": {
        "start": 20711,
        "end": 20712,
        "bids": [],
        "asks": [
            ["14.250",30]
        ],
        "ts": 1650447324950
    }
}
```

Topic：`/futuresMarket/level2:{symbol}`

* 推送频率: `100ms`一次

订阅此topic，获取Level 2买卖盘数据，订阅成功后，Websocket系统将向您推送增量数据的消息。


校准流程：

1. 订阅 `/futuresMarket/level2:BTCUSDTM`
2. 开始缓存收到的更新。同一个价位，后收到的更新覆盖前面的。
3. 访问Rest接口`/api/v2/order-book?symbol=BTCUSDTM&limit=1000`获得一个1000档的深度快照
4. 将目前缓存到的信息中`end`<步骤3中获取到的快照中的`start`的部分丢弃(丢弃更早的信息，已经过期)
5. 将深度快照中的内容更新到本地orderbook副本中，本地保存`lastUpdateId`=最后一个增量消息的`end`，并从websocket接收到的第一个`start <= lastUpdateId` 且 `end > lastUpdateId` 的event开始继续更新本地副本。
6. 每一个新的event的`start`应该小于或等于上一个event的`end`,否则可能出现了丢包，请从step3重新进行初始化。
7. 每一个event中的挂单量代表这个价格目前的挂单量绝对值，而不是相对变化。
8. 如果某个价格对应的挂单量为`0`，表示该价位的挂单已经撤单或者被吃，应该移除这个价位。
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## 成交记录 
```json
//订阅示例
{
    "id": 1545910660741,
    "type": "subscribe",
    "topic": "/futuresMarket/execution:XBTUSDM",
    "response": true
}
```
```json
//返回示例
{
    "topic": "/futuresMarket/execution:XBTUSDM",
    "subject": "execution",
    "data": {
        "symbol": "XBTUSDM", // 合约
        "matchSide": "sell", // 成交方向 buy/sell
        "size": 1, // 订单剩余数量
        "price": 3200.00, // 成交价格
        "tradeId": 21518, // 成交编号
        "ts": 1650447324950 // 时间毫秒
    }
}
```

Topic: `/futuresMarket/execution:{symbol}`

* 推送频率: `实时推送`

每撮合一笔订单，系统就会推送此订单成交记录消息


<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## level2的5档全量数据推送频道 
```json
{
   "type": "message",
   "topic": "/futuresMarket/level2Depth5:BTCUSDM",
   "subject": "level2Depth5",
   "data": {
       "sequence":243,
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
       "ts": 1650447324950 //时间毫秒
    }
 }
```

Topic: `/futuresMarket/level2Depth5:{symbol}`

* 推送频率: `100ms`一次

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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
```json
{
    "type": "message",
    "topic": "/futuresMarket/level2Depth50:BTCUSDM",
    "subject": "level2Depth5",
    "data": {
        "sequence": 243,
        "asks": [
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
        "ts": 1650447324950 // 时间毫秒
    }
}
```

Topic: `/futuresMarket/level2Depth50:{symbol}`

* 推送频率: `100ms`一次
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## 标记价格、指数价格
```json
  //标记价格、指数价格
{
    "type": "message",
    "topic": "/futuresContract/markPrice",
    "subject": "mark.index.price",
    "sn": 123123,
    "data": {
        "symbol": "XBTUSDM", //合约symbol
        "granularity": 1000, //粒度
        "indexPrice": 4000.23, //指数价格
        "markPrice": 4010.52, //标记价格
        "ts": 1551770400000 //时间毫秒
    }
}
```

Topic: `/futuresContract/markPrice`

* 推送频率: `1s`一次


<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


## 资金费率
```json
 //资金费率
{
    "type": "message",
    "topic": "/futuresContract/fundingRate:XBTUSDM",
    "subject": "funding.rate",
    "sn": 25997405694459904,
    "data": {
        "granularity": 60000, //粒度(预测资金费率：1分钟粒度60000; 资金费率: 8小时粒度28800000)
        "fundingRate": -0.002966, //资金费率
        "ts": 1551770400000
    }
}
```

Topic: `/futuresContract/fundingRate:{symbol}`

* 推送频率: `8h`一次
<br/><br/>
* `granularity`粒度说明：
    * `60000`（1分钟粒度）
    * `28800000`（8小时粒度）

<br/>
<br/>
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

## 订单私有消息
```json
{
    "type": "message",
    "topic": "/futuresTrade/orders",
    "subject": "orderChange",
    "channelType": "private",
    "data": {
        "orderId": "5cdfc138b21023a909e5ad55", //订单号
        "tradeId": "123", // 撮合交易id
        "symbol": "BTCUSDTM", //合约symbol
        "eventType": "match", //消息类型，取值列表: "open", "match", "filled", "canceled" , "adl", "liquidation"
        "status": "MATCHING", //订单状态: "MATCHING", "FINISH"
        "matchSize": "", //成交数量 (当类型为"match"时包含此字段)
        "matchPrice": "", //成交价格 (当类型为"match"时包含此字段)
        "orderType": "LIMIT", //订单类型, "MARKET"表示市价单, "LIMIT"表示限价单
        "side": "BUY", // 订单方向，买或卖
        "price": "3600", //订单价格
        "size": "20000", //订单数量
        "remainSize": "20001", //订单剩余可用于交易的数量
        "filledSize": "20000", //订单已成交的数量
        "canceledSize": "0", // canceled消息中，订单减少的数量
        "clientOid": "5ce24c16b210233c36ee321d", //用户自定义ID
        "orderTime": , // 下单时间 ms
        "liquidity": "maker", // 成交方向，取taker一方的买卖方向
        "ts": // 时间戳 ms,
        "sn": //序列号 sn
    }
}
```

Topic: `/futuresTrade/orders`

* 推送频率: `实时推送`
<br/><br/>
* `eventType`消息类型说明：
    * `open`（订单进入买卖盘时发出的消息）
    * `match`（订单成交时发出的消息）
    * `filled`（订单因成交后状态变为DONE时发出的消息）
    * `canceled`（订单因被取消后状态变为DONE时发出的消息）
    * `adl`（adl订单）
    * `liquidation`（强平订单）
<br/><br/>
* `status`订单状态说明：
    * `MATCHING`（撮合中，表示订单挂在盘口上）
    * `FINISH`（订单完成）
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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
    "topic": "/futuresTrade/stopOrder",
    "subject": "stopOrder",
    "data": {
        "orderId": "5cdfc138b21023a909e5ad55", //订单编号
        "symbol": "BTCUSDTM", //合约symbol
        "eventType": "open", // 消息类型: open (止损下单成功), triggered (止损单触发), canceled (止损单取消)
        "orderType": "STOP", // 订单类型: STOP, STOP_MARKET, TAKE_PROFIT, TAKE_PROFIT_MARKET
        "side": "BUY", // 订单买卖方向
        "size": "1000", //数量
        "orderPrice": "9000", //订单价格
        "stopPrice": "9100", //止损单触发价格
        "workingType": "TP", //止损单触发价格类型
        "triggerSuccess": true, //触发成功标记, 只有triggered类型消息需要
        "error": "error.createOrder.accountBalanceInsufficient", //错误码, 触发失败时使用
        "createdAt": 1558074652423, //创建时间
        "ts": 1233123123 //消息时间 ms
    }
}
```

Topic: `/futuresTrade/stopOrder`

* 推送频率: `实时推送`
<br/><br/>
* `eventType`说明：
    * `open`（止损下单成功）
    * `triggered`（止损单触发）
    * `canceled`（止损单取消）
<br/><br/>
* `orderType`说明：
    * `STOP`（限价止损单）
    * `STOP_MARKET`（市价止损单）
    * `TAKE_PROFIT`（限价止盈单）
    * `TAKE_PROFIT_MARKET`（市价止盈单）

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

## 可用余额变更事件

Topic: `/futuresAccount/accountChange`

* 推送频率: `实时推送`

```json
{
    "type": "message",
    "topic": "/futuresAccount/accountChange",
    "channelType": "private",
    "subject": "futures.availableBalance.change",
    "data": {
        "accountEquity": "999922.7850290000",//合约账户总权益
        "availableBalance": "814058.3002530800",//可用余额
        "availableTransferBalance": "814058.3002530800",//可转金额
        "event": "ORDER_MARGIN_CHANGE_EVENT",//消息事件
        "currency": "USDT",//转出冻结
        "holdBalance": "0.0000000000",//转出冻结
        "orderMargin": "185863.9384800000",//订单保证金
        "positionMargin": "0.5594959200",//仓位保证金
        "walletBalance": "999922.7982290000",//钱包余额
        "sn": 11004,//序列号
        "ts": 1650426007488//消息时间 ms
    }
}
```

* `event`说明：
    * `ORDER_MARGIN_CHANGE_EVENT`（订单保证金变更事件）
    * `POSITION_CHANGE_EVENT`（仓位变更事件）
    * `TRANSFER_EVENT`（划转事件）


<br/>
<br/>
<br/>
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
{
    "type": "message",
    "topic": "/futuresPosition/position",
    "channelType": "private",
    "subject": "position.change",
    "sn": 45300,
    "data": {
        "autoDeposit": false,//是否自动追加保证金
        "changeType": "POSITION_CHANGE",//仓位变化类型
        "entryPrice": 14.431,//平均开仓价格
        "entryValue": -17.3178,//开仓价值
        "leverage": 5,//全局杠杆
        "liquidationPrice": 17.161,//强平价格
        "liquidationValue": -20.59413818,
        "maintenanceMargin": 0.173178,//维持保证金
        "maintenanceMarginRate": 0.01,//仓位维持保证金率
        "margin": 3.44951618,//仓位保证金
        "marginType": "ISOLATED",//保证金模式
        "markPrice": 14.15,//标记价格
        "openTime": 1650424933902,//开仓时间
        "qty": -12,//仓位数量
        "riskRate": 0.0458,//仓位风险率（<1，数值越大，风险越高）
        "settleCurrency": "USDT",//合约结算币种
        "side": "BOTH",//仓位方向
        "snapshotId": 6558,
        "symbol": "LINKUSDTM",//合约symbol
        "totalMargin": 3.78731618,//仓位总保证金（包含了未实现盈亏）
        "unrealisedPnl": 0.3378,//未实现盈亏
        "sn": 12323,//序列号
        "ts": 1650424935025//消息时间 ms
    }
}
```

Topic: `futuresPosition/position`

* 推送频率: `实时推送`
<br/><br/>
* `changeType`说明：
    * `MARGIN_CHANGE`（增减保证金仓位变更）
    * `POSITION_CHANGE`（用户成交引起的仓位数量变更）
    * `LIQUIDATION`（强平仓位变更）
    * `ADL`（自动减仓仓位变更）
    * `LEVERAGE_CHANGE`（全局杠杆仓位变更)
    * `FUNDING_SETTLE`（资金费用结算仓位变更）
    * `AUTO_DEPOSIT`（自动追加保证金状态仓位变更）
    * `SETTLEMENT`（交割仓位变更）
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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
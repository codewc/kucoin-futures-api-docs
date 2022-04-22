# 基本介绍
## 简介
欢迎使用KuCoin合约(KuCoin Futures)开发者文档。 此文档概述了交易功能、市场行情和其他应用开发接口。


KuCoin Futures API分为两部分：**REST API 和 Websocket 实时数据流**

 -  REST API 包含三个类别：**用户（私有）、交易（私有）、市场数据（公共）**
 -  Websocket包含两类：**公共频道和私人频道**

## 更新预告

**为了进一步提升API安全性，KuCoin已经升级到了V2版本的API-KEY，验签逻辑也发生了一些变化，建议到[API管理页面](https://futures.kucoin.com/api)添加并更换到新的API-KEY。KuCoin将继续支持使用老的API-KEY到2021年05月01日。请查看“消息签名”，了解更多详情**

#### 2022.03.24
* 废弃了[GET /api/v1/level2/message/query](#level-2-3)接口
* 新增接口返回值描述
* 新增[POST /api/v3/transfer-out](#kucoin-3)接口
* 新增[POST /api/v1/transfer-in](#7d42c0706c)接口

#### 2022.02.07
* 新增[GET /api/v1/position](#844f298257)接口返回字段：maintainMargin、riskLimitLevel.

#### 2021.12.07
* 修改仓位变化接口["topic": "/contract/position:XBTUSDM"](#52fd7608a9)说明

#### 2021.11.19
* 新增阶梯风险限额相关接口:[GET /v1/contracts/risk-limit/{symbol}](#8159742eec)、[POST /v1/position/risk-limit-level/change](#6728847f23) <br/> 
* 仓位推送的topic:/contract/position:{symbol} 新增风险限额调整结果[subject: position.adjustRiskLimit](#f9c6e147de).

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


## 接口端点

每个接口都提供了对应的端点，可在**HTTP请求**模块下获取。


对于**GET请求**，只需将请求参数拼接在请求路径后面。

例如：对于“仓位” 接口，其默认端点为/api/v1/position。请求“合约”参数（XBTUSDM）时，该端点将变为：**/api/v1/position?symbol=XBTUSDM**。因此，您最终请求的URL应为：**https://api-futures.kucoin.com/api/v1/position?symbol=XBTUSDM**。

## 请求
所有的请求和响应的内容类型都是application/json。  


除非另行说明，所有的时间戳参数均以Unix时间戳毫秒计算。如：**1544657947759**

## 参数

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
1015 | cloudflare frequency limit according to IP, block 30s--超频错误:基于ip的cloudflare限制,触发限制时间30s
40010 | Unavailable to place orders. Your identity information/IP/phone number shows you're at a country/region that is restricted from this service. -- 您当前属于限制国家，委托无法生效。
100001 | There are invalid parameters -- 无效参数
100002 | systemConfigError -- 系统配置有误
100003 | Contract parameter invalid -- 没有此合约
100004 | Order is in not cancelable state -- 订单不存在或者订单不可撤销
100005 | contractRiskLimitNotExist -- 合约风险限额不存在
200001 | The query scope for Level 2 cannot exceed xxx -- level2查询范围必须小于等于xxx　
200002 | Too many requests in a short period of time, please retry later--超频错误:基于接口业务层面的限制, 触发限制时间10s
200002 | The query scope for Level 3 cannot exceed xxx -- level3查询范围必须小于等于xxx
200003 | The symbol parameter is invalid. -- 参数symbol无效
300000 | request parameter illegal -- 请求参数非法
300001 | Active order quantity limit exceeded (limit: xxx, current: xxx) -- 超过活动委托数量限制，最大可提交 xxx 个活动委托，当前活动委托数: xxx
300002 | Order placement/cancellation suspended, please try again later. -- 系统暂停下单/撤单操作，将自动恢复，请稍后再试
300003 | Balance not enough, please first deposit at least 2 USDT before you start the battle -- 可用余额不足，委托成本为 xxx
300004 | Stop order quantity limit exceeded (limit: xxx, current: xxx) -- 超过条件委托数量限制，最大可提交 xxx 个条件委托，当前条件委托数: xxx
300005 | xxx risk limit exceeded -- 委托会导致超过最大风险限额 xxx
300006 | The close price shall be greater than the bankruptcy price. Current bankruptcy price: xxx. -- 平仓委托价格不能劣于仓位的破产价格，当前破产价格：xxx
300007 | priceWorseThanLiquidationPrice -- 加仓委托价格不能劣于仓位的强平价格，当前强平价格：xxx
300008 | Unavailable to place the order, there's no contra order in the market. -- 市场无订单，市价单提交失败
300009 | Current position size: 0, unable to close the position. -- 当前仓位为0，无法平仓
300010 | Failed to close the position -- 平仓失败
300011 | Order price cannot be higher than xxx -- 委托价格不能高于xxx
300012 | Order price cannot be lower than xxx -- 委托价格不能低于xxx
300013 | Unable to proceed the operation, there's no contra order in order book. -- 委托订单没有合法可撮合价格，订单提交失败
300014 | The position is being liquidated, unable to place/cancel the order. Please try again later. -- 强制平仓中，系统暂停下单/撤单，将自动恢复，请稍后重试
300015 | The order placing/cancellation is currently not available. The Contract/Funding is under the settlement process. When the process is completed, the function will be restored automatically. Please wait patiently and try again later. -- 合约交割/资金费用结算中，系统暂停下单/撤单，将自动恢复，请稍后重试
300016 | The leverage cannot be greater than xxx. -- 杠杆数不能大于xxx
300017 | Unavailable to proceed the operation, this position is for Futures Brawl -- 该仓位为乱斗仓位，不支持操作
300018 | clientOid parameter repeated -- clientOid参数重复
400001 | Any of KC-API-KEY, KC-API-SIGN, KC-API-TIMESTAMP, KC-API-PASSPHRASE is missing in your request header -- 请求头中缺少KC-API-KEY、KC-API-SIGN、KC-API-TIMESTAMP和KC-API-PASSPHRASE参数
400002 | KC-API-TIMESTAMP Invalid -- 请求时间与服务器时差超过5秒
400003 | KC-API-KEY not exists -- API-KEY不存在
400004 | KC-API-PASSPHRASE error -- API-PASSPHRAE不正确
400005 | Signature error -- 签名错误，请检查您的签名
400006 | The requested ip address is not in the api whitelist -- 请求IP不在API白名单中
400007 | Access Denied -- API权限不足，无法访问该URI目标地址。
400100 | Parameter Error -- 请求参数不合法
404000 | Url Not Found -- 找不到请求资源
411100 | User are frozen -- 用户已被冻结，请联系帮助中心
429000 | Too Many Requests -- 超频错误:基于接口的全站流量限制，可以直接重试请求
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
请求头中的 **KC-API-SIGN**: 

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

# 用户配置

## 修改用户自动追加保证金状态
```json
{
  "code": "200000",
  "data": true
} 
```
### HTTP请求
POST /api/v2/user-config/change-auto-deposit
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
autoDeposit | Boolean | 是 | 是否开启自动追加保证金（开启时，达到强平价格时会尝试自动追加保证金）

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
### HTTP请求
GET /api/v2/user-config/leverage
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### 返回值
属性  | 含义 |  
--------- | -----------|
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
### HTTP请求
GET /api/v2/user-config/leverages
### 参数
无
### 返回值
属性 | 含义 |
--------- | -----------|
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
### HTTP请求
POST /api/v2/user-config/adjust-leverage
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
leverage | Integer | 是 | 杠杆（正数）|
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol | 
leverage | 用户当前杠杆 | 
maxRiskLimit | 该杠杆下持仓最大限额 | 



# 账户

## 获取账户概览
```json
//TODO
{
  "code": "string",
  "data": [
    {
      "accountEquity": 0,
      "availableBalance": 0,
      "availableTransferBalance": 0,
      "currency": "string",
      "holdBalance": 0,
      "orderMargin": 0,
      "positionMargin": 0,
      "unrealisedPNL": 0,
      "walletBalance": 0
    }
  ],
  "msg": "string",
  "retry": true,
  "success": true
} 
```
### HTTP请求
GET /api/v2/account-overview
### 请求示例
GET /api/v2/account-overview?currency=BTC
### API权限
该接口需要通用权限
### 频率限制
此接口针对每个账号请求频率限制为30次/3s
### 参数  
参数|数据类型|含义
---|---|---
currency|String|[可选]币种 BTC、USDT、ETH、XRP、DOT,不传则查询所有币种
### 返回值  
字段|含义
---|---
currency|币种
walletBalance|钱包余额
positionMargin|仓位保证金
orderMargin|订单保证金
availableBalance|可用余额
unrealisedPNL|未实现盈亏
accountEquity|合约账户总权益
holdBalance|转出冻结
availableTransferBalance|可转金额


## 查询资金记录
```json
// TODO
{
  "code": "string",
  "data": [
    {
      "amount": "string",
      "currency": "string",
      "remark": "string",
      "time": 0,
      "type": "string",
      "walletBalance": "string"
    }
  ],
  "msg": "string",
  "retry": true,
  "success": true
}
```
### HTTP请求
GET /api/v2/transaction-history
### 请求示例
GET /api/v2/transaction-history?currency=USDT
#### API权限
该接口需要通用权限
#### 频率限制
此接口针对每个账号请求频率限制为30次/3s
### 参数  
参数|数据类型|含义
---|---|---
limit|Integer|记录数，默认100，最大1000
currency|String|[可选]币种
type|String|[可选]流水类型
startAt|Long|[可选]开始时间
endAt|Long|[可选]结束时间  

type说明：CONSUME-消费,
    FUNDING-资金费用,
    FEE-手续费,
    REALIZED_PNL-已实现盈亏,
    TRANSFER_IN-转入,
    TRANSFER_OUT-转出,
    SON_MOTHER_ACCOUNT_TRANSFER-合约子母账号划转,
    TRIAL_RECYCLE-体验金回收,
    REWARD-发奖,  
    DEDUCTION_REFUND-抵扣返还,
    INSURANCE_CROSS_POSITION_PAY-保险基金穿仓支付;

### 返回值  
字段|含义
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
//TODO
{
  "code": "string",
  "data": {
    "amount": 0,
    "applyId": "string",
    "bizNo": "string",
    "createdAt": "2022-04-11T05:11:05.591Z",
    "currency": "string",
    "fee": 0,
    "payAccountType": "string",
    "payTag": "string",
    "reason": "string",
    "recAccountType": "string",
    "recRemark": "string",
    "recSystem": "KUCOIN",
    "recTag": "string",
    "remark": "string",
    "sn": 0,
    "status": "string",
    "updatedAt": "2022-04-11T05:11:05.591Z"
  },
  "msg": "string",
  "retry": true,
  "success": true
}
```
### HTTP请求
POST /api/v2/transfer-out
### 请求示例
POST /api/v2/transfer-out
### API权限
该接口需要通用权限
### 频率限制
此接口针对每个账号请求频率限制为30次/3s 
### 参数  
参数|数据类型|含义
---|---|---
amount|Number|划转金额
currency|String|币种
recAccountType|String|接收账户类型，只能是MAIN-储蓄账户、TRADE-币币账户
### 返回值  
字段|含义
---|---
applyId|转出申请Id
bizNo|业务主键id
payAccountType|付款账户类型
payTag|付款账户子类型
remark|用户备注
recAccountType|收款账户类型
recTag|收款账户子类型
recRemark|收款账户流水备注
recSystem|收款服务方
status|状态APPLY-待处理,PROCESSING-处理中,PENDING_APPROVAL-待审核,APPROVED-审核通过,REJECTED-审核拒绝,PENDING_CANCEL-待退款,CANCEL-取消,SUCCESS-成功
currency|币种
amount|划转金额
fee|划转手续费
sn|序列号
reason|失败原因
createdAt|创建时间
updatedAt|更新时间



## 查询转出申请记录
``` json
//TODO
{
  "code": "string",
  "data": [
    {
      "amount": 0,
      "applyId": "string",
      "createdAt": 0,
      "currency": "string",
      "reason": "string",
      "recRemark": "string",
      "recSystem": "KUCOIN",
      "remark": "string",
      "sn": 0,
      "status": "string"
    }
  ],
  "msg": "string",
  "retry": true,
  "success": true
}
```
### HTTP请求
GET /api/v2/transfer-list
### 请求示例
GET /api/v2/transfer-list
### API权限
该接口需要通用权限
### 频率限制
此接口针对每个账号请求频率限制为30次/3s
### 参数  
参数|数据类型|含义
---|---|---
startAt|Long|业务发生起始时间
endAt|Long|业务发生截止时间
limit|Integer|记录数，默认100，最大1000
currency|String|币种
status|String|状态，PROCESSING-处理中，SUCCESS-成功, FAILURE-失败
### 返回值  
字段|含义
---|---
applyId|转出申请Id
currency|币种
recRemark|收款账户流水备注
recSystem|收款服务方
status|状态PROCESSING-处理中，SUCCESS-成功,FAILURE-失败
amount|转出金额
reason|失败原因
sn|序列号
createdAt|时间
remark|付款账户备注


## 取消转出
```json
//TODO
{
  "code": "string",
  "msg": "string",
  "retry": true,
  "success": true
}
```
### HTTP请求
DELETE /api/v2/cancel/transfer-out
### 请求示例
DELETE /api/v2/cancel/transfer-out
### API权限
该接口需要通用权限
### 频率限制
此接口针对每个账号请求频率限制为30次/3s 
### 参数  
参数|数据类型|含义
---|---|---
applyId|String|转出申请id


## 资金转入合约账户
``` json
//TODO
{
  "code": "string",
  "msg": "string",
  "retry": true,
  "success": true
}
```
### HTTP请求
POST /api/v2/transfer-in
### 请求示例
POST /api/v2/transfer-in
### API权限
该接口需要交易权限
### 频率限制
此接口针对每个账号请求频率限制为30次/3s
### 参数  
参数|数据类型|含义
---|---|---
amount|Number|交易金额
currency|String|币种
payAccountType|String|付款账户类型：只能是MAIN-储蓄账户，TRADE-交易账户





---
# 交易
此部分需进行签名验证。

# 订单

## 下单
```json
//response
{"success": true, "code": "200", "msg": "success", "retry": false, "data": {"orderId": "25880469442662400"}}
```
### HTTP请求
POST /api/v2/order
### 参数
参数 | 数据类型 | 是否必须 | 含义 
--------- | ------- | -----------| -----------
 symbol       | STRING   | YES      | 交易对                                                       
 side         | ENUM     | YES      | 买卖方向 SELL, BUY                                           
 price        | DECIMAL  | NO       | 价格                                                         
 type         | ENUM     | YES      | 订单类型 LIMIT, MARKET, STOP, TAKE_PROFIT, STOP_MARKET, TAKE_PROFIT_MARKET, TRAILING_STOP_MARKET 
 size         | INT      | NO       | 下单数量,使用closeOrder不支持此参数                          
 clientOid    | STRING   | NO       | 唯一的订单ID，可用于识别订单。如：UUID,只能包含数字、字母、下划线（_）或 分隔线（-） 
 reduceOnly   | BOOLEAN  | NO       | [可选] 只减仓标记, 默认值是 false 。值为true时，需要设置平仓数量 
 closeOrder   | BOOLEAN  | NO       | [可选] 平仓单标记, 默认值是 false 。值为true时，代表仓位全平 
 hidden       | BOOLEAN  | NO       | [可选] 订单不会在买卖盘中展示。选择hidden，不允许选择postOnly。 
 visibleSize  | INT      | NO       | [可选] 隐藏单最大可展示的数量。                              
 stopPrice    | DECIMAL  | NO       | 触发价, 仅 STOP, STOP_MARKET, TAKE_PROFIT, TAKE_PROFIT_MARKET 需要此参数 
 postOnly     | BOOLEAN  | NO       | [可选] 只挂单的标识。选择postOnly，不允许选择hidden。当订单时效为IOC策略时，该参数无效。 
 workingType  | STRING   | NO       | [可选] 止损单触发价类型，包括TP和MP                          
 timeInForce  | STRING   | NO       | 有效方法                                                     

根据order ***type*** 的不同， 对应需要的参数如下：

| Type                           | 参数                   |
| ------------------------------ | ---------------------- |
| LIMIT                          | size, price            |
| MARKET                         | size                   |
| STOP,TAKE_PROFIT               | size, price, stopPrice |
| STOP_MARKET,TAKE_PROFIT_MARKET | size, stopPrice        |

条件单触发说明：
`STOP`, `STOP_MARKET` 止损单：
    - 买入: 最新合约价格/标记价格高于等于触发价`stopPrice`
    - 卖出: 最新合约价格/标记价格低于等于触发价`stopPrice`

`TAKE_PROFIT`, `TAKE_PROFIT_MARKET` 止盈单：
    - 买入: 最新合约价格/标记价格低于等于触发价`stopPrice`
    - 卖出: 最新合约价格/标记价格高于等于触发价`stopPrice`

### 返回
属性  | 含义 
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

### HTTP请求
DELETE /api/v2/order

### 参数

参数 | 数据类型 | 是否必须 | 含义 
--------- | ------- | -----------| -----------
symbol | STRING | YES | 合约symbol
orderId | STRING | NO | 订单id 
clientOid | STRING | NO | 用户自定义orderId 

orderId和clientOid 二选一， 如果都传，默认使用orderId.


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
### HTTP请求
DELETE /api/v2/orders
### 参数
参数 | 数据类型 | 是否必须 | 含义 
--------- | ------- | -----------| -----------
symbol | STRING | YES | 合约symbol
### 返回值
属性  | 含义 
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
### HTTP请求
GET /api/v2/orders/historical-trades
### 参数
| 参数     | 数据类型 | 含义 |
| :------- | -------- | ------------ |
| symbol | String | [必填] 合约symbol	|
| startAt | Long  | [可选] 开始时间（毫秒）	|
| endAt | Long | [可选] 截止时间（毫秒）	|
| limit | Integer | 默认 50; 最大 1000.	|
| fromId | Long | [可选] 从哪一条成交id开始返回. 缺省返回最近的成交记录	|
### 返回值
| 字段   | 含义   |
| ------ | ------ |
| id | 交易记录ID |
| orderId | 订单ID |
| symbol | 合约交易对名字 |
| time | 撮合时间 |
| side | 方向：BUY:买/做多 SELL:卖/做空 |
| size | 数量 |
| price | 价格 |
| fee | 手续费 |
| feeRate | 手续费率 |
| feeCurrency | 手续费币种 |
| pnl | 盈亏 |
| pnlCurrency | 盈亏币种 |
| value | 价值 |
| orderType | 订单类型 |
| placeType | 下单类型：DEFAULT、OTOCO、STEP_REDUCE_POSITION、LIQUIDATION_TAKE_OVER、ADL_TRIGGER、ADL_LUCKY |
| maker | 是否maker |
| forceTaker | 是否强制作为taker处理|


## 查询单个订单详情
### HTTP请求
GET /api/v2/order/detail
### 参数
| 参数     | 数据类型 | 含义 |
| :------- | -------- | -- |
| orderId | String |	|
| clientOid | String |	|
### 返回值
| 字段   | 含义   |
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
| placeType | 下单类型：DEFAULT、OTOCO、STEP_REDUCE_POSITION、LIQUIDATION_TAKE_OVER、ADL_TRIGGER、ADL_LUCKY |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |



## 查询活跃订单
### HTTP请求
GET /api/v2/orders/active
### 参数
| 参数     | 数据类型 | 含义 |
| :------- | -------- | -------------- |
| symbol | String | [必须] 合约symbol	|
### 返回值
| 字段   | 含义   |
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
| placeType | 下单类型：DEFAULT、OTOCO、STEP_REDUCE_POSITION、LIQUIDATION_TAKE_OVER、ADL_TRIGGER、ADL_LUCKY |
| takeProfitPrice| 订单止盈止损 止盈价格 |
| cancelSize | 取消数量 |
| clientOid	| 客户订单编号 |
| avgPrice	| 平均成交价 |



## 查询全部活跃订单
### HTTP请求
GET /api/v2/orders/all-active
### 参数
无
### 返回值
| 字段   | 含义   |
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
| placeType | 下单类型：DEFAULT、OTOCO、STEP_REDUCE_POSITION、LIQUIDATION_TAKE_OVER、ADL_TRIGGER、ADL_LUCKY |
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
### HTTP请求
GET /api/v2/orders/history
### 参数
| 参数     | 数据类型 | 含义 |
| :------- | -------- | ---------- |
| symbol | String | 合约symbol	|
| startAt | Long  | 开始时间（毫秒）	|
| endAt | Long  | 截止时间（毫秒）	|
| fromId | Long | [可选] 从哪一条成交id开始返回.	|
| limit | Integer | 默认 50; 最大 1000.	|
### 返回值
| 字段   | 含义   |
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
| placeType | 下单类型：DEFAULT、OTOCO、STEP_REDUCE_POSITION、LIQUIDATION_TAKE_OVER、ADL_TRIGGER、ADL_LUCKY |
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
### HTTP请求
GET /api/v2/symbol-position
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol | 
qty | 仓位数量（负数表示做空，正数表示做多） | 
leverage | 全局杠杆 | 
marginType | 保证金模式（ISOLATED-逐仓，CROSSED-全仓） | 
side | 仓位方向（BOTH，LONG，SHORT），当持仓模式为单向仓位时（目前只支持单向持仓），该合约只能有一个仓位，side=BOTH，当持仓模式为双向仓位时，该合约可以有两个反向的仓位,side=LONG 或SHORT | 
autoDeposit | 是否自动追加保证金（为true时，强平触发自动追加保证金） | 
entryPrice | 平均开仓价格 | 
entryValue | 开仓价值 | 
margin | 仓位保证金 | 
totalMargin | 仓位总保证金（包含了未实现盈亏） | 
liquidationPrice | 强平价格 | 
unrealisedPnl | 未实现盈亏 | 
riskRate | 仓位风险率（<1，数值越大，风险越高） | 
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
### HTTP请求
GET /api/v2/all-position
### 参数
无
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol | 
qty | 仓位数量（负数表示做空，正数表示做多） | 
leverage | 全局杠杆 | 
marginType | 保证金模式（ISOLATED-逐仓，CROSSED-全仓） | 
side | 仓位方向（BOTH，LONG，SHORT），当持仓模式为单向仓位时（目前只支持单向持仓），该合约只能有一个仓位，side=BOTH，当持仓模式为双向仓位时，该合约可以有两个反向的仓位,side=LONG 或SHORT | 
autoDeposit | 是否自动追加保证金（为true时，强平触发自动追加保证金） | 
entryPrice | 平均开仓价格 | 
entryValue | 开仓价值 | 
margin | 仓位保证金 | 
totalMargin | 仓位总保证金（包含了未实现盈亏） | 
liquidationPrice | 强平价格 | 
unrealisedPnl | 未实现盈亏 | 
riskRate | 仓位风险率（<1，数值越大，风险越高） | 
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
### HTTP请求
POST /api/v2/change-margin
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
positionSide | String | 是 | 目前只支持BOTH
amount | BigDecimal | 是 | 增减保证金数量，负数提取保证金，正数为增加仓位保证金
### 返回值
属性  | 含义 |  
--------- | -----------|
data | true 表示增减保证金成功 | 

## 仓位盈亏历史
```json
{
  "code": "200000",
  "data": [
    {
      "symbol":"LINKUSDTM",
      "settleCurrency":"USDT",
      "type":"CLOSE_LONG",
      "leverage":5,
      "pnl":"-0.006",
      "roe":"-0.0098", 
      "openTime":1649299430000,
      "closeTime":1649299436000,
      "openPrice":"15.37",
      "closePrice":"15.34"
    }
  ]
}
```
### HTTP请求
GET /api/v2/close-pnl-his
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 否 | 合约symbol
type | String | 否 | 平仓类型（CLOSE_LONG-手动平多，CLOSE_SHORT-手动平多，LIQUID_LONG-强制平多，LIQUID_SHORT-强制平空，ADL_LONG-adl平多，ADL_SHORT-adl平空）
settleCurrency | String | 否 | 合约结算币种
startAt | String | 否 | 开始时间（时间跨度最多3个月）
endAt | String | 否 | 截止时间（时间跨度最多3个月）
limit | int | 是 | 记录数（默认50，最大1000）
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol| 
settleCurrency | 合约结算币种 | 
type | 平仓类型（CLOSE_LONG，CLOSE_SHORT-用户操作手动平多空仓，LIQUID_LONG，LIQUID_SHORT-强制平多空仓，ADL_LONG，ADL_SHORT-adl平多空仓） | 
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
### HTTP请求
GET /api/v2/contracts/active
### 参数
无
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol | 
type | 合约类型（FFWCSX-永续，FFICSX-交割） | 
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
status | Open(已上线)、PrepareSettled(准备结算)、BeingSettled（结算中）、Paused(已暂停)、CancelOnly(只能撤单) | 


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
### HTTP请求
GET /api/v1/contracts/{symbol}
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### 返回值
属性  | 含义 |  
--------- | -----------|
symbol | 合约symbol | 
type | 合约类型（FFWCSX-永续，FFICSX-交割） | 
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
status | Open(已上线)、PrepareSettled(准备结算)、BeingSettled（结算中）、Paused(已暂停)、CancelOnly(只能撤单) | 

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
### HTTP请求
GET /api/v2/contracts/risk-limit/{symbol}
### 参数
参数 | 数据类型 | 是否必须 | 含义 |  
--------- | ------- | -----------| -----------|
symbol | String | 是 | 合约symbol
### 返回值
属性  | 含义 |  
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
### HTTP请求
GET /api/v2/funding-history
### 参数
| 参数    | 数据类型 | 含义                                                  |
| :------ | -------- | ------------------------------ |
| symbol  | String   | 合约symbol                                            |
| startAt | Long     | [可选] 开始时间（毫秒）                               |
| endAt   | Long     | [可选] 截止时间（毫秒）                               |
| limit   | Integer  | 默认 50; 最大 1000.                             |
| fromId  | Long     | [可选] 从哪一条成交id开始返回. 缺省返回最近的成交记录 |
### 返回值
| 字段   | 含义   |
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
        "1649642340000", 	//时间
        15.748,						//开盘价
        15.748,						//最高价
        15.736,						//最低价
        15.736,						//收盘价
        656								//成交量
      ]
    ]
  }
```
### HTTP请求
GET /api/v2/kline/query
### 参数
| 参数     | 数据类型 | 含义  |
| :------- | -------- | -----|
| from   |  Long  | 起始时间（毫秒） |
| to   | Long | 结束时间（毫秒） |
| granularity   | Integer   | 代表分钟数，可选范围：1,5,15,30,60,120,240,480,720,1440,10080 |
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
### HTTP请求
GET /api/v2/contract/{symbol}/funding-rates
### 参数
| 参数     | 数据类型 | 含义 |
| :------- | -------- | --------- |
| symbol   | String   | 合约symbol |
| startAt | Long   | 开始时间 |
| endAt | Long   | 结束时间 |
| fromId | Long | [可选] 从哪一条成交id开始返回.	|
| limit | Integer | 默认 50; 最大 1000.	|
### 返回值
| 字段   | 含义   |
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
      [
        "13.965",
        202
      ],
      [
        "13.970",
        101
      ]
    ],
    "bids": [
      [
        "13.942",
        164
      ],
      [
        "13.937",
        164
      ],
      [
        "13.928",
        178
      ]
    ]
  }
}
```
返回买卖盘数据
### HTTP请求
GET /api/v2/order-book
### 参数
| 字段   | 类型   | 是否必需 | 说明                                |
| ------ | ------ | -------- | ----------------------------------- |
| symbol | 字符串 | Y        | 合约名称                            |
| limit  | 数字   | N        | 买卖盘档数，默认500，取值范围1~1000 |
### 返回值
| 字段       | 含义     |
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
    "bidPrice" "13.942",
    "bidSize": 164
  }
}
```
返回买卖盘最佳买一卖一价
### HTTP请求
GET /api/v2/ticker/bookTicker
### 参数
| 字段   | 类型   | 是否必需 | 说明     |
| ------ | ------ | -------- | -------- |
| symbol | 字符串 | Y        | 合约名称 |
### 返回值
| 字段     | 含义       |
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
### HTTP请求
GET /api/v2/ticker/price
### 参数
| 字段   | 类型   | 是否必需 | 说明     |
| ------ | ------ | -------- | -------- |
| symbol | 字符串 | Y        | 合约名称 |
### 返回值
| 字段   | 含义       |
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
### HTTP请求
GET /api/v2/trades
### 参数
| 字段   | 类型   | 是否必需 | 说明                             |
| ------ | ------ | -------- | -------------------------------- |
| symbol | 字符串 | Y        | 合约名称                         |
| limit  | 数字   | N        | 返回记录条数，默认20，范围1~1000 |
### 返回值
| 字段   | 含义                        |
| ------ | --------------------------- |
| symbol | 合约名称                    |
| ts     | 成交时间戳                  |
| side   | taker方向，枚举值：buy/sell |
| price  | 成交价格                    |
| size   | 成交数量                    |


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

### 返回值
字段 | 含义
--------- | -------
data | 服务器时间, Unix时间戳。


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
### HTTP请求
GET /api/v1/status

### 返回值
字段 | 含义
--------- | -------
status | 服务状态。open: 正常运行, close: 服务关闭, cancelonly:只能撤单
msg | 备注



---
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
    "type": "message",
    "userId": "5c32d69203aa676ce4b543c7",  // 不推荐使用, 后续版本将删除
    "channelType": "private",
    "topic": "/contract/position:XBTUSDM", 	
    "subject": "position.change", 
      "data": {
      "realisedGrossPnl": 0E-8,                //累加已实现毛利
      "symbol":"XBTUSDM",                      //有效合约代码
      "crossMode": false,                      //是否全仓
      "liquidationPrice": 1000000.0,           //强平价格
      "posLoss": 0E-8,                         //手动追加的保证金
      "avgEntryPrice": 7508.22,                //平均开仓价格
      "unrealisedPnl": -0.00014735,            //未实现盈亏
      "markPrice": 7947.83,                    //标记价格
      "posMargin": 0.00266779,                 //仓位保证金
      "autoDeposit": false,                    //是否自动追加保证金
      "riskLimit": 100000,                     //风险限额
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
      "changeReason": "positionChange",        //变化原因:marginChange、positionChange、liquidation、autoAppendMarginStatusChange、adl
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
**changeReason**
“marginChange”: 仓位保证金变化;
“positionChange”: 仓位变化;
“liquidation”: 强平;
“autoAppendMarginStatusChange”: 修改是否自动追加保证金;
“adl”: 自动减仓;

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>

### 风险限额调整结果

```json 
// Adjustment Result of Risk Limit Level
{ 
  "userId": "xbc453tg732eba53a88ggyt8c", 
  "topic": "/contract/position:ADAUSDTM", 
  "subject": "position.adjustRiskLimit", 
  "data": { 
    "success": true, // 是否成功 
    "riskLimitLevel": 1, // 当前风险限额等级
    "msg": "" // 失败原因 
  }
} 
``` 
失败原因有两种情况：1.持仓价值大于风险限额等级额度; 2.余额不足，保证金追加失败。

<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
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

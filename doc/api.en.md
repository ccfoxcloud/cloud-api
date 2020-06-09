- [API](#api)
  - [Signature](#signature)
    - [Overview](#oveview)
    - [Applying for create an API Key](#applying-for-create-an-api-key)
    - [Before Requesting](#before-requesting)
    - [Signature demo](#signature-demo)
  - [Public API](#public-api)
  - [Account Asset Equity](#account-asset-equity)
  - [API interface](#api-interface)
    - [Child Account Registeration](#child-account-registration)
    - [Child Account Login](#%e5%ad%90%e8%b4%a6%e5%8f%b7%e7%99%bb%e5%bd%95)
    - [Inquire Child Account Futures Order History](#%e6%9f%a5%e8%af%a2%e5%ad%90%e8%b4%a6%e6%88%b7%e6%9c%9f%e8%b4%a7%e5%8e%86%e5%8f%b2%e5%a7%94%e6%89%98)
    - [Inquire Child Account Futures Ord3er History](#%e6%9f%a5%e8%af%a2%e5%ad%90%e8%b4%a6%e6%88%b7%e6%9c%9f%e8%b4%a7%e5%8e%86%e5%8f%b2%e6%88%90%e4%ba%a4)
    - [Inquire Transfer Status](#%e6%9f%a5%e8%af%a2%e8%bd%ac%e8%b4%a6%e7%8a%b6%e6%80%81)
    - [Inquire Assets](#%e8%b5%84%e4%ba%a7%e6%9f%a5%e8%af%a2)
    - [Transfer](#%e8%bd%ac%e8%b4%a6)
    - [Inquire Child Account Fee](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e6%89%8b%e7%bb%ad%e8%b4%b9)
    - [Inquire Child Account Profit and Loss](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e7%9b%88%e4%ba%8f)
    - [Inquire Child Account Match Amount](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e6%88%90%e4%ba%a4%e9%87%91%e9%a2%9d)
    - [Inquire Child Account Assets Snapshots](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e5%90%88%e7%ba%a6%e8%b5%84%e4%ba%a7%e5%bf%ab%e7%85%a7)
  - [Business Message Queue](#b%e7%ab%afmq%e5%af%b9%e6%8e%a5)

*Read this in other languages: [简体中文](api.md)*

# API

- Production Environment URL： Please contact CCFOX operation staffs to get the production environment url, and provide the access ip for updating the whitelist. 
  URL：https://brokerapi.ccfox.com
- Testing：Using the simulate currency symbol cusd in production environment to do the testing.

## Commitment on numbers

All the fields notified as number, please process it as the decimal except the field named as xxxID, xxxCOUNT, etc that obviously are the int or bigint.

## Signature

### Overview
During the internet transaction, requesting API will have some potential risk. To make sure the API request is correct, all the private API are using your API key as signature to authenticate the parameter or value has been changed during the transaction.

一个合法的请求由以下几部分组成：

A legal requesting includes several parts：
- Requesting address：refers to server address  xxx，e.g. xxx/api/v1/broker/queryAsset.
- AccessKeyId: The Access Key in your API Key you applied.
- Timestamp: the timestamp to verify your request, for example 1548311559. This could help to prevent third  party interception of your request, and the timestamp only available in one minute.
- Signature：The value calculated by signature to make sure the signature is available and correct


``` js
// requst header
var headers = {
  'content-type' : 'application/json',
  'apiExpires': expires, //UNIX timestamp count by second, legal in one minute.

  'apiKey': apiKey, // apiKey
  'signature': signature // signature
};

```

### Applying for create an API Key

Create a business API Key by using CRM system

API Key includes following two parts


* `Access Key` API access Key 
* `Secret Key` a key for signature encrypt verification

### Before Requesting

Formatting the request of signature calculation due to SHA256 is used to do the calculation, therefore, different calculations would generate various results. Thus, the formatting is necessary before sending the request. (GET and POST are also different).

 e.g. assets check(GET)
`GET https://xxx.io/api/v1/broker/queryAsset?filter={applId：应用ID（默认为2）  queryUserId：用户ID  currencyId：币种ID}`

### Signature demo

1. [node.js](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/node.js)
2. [python](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/code.py)
3. [java](https://github.com/ccfoxexchange/ccfox_api/tree/master/java/)

## Public API

Currency API https://apitest.ccfox.com/future/queryCommonInfo

- Info

**Path：** /future/queryCommonInfo

**Method：** GET

- Request Parameter

- Return Data

| Name                     | Type      | Is required | Default Value | Description                                                     |           |
| ------------------------ | --------- | -------- | ------ | -------------------------------------------------------- | ----------------- |
| currencyList             | object [] | No   |        | Currency list                                                 | item Type: object |
| ├─ currencyId            | number    | yes     |        | currency id                                                   |                   |
| ├─ symbol                | string    | yes     |        | currency name                                                 |                   |
| ├─ displayPrecision      | number    | yes     |        | Display Precision                                             |                   |
| contractList             | object [] | No   |        | future contract list                                             | item Type: object |
| ├─ varietyId             | number    | yes     |        | variety ID                                                   |                   |
| ├─ applId                | number    | yes     |        | 1:spot，2：future                                          |                   |
| ├─ symbol                | string    | yes     |        | Symbol                                                  |                   |
| ├─ priceTick             | number    | yes     |        | Price tick                                             |                   |
| ├─ lotSize               | number    | yes     |        | Minimum Trading Unit                                             |                   |
| ├─ takerFeeRatio         | number    | yes     |        | Taker Fee Ratio                                            |                   |
| ├─ makerFeeRatio         | number    | yes     |        | Maker Fee Ratio                                            |                   |
| ├─ limitMaxLevel         | number    | yes     |        | Limit price max level                                     |                   |
| ├─ marketMaxLevel        | number    | yes     |        | Market price max level                                     |                   |
| ├─ maxNumOrders          | number    | yes     |        | Maximum number of orders                                         |                   |
| ├─ priceLimitRate        | number    | yes     |        | Price limit ratio                                                 |                   |
| ├─ commodityId           | number    | yes     |        | Commodity ID                                               |                   |
| ├─ currencyId            | number    | yes     |        | Currency ID                                               |                   |
| ├─ contractType          | number    | yes     |        | Contract Type，1 fixed term，2 perpetual                                    |                   |
| ├─ deliveryType          | number    | yes     |        | Delievery Type，1 cash ，2 Physical                           |                   |
| ├─ deliveryPeriod        | number    | yes     |        | Delivery Date，1 day 2 week 3 month                                     |                   |
| ├─ contractSide          | number    | yes     |        | Contract Side，1 Positive Inverse                                   |                   |
| ├─ contractUnit          | number    | yes     |        |  Contract Unit                                                 |                   |
| ├─ posiLimit             | number    | yes     |        | Position limit                                                 |                   |
| ├─ orderLimit            | number    | yes     |        | Order Limit                                                 |                   |
| ├─ impactMarginNotional  | number    | yes     |        | Impact Margin Notional                                             |                   |
| ├─ fairBasisInterval     | number    | yes     |        | Fair basis interval( per second)                               |                   |
| ├─ clearPriceInterval    | number    | yes     |        | Clear Price Interval                                   |                   |
| ├─ deliveryPriceInterval | number    | yes     |        | Delivery Price Interval                                           |                   |
| ├─ minMaintainRate       | number    | yes     |        | Minimum Maintenance Rate                                         |                   |
| ├─ createTime            | number    | yes     |        | Create time                                                 |                   |
| ├─ enabled               | number    | yes     |        | Whether enabled or not                                                 |                   |
| ├─ contract_status       | number    | yes     |        | 1 call auction，2 continuous trading，3 trading suspension，4 delisted，5 unlisted'       |                   |
| ├─ futureContractList    | object [] | yes     |        |                                                          | item Type: object |
| ├─ contractId            | number    | yes     |        |   Contract Id                                                   |                   |
| ├─ applId                | number    | yes     |        | Application ID                                                 |                   |
| ├─ symbol                | string    | yes     |        | Contract Symbol                                                 |                   |
| ├─ priceTick             | number    | yes     |        | Minimum Tick Size                                             |                   |
| ├─ lotSize               | number    | yes     |        | Minimum Trading Size                                           |                   |
| ├─ takerFeeRatio         | number    | yes     |        | Taker Fee Ratio                                            |                   |
| ├─ makerFeeRatio         | number    | yes     |        | Maker Fee Ratio                                            |                   |
| ├─ limitMaxLevel         | number    | yes     |        | Limit Order Max Level                                     |                   |
| ├─ marketMaxLevel        | number    | yes     |        | Market Order Max Level                                     |                   |
| ├─ maxNumOrders          | number    | yes     |        | Max Maker Order                                         |                   |
| ├─ priceLimitRate        | number    | yes     |        | Price Limit Rate                                                 |                   |
| ├─ commodityId           | number    | yes     |        | Commodity Id                                               |                   |
| ├─ currencyId            | number    | yes     |        | Currency ID                                               |                   |
| ├─ contractStatus        | number    | yes     |        | trading pair status:1 call auction, 2 continuous trading, 3 trading suspension 4 delisted 5 unlisted |                   |
| ├─ listPrice             | number    | yes     |        | List Price                                                 |                   |
| ├─ listTime              | number    | yes     |        | List Time                                                 |                   |
| ├─ createTime            | number    | yes     |        | Create Time                                                 |                   |
| ├─ contractType          | number    | yes     |        | contract type, 1 for fixed-term contract, 2 for perpetual                                   |                   |
| ├─ deliveryType          | number    | yes     |        | delivery type, 1 for cash, 2 for physical                   |                   |
| ├─ deliveryPeriod        | number    | yes     |        | delivery period, 0 for perpetual, 1 for day, 2 for week, 3 for month                                 |                   |
| ├─ contractSide          | number    | yes     |        | Contract side, 1 positive, 2 reverse                                   |                   |
| ├─ contractUnit          | number    | yes     |        | Contract Unit                                                 |                   |
| ├─ lastTradeTime         | number    | yes     |        | Last Trade Time                                             |                   |
| ├─ deliveryTime          | number    | yes     |        | Last Delivery Time                                            |                   |
| ├─ posiLimit             | number    | yes     |        | Position Limit                                                 |                   |
| ├─ orderLimit            | number    | yes     |        | Order Limit                                                 |                   |
| ├─ impactMarginNotional  | number    | yes     |        | Margin Impace                                             |                   |
| ├─ minMaintainRate       | number    | yes     |        | Minimum Maintain Rate                                         |                   |
| ├─ fairBasisInterval     | number    | yes     |        | Fair Price Basis Difference Interval                                       |                   |
| ├─ clearPriceInterval    | number    | yes     |        | Clear Price Interval                                           |                   |
| ├─ deliveryPriceInterval | number    | yes     |        | Delivery Price Interval                                           |                   |
| ├─ varietyId             | number    | yes     |        | Variety ID                                           |                   |

## Account Asset Equity

API： https://apitest.ccfox.com/futureAsset/queryAccountEquity

- Basic Info

**Path：** /futureAsset/queryAccountEquity

**Method：** GET

**Description：**

### Request Parameter
### Query：

| Parameter Name | Is required | Example                      | Description                                                         |
| :------- | :------- | :------------------------ | :----------------------------------------------------------- |
| legalSymbol   | Yes       | CNY | Fiat symbol, default:CNY|

### Return Data

| Name                     | Type      | Is required | Default Value | Description                                                     | Other Info          |
| ------------------------ | --------- | -------- | ------ | -------------------------------------------------------- | ----------------- |
| code             | number     | yes   |        | 0, succeed，others are failed | |
| msg              | string     | yes   |        | message | |
| data             | object []  | yes   |        | data collection | |
| ├─accountType    | number     | yes   |        | account Type，1 spot，2 futures，5 deposit，10 event | Cloud contract user only has future account：2|
| ├─totalAccountEquity    | number     | yes   ||Total Account Equity, which is the estimate BTC value | |
| ├─simulationAvailableMargin    | number     | yes   || Simulation Available Margin | |
| ├─totalLegalValue    | object     | yes   ||Fiat Value | |
| ├├─value    | number     | yes   ||Total Fiat Value | |
| ├├─symbol    | string     | yes   ||Fiat Symbol | |
| ├─currencyAssetList    | object     | yes   ||Currency Asset List | |
| ├├─currencyId    | number     | yes   ||Currency ID | |
| ├├─accountEquity    | number     | yes   ||Account Equity | |
| ├├─floatProfitLoss    | number     | yes   ||Float Profit & Loss | |
| ├├─availableMargin    | number     | yes   ||Available Margin | |
| ├├─initMargin    | number     | yes   ||Initial Margin | |
| ├├─currentCloseProfitLoss    | number     | yes   ||Curreny Closed Profit & Loss | |
| ├├─totalLeverageLevel    | number     | yes   || Total Leverage Level | |
| ├├─frozenForTrade    | number     | yes   || Frozen Balance For Margin Trade | |
| ├├─available    | number     | yes   || Available balance, except futures and spot | |
| ├├─locked    | number     | yes   || Locked balance, except futures and spot | |
| ├├─btcValuation    | number     | yes   ||B TC valuation, except futures | |
| ├├─totalBalance    | number     | yes   || Total Account Balance | |




## API interface

### Child account registration

- Basic Info

**Path：** /api/v1/broker/registerSon

**Method：** POST

**Description：**

verifications： 
  Signature，API Key Authorisation，User group，Duplicate Registration Request，Registered，Registration Login Threshold.

- Request Parameter

**Headers**

| Parameter Name | Value                                                            | Is required | Example                                                          | Description                                                                  |
| -------------- | ---------------------------------------------------------------- | ----------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Content-Type   | application/json                                                 | Yes         |                                                                  |                                                                              |
| signature      | 1dec55ac42478858d322cf841f22522bb47752bbd5d330adbe84db9cf5854733 | Yes         | 1dec55ac42478858d322cf841f22522bb47752bbd5d330adbe84db9cf5854733 | Signature，generation method：encryption on body parameter by sha256（Ask the technical staffs for the demo document）|
| apiKey         | 61ba5fc3-2384-472e-8712-e5f83b358815                             | Yes         | 61ba5fc3-2384-472e-8712-e5f83b358815                             |  API key：unique key for each business parter （need to be applied）         |
| apiExpires     | 1561708654079                                                    | Yes         | 1561708654079                                                    | API expire time（timestamp：millisecond）                                    |
| UNIQUE         | XXXX                                                             | Yes         |                                                                  | uuid, this id will be unique in each request, to avoid duplicate submission  |

**Body**

| Name        | Type   | Is required | Default Value | Description                         | Other Info |
| ----------- | ------ | -------- | ------ | ---------------------------- | -------- |
| groupId     | number | yes     |        | Group ID                       |          |
| areaCode    | string | No   |        | Area Code  e.g. +86             |          |
| sonUserName | string | yes     |        | Username（15 digits）   |          |
| sonPassword | string | yes     |        | Password:  32 bit MD5 Encrypted       |          |
| phone       | string | No   |        | Phone number( area code is required if phone number is submitted) |          |
| email       | string | No   |        | Email Address                         |          |

- Return Data

| Name | Type   | Is required | Default Value | Description                           | Other Info |
| ---- | ------ | -------- | ------ | ------------------------------ | -------- |
| code | number | yes     |        | Return code（0: success  others: fail） |          |
| msg  | string | yes     |        | Return Message                       |          |
| data | null   | yes     |        |                                |          |

### 子账号登录



- Basic Info

**Path：** /api/v1/broker/loginSon

**Method：** POST

**Description：**

- Request Parameter

**Headers**

**Body**

| Name        | Type   | Is required | Default Value | Description                      | Other Info |
| ----------- | ------ | -------- | ------ | ------------------------- | -------- |
| sonUserName | string | yes     |        | UserName                  |          |
| sonPassword | string | yes     |        | Password:  32 bit MD5 Encrypted|          |

- Return Data

| Name             | Type   | Is required | Default Value | Description                           | Other Info |
| ---------------- | ------ | -------- | ------ | ------------------------------ | -------- |
| code             | number | yes     |        |  Return code（0: success  others: fail |          |
| msg              | string | yes     |        | Return Message                       |          |
| data             | object | yes     |        |                                |          |
| ├─ access_token  | string | yes     |        | Token                        | Avaliable Time: Millisecond as Unit         |
| ├─ token_type    | string | yes     |        | tokenType                      |          |
| ├─ refresh_token | string | yes     |        | Update Toekn                      |          |
| ├─ expires_in    | string | yes     |        | Available Time( per second )          |          |
| ├─ scope         | string | yes     |        | Scope                               |          |
| ├─ userId        | number | yes     |        | user id                         |          |

### Inquire Child account futures contract records



- Basic Info

**Path：** /api/v1/broker/queryHisOrder

**Method：** GET

**Description：**

- Request Parameter
- Query：

| Parameter Name | Is required | Example                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| :------------- | :---------- | :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filter         | Yes         | %7B%22userId%22%3A+275%7D    | attention： parameter filter=%7B%22userId%22%3A275%7D，value is URLEcode， Convertion to upper case is required after coding, e.g. %3a to %3A result value：{"userId":275} userId（int）：user ID， required contractId（int）：contract ID， not required side（int）：pre/post page, not required startDate（long）：start timestamp, not required endDate（long）：end timestamp, not required pageNum（int）：current page, not required pageSize（int）：display page, not required
  |




- Return Data

| Name                | Type      | Is required | Default Value | Description                                                                                                                                                            | Other Info          |
| ------------------- | --------- | ----------- | ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| code                | number    | No          |               |                                                                                                                                                                        |                   |
| msg                 | string    | No          |               |                                                                                                                                                                        |                   |
| data                | object [] | No          |               |                                                                                                                                                                        | item Type: object |
| ├─ applId           | number    | yes         |               | 2: Futures                                                                                                                                                             |                   |
| ├─ timestamp        | number    | yes         |               | Timestamp When ordering                                                                                                                                                |                   |
| ├─ userId           | number    | yes         |               | User ID                                                                                                                                                          |                   |
| ├─ contractId       | number    | yes         |               | Contract ID                                                                                                                                                        |                   |
| ├─ uuid             | string    | yes         |               | Order ID                                                                                                                                                        |                   |
| ├─ side             | number    | yes         |               | Side of Bid or Ask，1 bid，-1 ask                                                                                                                                             |                   |
| ├─ price            | string    | yes         |               | Order Price                                                                                                                                                        |                   |
| ├─ quantity         | string    | yes         |               | Order Quantity                                                                                                                                                        |                   |
| ├─ orderType        | number    | yes         |               | Order Type，1（Limit），3（Market）                                                                                                                              |                   |
| ├─ orderSubType     | number    | yes         |        | Order sub Type 0（Default Value），1（Passive order），2（Conditional Order Market Price），3（Conditional Price Index Price），4（Conditional Price Mark Price）            |                   |
| ├─ timeInForce      | number    | yes         |        | Order time in force type：1（Available before cancelled），2（match immediately and rest will be cancelled，not actived），3（all matched or canceled，not actived），4（first five matched and rest will be canceled，not actived），5（first five matched and rest will turn to limit price，not actived） |                   |
| ├─ minimalQuantity  | string    | yes         |        | Minimal quantity                                                                                                                                                      |                   |
| ├─ stopPrice        | string    | yes         |        | Stop profit & loss price                                                                                                                                                      |                   |
| ├─ stopCondition    | number    | yes         |        | Stop profit and loss symbol 1（stop profit，not actived），2（stop loss,not actived），3（only deleverage，not actived）                                                      |                   |
| ├─ orderStatus      | number    | yes         |        | Order status 0: not apply,1:applying,2:failed apply, 3:partial matched ,4:all matched,5:partial cancelled, 6:all cancelled, 7:cancelling,8:Invalid, 11:orders that condition higher than cache ,12:orders that condition lower than cache.             |                   |
| ├─ makerFeeRatio    | string    | yes     |        | maker Fee Ratio                                                                                                                                                  |                   |
| ├─ takerFeeRatio    | string    | yes     |        | taker Fee Ratio                                                                                                                                                  |                   |
| ├─ clOrderId        | string    | yes     |        | Client Order ID                                                                                                                                                    |                   |
| ├─ filledCurrency   | string    | yes     |        | Filled Curency                                                                                                                                                        |                   |
| ├─ filledQuantity   | string    | yes     |        | Filled quantity                                                                                                                                                        |                   |
| ├─ canceledQuantity | string    | yes     |        | cancelled quantity                                                                                                                                                        |                   |
| ├─ matchTime        | number    | yes     |        | Matched time                                                                                                                                                        |                   |
| ├─ positionEffect   | number    | yes     |        | Position effect 1 for open 2 for close                                                                                                                                            |                   |
| ├─ marginType       | number    | yes     |        | Margin Type，1 Cross ，2 for isolated                                                                                                                                        |                   |
| ├─ marginRate       | string    | yes     |        | Margin ratio（ The reciprocal is the times of leverage）                                                                                                                            |                   |
| ├─ fcOrderId        | string    | yes     |        | The order Id of which is liquidated，not null for liquidation order               |                   |
| ├─ deltaPrice       | string    | yes     |        | Difference between mark price and order price                                     |                   |
| ├─ frozenPrice      | string    | yes     |        | Price of funding                          |                   |

### Inquire Child futures Matches Records



- Basic Info

**Path：** /api/v1/broker/queryHisMatch

**Method：** GET

**Description：**

- Request Parameter

**Headers**

- Query：

| Parameter Name | Is required | Example                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| :------- | :------- | :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filter   | Yes       | %7B%22userId%22%3A+275%7D | attention： parameter filter=%7B%22userId%22%3A275%7D，value is URLEcode， Convertion to upper case is required after coding, e.g. %3a to %3A result value：{"userId":275} userId（int）：user ID， required contractId（int）：contract ID， not required side（int）：pre/post page, not required startDate（long）：start timestamp, not required endDate（long）：end timestamp, not required pageNum（int）：current page, not required pageSize（int）：display page, not required |




- Return Data

| Name                 | Type      | Is required | Default Value | Description                                                                   | Other Info          |
| -------------------- | --------- | -------- | ------ | ---------------------------------------------------------------------- | ----------------- |
| code                 | number    | No   |        |                                                                        |                   |
| msg                  | string    | No   |        |                                                                        |                   |
| data                 | object [] | No   |        |                                                                        | item Type: object |
| ├─ applId            | number    | yes     |        | 2: Futures                                                                |                   |
| ├─ matchTime         | number    | yes     |        | Match Time                                                               |                   |
| ├─ contractId        | number    | yes     |        | Contract ID                                                               |                   |
| ├─ execId            | string    | yes     |        | Executive ID                                                                 |                   |
| ├─ bidUserId         | number    | yes     |        | Bid User ID                                                             |                   |
| ├─ askUserId         | number    | yes     |        | Ask User ID                                                             |                   |
| ├─ bidOrderId        | string    | yes     |        | Bid Order ID                                                             |                   |
| ├─ askOrderId        | string    | yes     |        | Ask Order ID                                                             |                   |
| ├─ matchPrice        | string    | yes     |        | Match Price                                                                 |                   |
| ├─ matchQty          | string    | yes     |        | Match Quantity                                                               |                   |
| ├─ matchAmt          | string    | yes     |        | Match Amount                                                               |                   |
| ├─ bidFee            | string    | yes     |        | Bid Fee                                                             |                   |
| ├─ askFee            | string    | yes     |        | Ask Fee                                                             |                   |
| ├─ takerSide         | number    | yes     |        | Taker Side 1 for bid，-1 for Ask                                                 |                   |
| ├─ updateTime        | number    | yes     |        | Lastest Update Time                                                           |                   |
| ├─ bidPositionEffect | number    | yes     |        | Bid Position Effect：1 open 2 Close                                               |                   |
| ├─ askPositionEffect | number    | yes     |        | Ask Position Effect ：1 open 2 Close                                               |                   |
| ├─ bidMarginType     | number    | yes     |        | Bid Margin Type：1 Cross, 2 Isolated                                           |                   |
| ├─ askMarginType     | number    | yes     |        | Ask Margin Type：1 Cross, 2 Isolated                                           |                   |
| ├─ bidInitRate       | string    | yes     |        | Bid Initial Rate                                                       |                   |
| ├─ askInitRate       | string    | yes     |        | Ask Initial Rate                                                       |                   |
| ├─ bidMatchType      | number    | yes     |        | Bid Match Type：0 Normal 1 Liquidation 2 Deleverage（bankrupt）3 deleverage（profit） |                   |
| ├─ askMatchType      | number    | yes     |        | Ask Match Type：0 Normal 1 liquidation 2 deleverage (bankrupt）3 deleverage（profit） |                   |
| ├─ bidPnlType        | number    | yes     |        | Bid Profit & loss Type：0 Normal matching 1 Close 2 Liquidated 3 Forced-deleverageing                             |                   |
| ├─ bidPnl            | number    | yes     |        | Bid Profit & loss                                                            |                   |
| ├─ askPnlType        | number    | yes     |        | Ask Profit & loss Type：0 Normal matching 1 Close 2 Liquidated 3 Forced-deleverageing                             |                   |
| ├─ askPnl            | number    | yes     |        | Ask profit 7 loss                                                           |                   |

### Inquire Assets Status



- Basic Info

**Path：** /api/v1/broker/queryTransferStatus

**Method：** GET

**Description：**

- Request Parameter

**Headers**

**Query**

| Parameter Name | Is required | Example                                            | Description                                                                                                                                                                                                                                      |
| -------- | -------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       | {"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"} | Attention: parameter filter={"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"}，value is URLEcode, conversion is required after coding : {"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"} clientId（String）：API sender transfer id, required |

- Return Data

| Name             | Type   | Is required | Default Value | Description                                                                                                                 | Other Info |
| ---------------- | ------ | -------- | ------ | -------------------------------------------------------------------------------------------------------------------- | -------- |
| code             | number | yes     |        |                                                                                                                      |          |
| msg              | string | yes     |        |                                                                                                                      |          |
| data             | object | yes     |        |                                                                                                                      |          |
| ├─clientId       | string | yes     |        | Transfer Client Id                                                                                                              |          |
| ├─transferStatus | number | yes     |        | Transfer status：10000, "Succeed",     10001, "Failed",     10002, "Transfering",     10003, "Verifying",     10004, "Failed to Verify "; |          |

### 资产查询



- Basic Info

**Path：** /api/v1/broker/queryAsset

**Method：** GET

**Description：**

- Request Parameter

| Parameter Name | Is required | Example                                            | Description                                                                                                                                                                                                                                                                                                                              |
| -------- | -------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       | {"applId":5,"queryUserId":"129","currencyId":1} | Attention： parameter filter={"applId":5,"queryUserId":"129","currencyId":1}，value is URLEcode， conversion to upper case is required. E.g. {"applId":5,"queryUserId":"129","currencyId":1}  applId（int）: 5：my wallet，2: future wallet， reuqired queryUserId（int）：user ID, check this user assets， required currencyId（int）：currency ID not required |

- Return Data

| Name                | Type      | Is required | Default Value | Description           | Other Info          |
| ------------------- | --------- | -------- | ------ | -------------- | ----------------- |
| code                | number    | No   |        |                |                   |
| msg                 | string    | No   |        |                |                   |
| data                | object [] | No   |        |                | item Type: object |
| ├─ currencyId       | number    | yes     |        | Currency ID         |                   |
| ├─ available        | number    | yes     |        | Availabl fund       |                   |
| ├─ totalBalance     | string    | yes     |        | Total Balance         |                   |
| ├─ frozenForTrade   | string    | yes     |        | Frozen assets for trade       |                   |
| ├─ initMargin       | string    | yes     |        | Initial Margin   |                   |
| ├─ frozenInitMargin | string    | yes     |        | Forzen Initial Margin |                   |
| ├─ closeProfitLoss  | string    | yes     |        | Closed Profit & Loss     |                   |

### Transfer



- Basic Info

**Path：** /api/v1/broker/transfer

**Method：** POST

**Description：**

Verification:
  Signature Verification，API Key authentication，quantity，available currency，available app ID（applID），business account verification，remain amount and transfer amount（only item：5 my wallet）

- Request Parameter

**Headers**

**Body**

| Name       | Type   | Is required | Default Value | Description                                           | Other Info |
| ---------- | ------ | -------- | ------ | ---------------------------------------------- | -------- |
| fromUserId | number | yes     |        | Source User ID                                     |          |
| fromApplId | number | yes     |        | Source App: default 2                              |          |
| toUserId   | string | yes     |        | Receiving User ID                                     |          |
| toApplId   | number | yes     |        | Receiving: default 2                              |          |
| currencyId | number | yes     |        | Currency ID                                         |          |
| quantity   | string | yes     |        | Transfer amount：Six decimal Limition places,for cuse minimum transfer: 0.1 |          |
| clientId   | string | yes     |        | Transaction ID：UUID unique symbol，32bit (digits + character) |          |

- Return Data

| Name | Type   | Is required | Default Value | Description                                   | Other Info |
| ---- | ------ | -------- | ------ | -------------------------------------- | -------- |
| code | number | yes     |        | Return code（0：success  others: fail）         |          |
| msg  | string | yes     |        | Return message                               |          |
| data | object | yes     |        | Transaction no.（for check the status of transaction by clientId）|          |


### 查询子用户手续费



- Basic Info

**Path：** /api/v1/broker/queryDayFees

**Method：** GET

**Description：**

- Request Parameter

**Headers**

**Query**

| Parameter Name | Is required | Example                                                                | Description                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       |  Value is URLEcode， conversion to upper case is required after coding，e.g. %3a to %3A, result value is：{"currencyId":7,"statDate":"20200109"}, Parameters： currencyId(required,  currency ID), statDate(required status date，e.g. 20200109), pageNum(not required current page，default：1), pageSize(not required ，page size default：1000，take1000 when it is over)
 |

- Return Data

| Name        | Type   | Is required | Default Value | Description       | Other Info |
| ----------- | ------ | -------- | ------ | ---------- | -------- |
| code        | number | yes     |        |            |          |
| msg         | string | yes     |        |            |          |
| data        | object | yes     |        |            |          |
| ├─pageNum   | number | yes     |        | Current Page     |          |
| ├─pageSize  | number | yes     |        | Page Number     |          |
| ├─total     | number | yes     |        | Total Records |          |
| ├─pages     | number | yes     |        | Pages       |          |
| ├─list      | object | yes     |        | List of data       |          |
| ├──userId   | number | yes     |        | User ID     |          |
| ├──totalFee | number | yes     |        | Total Fee |          |


### Inquire Child Profit and Loss



- Basic Info

**Path：** /api/v1/broker/queryDayProfits

**Method：** GET

**Description：**

- Request Parameter

**Headers**

**Query**

| Parameter Name | Is required | Example                                                                | Description                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       | filter=%7B%22currencyId%22%3A7%2C%22statDate%22%3A%2220200109%22%7D | Value is URLEcode， conversion to upper case is required after coding，e.g. ：%3a to %3A, result value：{"currencyId":7,"statDate":"20200109"}, Parameters： currencyId(required currency ID), statDate(required status date，e.g. ：20200109), pageNum(not required currency page，default：1), pageSize(not required page size，default：1000，take 1000 when it is over) |

- Return Data

| Name           | Type   | Is required | Default Value | Description           | Other Info |
| -------------- | ------ | -------- | ------ | -------------- | -------- |
| code           | number | yes     |        |                |          |
| msg            | string | yes     |        |                |          |
| data           | object | yes     |        |                |          |
| ├─pageNum      | number | yes     |        | Current page number         |          |
| ├─pageSize     | number | yes     |        | Page Size         |          |
| ├─total        | number | yes     |        | Total Records     |          |
| ├─pages        | number | yes     |        | Pages           |          |
| ├─list         | object | yes     |        | List of data           |          |
| ├──userId      | number | yes     |        | user ID         |          |
| ├──totalProfit | number | yes     |        | Total Profit & Loss |          |


### Inquire Child Account Match Amount

- Basic Info

**Path：** /api/v1/broker/queryDayMatchAmts

**Method：** GET

**Description：**

- Request Parameter

**Headers**

**Query**

| Parameter Name | Is required | Example                                                                | Description                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       | filter=%7B%22currencyId%22%3A7%2C%22statDate%22%3A%2220200109%22%7D | Value is URLEcode, conversion to upper case is required after coding，e.g. %3a to %3A,result value is：{"currencyId":7,"statDate":"20200109"}, Parameters： currencyId(required currency ID), statDate(required status date，e.g. 20200109), pageNum(not required current page，default：1), pageSize(not required page size，default：1000，take 1000 when it is over) |

- Return Data

| Name        | Type   | Is required | Default Value | Description             | Other Info |
| ----------- | ------ | -------- | ------ | ---------------- | -------- |
| code        | number | yes     |        |                  |          |
| msg         | string | yes     |        |                  |          |
| data        | object | yes     |        |                  |          |
| ├─pageNum   | number | yes     |        | Current Page           |          |
| ├─pageSize  | number | yes     |        | Page size           |          |
| ├─total     | number | yes     |        | Total records       |          |
| ├─pages     | number | yes     |        | pages             |          |
| ├─list      | object | yes     |        | List of data             |          |
| ├──userId   | number | yes     |        | User Id           |          |
| ├──matchAmt | number | yes     |        | User's Total match amount |          |


### Inquire Child Assets Snapshots



- Basic Info

**Path：** /api/v1/broker/queryAssetSnapshots

**Method：** GET

**Description：**

- Request Parameter

**Headers**

**Query**

| Parameter Name | Is required | Example                              | Description                                                                                                                                                                                                            |
| -------- | -------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | Yes       | filter=%7b%22currencyId%22%3a1%7d |  Value is URLEcode, conversion to upper case is required after coding，e.g.：%3a to %3A,result value is {"currencyId":1}, parameter： currencyId(required currency ID), pageNum(not required current page，default：1), pageSize(not required page size，default：1000，take 1000 when it is over) |

- Return Data

| Name                | Type   | Is required | Default Value | Description                                           | Other Info |
| ------------------- | ------ | -------- | ------ | ---------------------------------------------- | -------- |
| code                | number | yes     |        |                                                |          |
| msg                 | string | yes     |        |                                                |          |
| data                | object | yes     |        |                                                |          |
| ├─pageNum           | number | yes     |        | current page                                         |          |
| ├─pageSize          | number | yes     |        | page size                                         |          |
| ├─total             | number | yes     |        | total records                                    |          |
| ├─pages             | number | yes     |        | pages                                           |          |
| ├─list              | object | yes     |        | list of data                                           |          |
| ├──userId           | number | yes     |        | user ID                                         |          |
| ├──currencyId       | number | yes     |        | currency ID                                         |          |
| ├──totalMoney       | number | yes     |        | total assets                                        |          |
| ├──orderFrozenMoney | number | yes     |        | Order Frozen capital                                  |          |
| ├──closeProfitLoss  | number | yes     |        | Closed profit and loss                                     |          |
| ├──startDate        | number | yes     |        | Start date，the latest updated date，will be updated on 23：45：00 every day |          |
| ├──endDate          | number | yes     |        | End date，9999-12-13                          |          |

NOTE: 资产 = totalMoney + closeProfitLoss
	      
## Business Message Queue

  - How to access
    <br>Message Queue is mainly used to store the notification message generated during the future contract transaction. Matching is required between the configuration of message queue and exchange.
</br>
   - details
   Message format：
     ```
      {
          "message_type": 6001,
          "id": 154804153000,
          "account_id": 24,
          "contract_id": 100,
          "side": 1,
          "margin_rate": "50000000000000000",
          "trigger_type": 1
      }
      // message_type Message Type，6001：Notification
      //  id           The timestamp that this message generated（millisecond）, could be used in reduce duplication
      //  account_id   Child account ID
      //  contract_id  contract ID, 0:Cross，>0:specific
      //  side         trading side，1：bid，-1：ask
      //  margin_rate  Margin rate，margin rate，the value is expanded to 10^18，it needs to be reduced on the business partner's application/system.
      //  trigger_type Type，1：Alert， 2：Liquidated，3：Deleveraging, 4: Deleveraging opponent
      ``` 
   - Notification Message Details
   Lending Message format：
     ```
      {
        "message_type":"1001",          #Message ID
        "id":125,                       #Lending ID,
        "user_id":1,                    #User ID
        "currency_id":1,                #Currency ID
        "lending_num":200,              #Lending
        "time":154804153000             #time, millisecond
      }
     ```
   - Notification Message Details  
    Lending forced-deleverageing message format：
  
     ```
      {
        "message_type":"1002"        #Message ID
        "id":125,                    #Lending ID,
        "user_id":1,                 #User ID
        "currency_id":1,             #Currency ID
        "coin":200.090,              #Available capital
        "time":154804153000          #Time,millisecond
      }
     ```
  - Notification Message Details  
   Lending Alert Message format：
     ```
      {
        "message_type":"1003"         #Message ID
        "id":125,                     #Lending ID,
        "user_id":1,                  #User ID
        "currency_id":1,              #Currency ID
        "coin":200.090,               #Available capital
        "time":154804153000           #Time，millisecond
      }
     ```
   - Notification Message Details  
   Conditional Order Invoked Message format：
     ```
      {
	    "conditionOrderId": "101591010830537061",	#Condition order ID
	    "userId": 274,					#User ID
	    "contractId": 1000000,				#Contract ID
	    "quantity": 999999,				#Order quantity
	    "side": -1,					#Trading side，1:bid，-1：ask
	    "orderType": 3,					#Order Type，1-Limit；3-Market
	    "orderPrice": null,				#Order Price,	orderType：null	for market price
	    "positionEffect": 2,				#Position Effect，1 open ，2 close
	    "marginType": 1,				#Margin Type，1 Cross，2 Isolated
	    "marginRate": 0.0100000000,			#Margin rate,	Leverage=1/marginRate
	    "triggerType": 3,				#trigger Type，2：Market Price，3：Index price，4：Mark price
	    "curtPrice": 1000,				#Current price when ordering 
	    "triggerPrice": 10167.5,			#trigger price which is pre-set
	    "realTriggerPrice": 10168,			#real trigger price
	    "conditionOrderType": 1,			#0：Normal conditional order，1：take profit conditional order，2：Stop loss conditional order
	    "uuid": 11111,					#take profit & stop loos-united symbol
	    "code": 0,					#result code,0:succeed，others：failed
	    "orderId": "11591011422531540",			#order ID
	    "status": 2,					#status，1: not triggered ，2：order filled，3：order failed，4：cancelled
	    "updateTime": 1591087207078,			#update time
	    "createTime": 1591086948659			#create time
      }
     ```

   - Message Queue Configuration Description

      The following code is MQ consuming group,contact us if assistance is needed.
      
      ```java
        # Consumming group ID 
        # Liquidation Alert, Liquidation & Deleveraging Notification Message group-id: GID_broker_notice, Lending arrived、Lending Liquidation Alert、Lending Liquidation Notification Message group-id: GID_broker_lending, Conditional Order Notification Message group-id: GID_broker_order
        group-id: **********
        # Access Public key
        access-key: **********
        # Access Secret key
        secret-key: **********
        # URL
        namesrv-addr: **********
        #  Message model
        messageModel: CLUSTERING
        # Message topic
        topic: **********
        # Message Tag
        # Liquidation Alert, Liquidation & Deleveraging Notification Message tags: TG_broker_notice, Lending arrived、Lending Liquidation Alert、Lending Liquidation Notification Message tags: broker-lending-msg, Conditional Order Notification Message tags: TG_order_notice
        tags:**********
      ```
  
  - RocketMQ Sample
    - [RocketMQ Sample url](https://help.aliyun.com/product/29530.html?spm=a2c4g.11186623.6.540.6ff139c69dmBkV)
    - [java demo](https://github.com/ccfoxexchange/rocket-consumer-client)
    - [Notification Template](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/%E9%80%9A%E7%9F%A5%E6%A8%A1%E6%9D%BF.xlsx)

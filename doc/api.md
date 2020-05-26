- [API对接](#api%e5%af%b9%e6%8e%a5)
  - [签名](#%e7%ad%be%e5%90%8d)
    - [说明](#%e8%af%b4%e6%98%8e)
    - [申请创建 API Key](#%e7%94%b3%e8%af%b7%e5%88%9b%e5%bb%ba-api-key)
    - [签名步骤](#%e7%ad%be%e5%90%8d%e6%ad%a5%e9%aa%a4)
    - [签名demo](#%e7%ad%be%e5%90%8ddemo)
  - [基础接口](#%e5%9f%ba%e7%a1%80%e6%8e%a5%e5%8f%a3)
  - [资产权益](#%e5%9f%ba%e7%a1%80%e6%8e%a5%e5%8f%a3)
  - [API接口](#api%e6%8e%a5%e5%8f%a3)
    - [子账号注册](#%e5%ad%90%e8%b4%a6%e5%8f%b7%e6%b3%a8%e5%86%8c)
    - [子账号登录](#%e5%ad%90%e8%b4%a6%e5%8f%b7%e7%99%bb%e5%bd%95)
    - [查询子账户期货历史委托](#%e6%9f%a5%e8%af%a2%e5%ad%90%e8%b4%a6%e6%88%b7%e6%9c%9f%e8%b4%a7%e5%8e%86%e5%8f%b2%e5%a7%94%e6%89%98)
    - [查询子账户期货历史成交](#%e6%9f%a5%e8%af%a2%e5%ad%90%e8%b4%a6%e6%88%b7%e6%9c%9f%e8%b4%a7%e5%8e%86%e5%8f%b2%e6%88%90%e4%ba%a4)
    - [查询转账状态](#%e6%9f%a5%e8%af%a2%e8%bd%ac%e8%b4%a6%e7%8a%b6%e6%80%81)
    - [资产查询](#%e8%b5%84%e4%ba%a7%e6%9f%a5%e8%af%a2)
    - [转账](#%e8%bd%ac%e8%b4%a6)
    - [查询子用户手续费](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e6%89%8b%e7%bb%ad%e8%b4%b9)
    - [查询子用户盈亏](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e7%9b%88%e4%ba%8f)
    - [查询子用户成交金额](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e6%88%90%e4%ba%a4%e9%87%91%e9%a2%9d)
    - [查询子用户合约资产快照](#%e6%9f%a5%e8%af%a2%e5%ad%90%e7%94%a8%e6%88%b7%e5%90%88%e7%ba%a6%e8%b5%84%e4%ba%a7%e5%bf%ab%e7%85%a7)
  - [B端mq对接](#b%e7%ab%afmq%e5%af%b9%e6%8e%a5)

# API对接

- 生产环境地址：请找ccfox运营人员获取生产环境接入地址，并向ccfox提供访问ip以添加白名单。
- 测试：使用生产环境的 模拟币种cusd完成测试

## 签名

### 说明
API 请求在通过 internet 传输的过程中极有可能被篡改，为了确保请求未被更改，除公共接口（基础信息，行情数据）外的私有接口均必须使用您的 API Key 做签名认证，以校验参数或参数值在传输途中是否发生了更改。

一个合法的请求由以下几部分组成：

* 方法请求地址：即访问服务器地址 xxx，比如xxx/api/v1/broker/queryAsset。
* API 访问密钥（AccessKeyId）：您申请的 API Key 中的 Access Key。
* 时间戳（Timestamp）：您发出请求的时间戳。如：1548311559。在查询请求中包含此值有助于防止第三方截取您的请求。校验一分钟以内合法
* 签名：签名计算得出的值，用于确保签名有效和未被篡改。

``` js
// 请求头包含
var headers = {
  'content-type' : 'application/json',
  'apiExpires': expires, //UNIX时间戳以秒为单位。 校验一分钟以内合法
  'apiKey': apiKey, // API 访问密钥（apiKey）
  'signature': signature // 签名
};

```

### 申请创建 API Key

通过crm系统获取B端 API Key

API Key 包括以下两部分

* `Access Key` API 访问密钥
* `Secret Key` 签名认证加密所使用的密钥

### 签名步骤

规范要计算签名的请求 因为使用 SHA256 进行签名计算时，使用不同内容计算得到的结果会完全不同。所以在进行签名计算前，请先对请求进行规范化处理。（GET和POST请求不同）
例：资产查询(GET)
`GET https://xxx.io/api/v1/broker/queryAsset?filter={applId：应用ID（默认为2）  queryUserId：用户ID  currencyId：币种ID}`

### 签名demo

1. [node.js](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/node.js)
2. [python](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/code.py)
3. [java](https://github.com/ccfoxexchange/ccfox_api/tree/master/java/)

## 基础接口

币种接口： https://apitest.ccfox.com/future/queryCommonInfo

- 基本信息

**Path：** /future/queryCommonInfo

**Method：** GET

- 请求参数

- 返回数据

| 名称                     | 类型      | 是否必须 | 默认值 | 备注                                                     | 其他信息          |
| ------------------------ | --------- | -------- | ------ | -------------------------------------------------------- | ----------------- |
| currencyList             | object [] | 非必须   |        | 币种list                                                 | item 类型: object |
| ├─ currencyId            | number    | 必须     |        | 币种id                                                   |                   |
| ├─ symbol                | string    | 必须     |        | 币种名称                                                 |                   |
| ├─ displayPrecision      | number    | 必须     |        | 页面显示位数                                             |                   |
| contractList             | object [] | 非必须   |        | 期货合约list                                             | item 类型: object |
| ├─ varietyId             | number    | 必须     |        | 品种ID                                                   |                   |
| ├─ applId                | number    | 必须     |        | 1:现货，2：期货                                          |                   |
| ├─ symbol                | string    | 必须     |        | 品种名称                                                 |                   |
| ├─ priceTick             | number    | 必须     |        | 最小报价单位                                             |                   |
| ├─ lotSize               | number    | 必须     |        | 最小交易单位                                             |                   |
| ├─ takerFeeRatio         | number    | 必须     |        | Taker手续费率                                            |                   |
| ├─ makerFeeRatio         | number    | 必须     |        | Maker手续费率                                            |                   |
| ├─ limitMaxLevel         | number    | 必须     |        | 限价委托最大成交档位                                     |                   |
| ├─ marketMaxLevel        | number    | 必须     |        | 市价委托最大成交档位                                     |                   |
| ├─ maxNumOrders          | number    | 必须     |        | 用户最大挂单笔数                                         |                   |
| ├─ priceLimitRate        | number    | 必须     |        | 涨跌停率                                                 |                   |
| ├─ commodityId           | number    | 必须     |        | 商品币种ID                                               |                   |
| ├─ currencyId            | number    | 必须     |        | 货币币种ID                                               |                   |
| ├─ contractType          | number    | 必须     |        | 合约类型，1定期，2永续                                   |                   |
| ├─ deliveryType          | number    | 必须     |        | 交割类型，1现金交割，2实物交割                           |                   |
| ├─ deliveryPeriod        | number    | 必须     |        | 交割周期，1日2周3月                                      |                   |
| ├─ contractSide          | number    | 必须     |        | 合约方向，1正向，2反向                                   |                   |
| ├─ contractUnit          | number    | 必须     |        | 合约单位                                                 |                   |
| ├─ posiLimit             | number    | 必须     |        | 持仓限额                                                 |                   |
| ├─ orderLimit            | number    | 必须     |        | 委托限额                                                 |                   |
| ├─ impactMarginNotional  | number    | 必须     |        | 保证金影响额                                             |                   |
| ├─ fairBasisInterval     | number    | 必须     |        | 结算价基差计算间隔，单位秒                               |                   |
| ├─ clearPriceInterval    | number    | 必须     |        | 结算价计算间隔，单位秒                                   |                   |
| ├─ deliveryPriceInterval | number    | 必须     |        | 交割价计算间隔                                           |                   |
| ├─ minMaintainRate       | number    | 必须     |        | 最小维持保证金率                                         |                   |
| ├─ createTime            | number    | 必须     |        | 创建时间                                                 |                   |
| ├─ enabled               | number    | 必须     |        | 是否启用                                                 |                   |
| ├─ contract_status       | number    | 必须     |        | 1集合竞价，2连续交易，3交易暂停，4已摘牌，5未上市'       |                   |
| ├─ futureContractList    | object [] | 必须     |        |                                                          | item 类型: object |
| ├─ contractId            | number    | 必须     |        | 合约Id                                                   |                   |
| ├─ applId                | number    | 必须     |        | 应用标识                                                 |                   |
| ├─ symbol                | string    | 必须     |        | 合约名称                                                 |                   |
| ├─ priceTick             | number    | 必须     |        | 最小报价单位                                             |                   |
| ├─ lotSize               | number    | 必须     |        | 最小交易量单位                                           |                   |
| ├─ takerFeeRatio         | number    | 必须     |        | Taker手续费率                                            |                   |
| ├─ makerFeeRatio         | number    | 必须     |        | Maker手续费率                                            |                   |
| ├─ limitMaxLevel         | number    | 必须     |        | 限价委托最大成交档位                                     |                   |
| ├─ marketMaxLevel        | number    | 必须     |        | 市价委托最大成交档位                                     |                   |
| ├─ maxNumOrders          | number    | 必须     |        | 用户最大挂单笔数                                         |                   |
| ├─ priceLimitRate        | number    | 必须     |        | 涨跌停率                                                 |                   |
| ├─ commodityId           | number    | 必须     |        | 商品币种ID                                               |                   |
| ├─ currencyId            | number    | 必须     |        | 货币币种ID                                               |                   |
| ├─ contractStatus        | number    | 必须     |        | 交易对状态:1集合竞价,2连续交易,3交易暂停,4已摘牌,5未上市 |                   |
| ├─ listPrice             | number    | 必须     |        | 挂牌价格                                                 |                   |
| ├─ listTime              | number    | 必须     |        | 上市时间                                                 |                   |
| ├─ createTime            | number    | 必须     |        | 创建时间                                                 |                   |
| ├─ contractType          | number    | 必须     |        | 合约类型，1定期，2永续                                   |                   |
| ├─ deliveryType          | number    | 必须     |        | 交割类型，1现金交割，2实物交割                           |                   |
| ├─ deliveryPeriod        | number    | 必须     |        | 交割周期，0永续1日2周3月                                 |                   |
| ├─ contractSide          | number    | 必须     |        | 合约方向，1正向，2反向                                   |                   |
| ├─ contractUnit          | number    | 必须     |        | 合约单位                                                 |                   |
| ├─ lastTradeTime         | number    | 必须     |        | 最后交易时间                                             |                   |
| ├─ deliveryTime          | number    | 必须     |        | 最后交割时间                                             |                   |
| ├─ posiLimit             | number    | 必须     |        | 持仓限额                                                 |                   |
| ├─ orderLimit            | number    | 必须     |        | 委托限额                                                 |                   |
| ├─ impactMarginNotional  | number    | 必须     |        | 保证金影响额                                             |                   |
| ├─ minMaintainRate       | number    | 必须     |        | 最小维持保证金率                                         |                   |
| ├─ fairBasisInterval     | number    | 必须     |        | 结算价基差计算间隔                                       |                   |
| ├─ clearPriceInterval    | number    | 必须     |        | 结算价计算间隔                                           |                   |
| ├─ deliveryPriceInterval | number    | 必须     |        | 交割价计算间隔                                           |                   |
| ├─ varietyId             | number    | 必须     |        | 品种ID，标的ID                                           |                   |

## 资产权益

币种接口： https://apitest.ccfox.com/futureAsset/queryAccountEquity

- 基本信息

**Path：** /futureAsset/queryAccountEquity

**Method：** GET

**接口描述：**

### 请求参数
### Query：

| 参数名称 | 是否必须 | 示例                      | 备注                                                         |
| :------- | :------- | :------------------------ | :----------------------------------------------------------- |
| legalSymbol   | 是       | CNY | 法币标识，默认：CNY|

### 返回数据

| 名称                     | 类型      | 是否必须 | 默认值 | 备注                                                     | 其他信息          |
| ------------------------ | --------- | -------- | ------ | -------------------------------------------------------- | ----------------- |
| code             | number     | 必须   |        | 0：成功，其他失败 | |
| msg              | string     | 必须   |        | 消息 | |
| data             | object []  | 必须   |        | 数据集合 | |
| ├─accountType    | number     | 必须   |        | 账户类型，1现货，2期货，5提币，10活动 | 合约云用户只有：期货账户：2|
| ├─totalAccountEquity    | number     | 必须   ||账户总权益，即btc估值 | |
| ├─simulationAvailableMargin    | number     | 必须   ||模拟币可用保证金 | |
| ├─totalLegalValue    | object     | 必须   ||法币价值 | |
| ├├─value    | number     | 必须   ||总法币价值 | |
| ├├─symbol    | string     | 必须   ||法币标识 | |
| ├─currencyAssetList    | object     | 必须   ||货币资产列表 | |
| ├├─currencyId    | number     | 必须   ||货币ID | |
| ├├─accountEquity    | number     | 必须   ||该币种账户权益 | |
| ├├─floatProfitLoss    | number     | 必须   ||浮动盈亏 | |
| ├├─availableMargin    | number     | 必须   ||可用保证金 | |
| ├├─initMargin    | number     | 必须   ||持仓占用保证金 | |
| ├├─currentCloseProfitLoss    | number     | 必须   ||当日已实现盈亏 | |
| ├├─totalLeverageLevel    | number     | 必须   ||整体杠杆水平 | |
| ├├─frozenForTrade    | number     | 必须   ||委托占用保证金，现货也有该字段 | |
| ├├─available    | number     | 必须   ||可用余额，除期货现货外 | |
| ├├─locked    | number     | 必须   ||锁定金额，除期货、现货外 | |
| ├├─btcValuation    | number     | 必须   ||btc估值，除期货外账户有该字段 | |
| ├├─totalBalance    | number     | 必须   ||账户余额，现货 | |




## API接口 

### 子账号注册

- 基本信息

**Path：** /api/v1/broker/registerSon

**Method：** POST

**接口描述：**

校验：
    签名校验，apikey 权限校验，用户组是否属于券商校验，子账号重复提交注册校验，是否已注册校验，注册上线阈值校验

- 请求参数

**Headers**

| 参数名称     | 参数值                                                           | 是否必须 | 示例                                                             | 备注                                                                         |
| ------------ | ---------------------------------------------------------------- | -------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Content-Type | application/json                                                 | 是       |                                                                  |                                                                              |
| signature    | 1dec55ac42478858d322cf841f22522bb47752bbd5d330adbe84db9cf5854733 | 是       | 1dec55ac42478858d322cf841f22522bb47752bbd5d330adbe84db9cf5854733 | 签名，生成方式：sha256对请求body参数加密（对外生成示例文档询问技术人员索要） |
| apiKey       | 61ba5fc3-2384-472e-8712-e5f83b358815                             | 是       | 61ba5fc3-2384-472e-8712-e5f83b358815                             | API访问密钥：每个券商对应接口的标识（券商申请的apiKey）                      |
| apiExpires   | 1561708654079                                                    | 是       | 1561708654079                                                    | API此次访问过期时间（时间戳：毫秒）                                          |
| UNIQUE       | XXXX                                                             | 是       |                                                                  | uuid, 每次申请都需不一样的值,用于防重复提交                                  |

**Body**

| 名称        | 类型   | 是否必须 | 默认值 | 备注                         | 其他信息 |
| ----------- | ------ | -------- | ------ | ---------------------------- | -------- |
| groupId     | number | 必须     |        | 分组ID                       |          |
| areaCode    | string | 非必须   |        | 区号   示例：+86             |          |
| sonUserName | string | 必须     |        | 账号名称（限定15位纯数字）   |          |
| sonPassword | string | 必须     |        | 密码：MD5加密32位小写        |          |
| phone       | string | 非必须   |        | 手机号：传了手机号区号必须传 |          |
| email       | string | 非必须   |        | 邮箱                         |          |

- 返回数据

| 名称 | 类型   | 是否必须 | 默认值 | 备注                           | 其他信息 |
| ---- | ------ | -------- | ------ | ------------------------------ | -------- |
| code | number | 必须     |        | 返回code（0：成功  非0：失败） |          |
| msg  | string | 必须     |        | 返回消息                       |          |
| data | null   | 必须     |        |                                |          |

### 子账号登录



- 基本信息

**Path：** /api/v1/broker/loginSon

**Method：** POST

**接口描述：**

- 请求参数

**Headers**

**Body**

| 名称        | 类型   | 是否必须 | 默认值 | 备注                      | 其他信息 |
| ----------- | ------ | -------- | ------ | ------------------------- | -------- |
| sonUserName | string | 必须     |        | 账号名称                  |          |
| sonPassword | string | 必须     |        | 账号密码：MD5加密32位小写 |          |

- 返回数据

| 名称             | 类型   | 是否必须 | 默认值 | 备注                           | 其他信息 |
| ---------------- | ------ | -------- | ------ | ------------------------------ | -------- |
| code             | number | 必须     |        | 返回code（0：成功  非0：失败） |          |
| msg              | string | 必须     |        | 返回消息                       |          |
| data             | object | 必须     |        |                                |          |
| ├─ access_token  | string | 必须     |        | token                          |          |
| ├─ token_type    | string | 必须     |        | token类型                      |          |
| ├─ refresh_token | string | 必须     |        | 刷新token                      |          |
| ├─ expires_in    | string | 必须     |        | 有效时间：毫秒为单位           |          |
| ├─ scope         | string | 必须     |        |                                |          |
| ├─ userId        | number | 必须     |        | 用户id                         |          |

### 查询子账户期货历史委托



- 基本信息

**Path：** /api/v1/broker/queryHisOrder

**Method：** GET

**接口描述：**

- 请求参数
- Query：

| 参数名称 | 是否必须 | 示例                      | 备注                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| :------- | :------- | :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filter   | 是       | %7B%22userId%22%3A+275%7D | 注意：     请求参数filter=%7B%22userId%22%3A275%7D，值为URLEcode编码， 编码后 用大写 比如 %3a 需要 转换成 %3A     解码值为：{"userId":275}     其中             userId（int）：用户ID，                                            必须             contractId（int）：交易对ID，                                  非必须             side（int）：上下页数,                                               非必须             startDate（long）：开始时间戳,                                非必须             endDate（long）：结束时间戳,                                 非必须             pageNum（int）：当前页数,                                      非必须             pageSize（int）：页显示数量,                                    非必须 |




- 返回数据

| 名称                | 类型      | 是否必须 | 默认值 | 备注                                                                                                                                                            | 其他信息          |
| ------------------- | --------- | -------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| code                | number    | 非必须   |        |                                                                                                                                                                 |                   |
| msg                 | string    | 非必须   |        |                                                                                                                                                                 |                   |
| data                | object [] | 非必须   |        |                                                                                                                                                                 | item 类型: object |
| ├─ applId           | number    | 必须     |        | 2：期货                                                                                                                                                         |                   |
| ├─ timestamp        | number    | 必须     |        | 委托时间                                                                                                                                                        |                   |
| ├─ userId           | number    | 必须     |        | 用户ID                                                                                                                                                          |                   |
| ├─ contractId       | number    | 必须     |        | 交易对ID                                                                                                                                                        |                   |
| ├─ uuid             | string    | 必须     |        | 委托编号                                                                                                                                                        |                   |
| ├─ side             | number    | 必须     |        | 买卖方向，1买，-1卖                                                                                                                                             |                   |
| ├─ price            | string    | 必须     |        | 委托价格                                                                                                                                                        |                   |
| ├─ quantity         | string    | 必须     |        | 委托数量                                                                                                                                                        |                   |
| ├─ orderType        | number    | 必须     |        | 订单委托类型，1（限价），3（市价）                                                                                                                              |                   |
| ├─ orderSubType     | number    | 必须     |        | 委托子类型 0（默认值），1（被动委托），2（最近价触发条件委托），3（指数触发条件委托），4（标记价触发条件委托）                                                  |                   |
| ├─ timeInForce      | number    | 必须     |        | 订单有效时期类型：1（取消前有效），2（立即成交剩余撤销，未启用），3（全部成交否则撤销，未启用），4（五档成交剩余撤销，未启用），5（五档成交剩余转限价，未启用） |                   |
| ├─ minimalQuantity  | string    | 必须     |        | 最小成交量                                                                                                                                                      |                   |
| ├─ stopPrice        | string    | 必须     |        | 止损止盈价                                                                                                                                                      |                   |
| ├─ stopCondition    | number    | 必须     |        | 止损止盈标志 1（止盈，未启用），2（止损，未启用），3（只减仓，未启用）                                                                                          |                   |
| ├─ orderStatus      | number    | 必须     |        | 委托状态 0:未申报,1:正在申报,2:已申报未成交,3:部分成交,4:全部成交,5:部分撤单, 6:全部撤单7:撤单中,8:失效,11:缓存高于条件的委托,12:缓存低于条件的委托             |                   |
| ├─ makerFeeRatio    | string    | 必须     |        | maker 手续费率                                                                                                                                                  |                   |
| ├─ takerFeeRatio    | string    | 必须     |        | taker 手续费率                                                                                                                                                  |                   |
| ├─ clOrderId        | string    | 必须     |        | 客户订单编号                                                                                                                                                    |                   |
| ├─ filledCurrency   | string    | 必须     |        | 成交金额                                                                                                                                                        |                   |
| ├─ filledQuantity   | string    | 必须     |        | 成交数量                                                                                                                                                        |                   |
| ├─ canceledQuantity | string    | 必须     |        | 撤单数量                                                                                                                                                        |                   |
| ├─ matchTime        | number    | 必须     |        | 成交时间                                                                                                                                                        |                   |
| ├─ positionEffect   | number    | 必须     |        | 开平标志，1开仓2平仓                                                                                                                                            |                   |
| ├─ marginType       | number    | 必须     |        | 保证金类型，1全仓，2逐仓                                                                                                                                        |                   |
| ├─ marginRate       | string    | 必须     |        | 保证金率（倒数即为杠杠倍数）                                                                                                                                    |                   |
| ├─ fcOrderId        | string    | 必须     |        | 强平委托号，非空时为强平委托                                                                                                                                    |                   |
| ├─ deltaPrice       | string    | 必须     |        | 标记价与委托价之差                                                                                                                                              |                   |
| ├─ frozenPrice      | string    | 必须     |        | 资金计算价格                                                                                                                                                    |                   |

### 查询子账户期货历史成交



- 基本信息

**Path：** /api/v1/broker/queryHisMatch

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

- Query：

| 参数名称 | 是否必须 | 示例                      | 备注                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| :------- | :------- | :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| filter   | 是       | %7B%22userId%22%3A+275%7D | 注意：     请求参数filter=%7B%22userId%22%3A275%7D，值为URLEcode编码， 编码后 用大写 比如 %3a 需要 转换成 %3A     解码值为：{"userId":275}     其中             userId（int）：用户ID，                                            必须             contractId（int）：交易对ID，                                  非必须             side（int）：上下页数,                                               非必须             startDate（long）：开始时间戳,                                非必须             endDate（long）：结束时间戳,                                 非必须             pageNum（int）：当前页数,                                      非必须             pageSize（int）：页显示数量,                                    非必须 |




- 返回数据

| 名称                 | 类型      | 是否必须 | 默认值 | 备注                                                                   | 其他信息          |
| -------------------- | --------- | -------- | ------ | ---------------------------------------------------------------------- | ----------------- |
| code                 | number    | 非必须   |        |                                                                        |                   |
| msg                  | string    | 非必须   |        |                                                                        |                   |
| data                 | object [] | 非必须   |        |                                                                        | item 类型: object |
| ├─ applId            | number    | 必须     |        | 2：期货                                                                |                   |
| ├─ matchTime         | number    | 必须     |        | 成交时间                                                               |                   |
| ├─ contractId        | number    | 必须     |        | 交易对ID                                                               |                   |
| ├─ execId            | string    | 必须     |        | 成交号                                                                 |                   |
| ├─ bidUserId         | number    | 必须     |        | 买方用户ID                                                             |                   |
| ├─ askUserId         | number    | 必须     |        | 卖方用户ID                                                             |                   |
| ├─ bidOrderId        | string    | 必须     |        | 买方委托号                                                             |                   |
| ├─ askOrderId        | string    | 必须     |        | 卖方委托号                                                             |                   |
| ├─ matchPrice        | string    | 必须     |        | 成交价                                                                 |                   |
| ├─ matchQty          | string    | 必须     |        | 成交数量                                                               |                   |
| ├─ matchAmt          | string    | 必须     |        | 成交金额                                                               |                   |
| ├─ bidFee            | string    | 必须     |        | 买方手续费                                                             |                   |
| ├─ askFee            | string    | 必须     |        | 卖方手续费                                                             |                   |
| ├─ takerSide         | number    | 必须     |        | 订单成交方向 1买，-1卖                                                 |                   |
| ├─ updateTime        | number    | 必须     |        | 最近更新时间                                                           |                   |
| ├─ bidPositionEffect | number    | 必须     |        | 买方开平标志：1开仓2平仓                                               |                   |
| ├─ askPositionEffect | number    | 必须     |        | 卖方开平标志：1开仓2平仓                                               |                   |
| ├─ bidMarginType     | number    | 必须     |        | 买方保证金类型：1全仓，2逐仓                                           |                   |
| ├─ askMarginType     | number    | 必须     |        | 卖方保证金类型：1全仓，2逐仓                                           |                   |
| ├─ bidInitRate       | string    | 必须     |        | 买方初始保证金率                                                       |                   |
| ├─ askInitRate       | string    | 必须     |        | 卖方初始保证金率                                                       |                   |
| ├─ bidMatchType      | number    | 必须     |        | 买方成交类型：0普通成交1强平成交2强减成交（破产方）3强减成交（盈利方） |                   |
| ├─ askMatchType      | number    | 必须     |        | 卖方成交类型：0普通成交1强平成交2强减成交（破产方）3强减成交（盈利方） |                   |
| ├─ bidPnlType        | number    | 必须     |        | 买方盈亏类型：0正常成交1正常平仓2强平3强减                             |                   |
| ├─ bidPnl            | number    | 必须     |        | 买方平仓盈亏                                                           |                   |
| ├─ askPnlType        | number    | 必须     |        | 卖方盈亏类型：0正常成交1正常平仓2强平3强减                             |                   |
| ├─ askPnl            | number    | 必须     |        | 卖方平仓盈亏                                                           |                   |

### 查询转账状态



- 基本信息

**Path：** /api/v1/broker/queryTransferStatus

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

**Query**

| 参数名称 | 是否必须 | 示例                                            | 备注                                                                                                                                                                                                                                      |
| -------- | -------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | {"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"} | 注意： 请求参数filter={"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"}，值为URLEcode编码， 编码后 用大写 比如 : 需要 转换成 : 解码值为：{"clientId":"31eb1f4b37204cc19bb1177cc5833cb3"} 其中 clientId（String）：接口请求方转账单号， 必须 |

- 返回数据

| 名称             | 类型   | 是否必须 | 默认值 | 备注                                                                                                                 | 其他信息 |
| ---------------- | ------ | -------- | ------ | -------------------------------------------------------------------------------------------------------------------- | -------- |
| code             | number | 必须     |        |                                                                                                                      |          |
| msg              | string | 必须     |        |                                                                                                                      |          |
| data             | object | 必须     |        |                                                                                                                      |          |
| ├─clientId       | string | 必须     |        | 转账单号                                                                                                             |          |
| ├─transferStatus | number | 必须     |        | 转账状态：10000, "转账成功",     10001, "转账失败",     10002, "转账中",     10003, "审核中",     10004, "审核失败"; |          |

### 资产查询



- 基本信息

**Path：** /api/v1/broker/queryAsset

**Method：** GET

**接口描述：**

- 请求参数

| 参数名称 | 是否必须 | 示例                                            | 备注                                                                                                                                                                                                                                                                                                                              |
| -------- | -------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | {"applId":5,"queryUserId":"129","currencyId":1} | 注意： 请求参数filter={"applId":5,"queryUserId":"129","currencyId":1}，值为URLEcode编码， 编码后 用大写 比如 : 需要 转换成 : 解码值为：{"applId":5,"queryUserId":"129","currencyId":1} 其中 applId（int）：5：我的钱包，2：期货钱包， 必须 queryUserId（int）：用户ID, 查询此用户ID的余额， 必须 currencyId（int）：货币ID 非必须 |

- 返回数据

| 名称                | 类型      | 是否必须 | 默认值 | 备注           | 其他信息          |
| ------------------- | --------- | -------- | ------ | -------------- | ----------------- |
| code                | number    | 非必须   |        |                |                   |
| msg                 | string    | 非必须   |        |                |                   |
| data                | object [] | 非必须   |        |                | item 类型: object |
| ├─ currencyId       | number    | 必须     |        | 币种ID         |                   |
| ├─ available        | number    | 必须     |        | 可用金额       |                   |
| ├─ totalBalance     | string    | 必须     |        | 总资产         |                   |
| ├─ frozenForTrade   | string    | 必须     |        | 冻结资产       |                   |
| ├─ initMargin       | string    | 必须     |        | 已占用保证金   |                   |
| ├─ frozenInitMargin | string    | 必须     |        | 委托冻结保证金 |                   |
| ├─ closeProfitLoss  | string    | 必须     |        | 已实现盈亏     |                   |

### 转账



- 基本信息

**Path：** /api/v1/broker/transfer

**Method：** POST

**接口描述：**

校验：
    签名校验，apikey 权限校验，数量校验，有效货币校验，有效应用ID校验（applID），转入转出用户属于券商校验，钱包余额与转账金额校验（只校验：5我的钱包）

- 请求参数

**Headers**

**Body**

| 名称       | 类型   | 是否必须 | 默认值 | 备注                                           | 其他信息 |
| ---------- | ------ | -------- | ------ | ---------------------------------------------- | -------- |
| fromUserId | number | 必须     |        | 来源用户ID                                     |          |
| fromApplId | number | 必须     |        | 来源应用：默认传2                              |          |
| toUserId   | string | 必须     |        | 接收用户ID                                     |          |
| toApplId   | number | 必须     |        | 接收应用：默认传2                              |          |
| currencyId | number | 必须     |        | 币种ID                                         |          |
| quantity   | string | 必须     |        | 转账数量：限制小数点后六位,cusd最小划转数：0.1 |          |
| clientId   | string | 必须     |        | 转账单号：UUID生成唯一标识，32位数字和字母组成 |          |

- 返回数据

| 名称 | 类型   | 是否必须 | 默认值 | 备注                                   | 其他信息 |
| ---- | ------ | -------- | ------ | -------------------------------------- | -------- |
| code | number | 必须     |        | 返回code（0：成功  非0：失败）         |          |
| msg  | string | 必须     |        | 返回消息                               |          |
| data | object | 必须     |        | 转账单号（用于查询转账状态clientId值） |          |


### 查询子用户手续费



- 基本信息

**Path：** /api/v1/broker/queryDayFees

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

**Query**

| 参数名称 | 是否必须 | 示例                                                                | 备注                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | filter=%7B%22currencyId%22%3A7%2C%22statDate%22%3A%2220200109%22%7D | 值为URLEcode编码， 编码后用大写，如：%3a 需要转换成 %3A,解码值为：{"currencyId":7,"statDate":"20200109"},参数如下所示：currencyId(必传   货币ID),statDate(必传   查询日期，如：20200109),pageNum(非必 当前页，默认：1),pageSize(非必  页条数，默认：1000，大于1000时取1000) |

- 返回数据

| 名称        | 类型   | 是否必须 | 默认值 | 备注       | 其他信息 |
| ----------- | ------ | -------- | ------ | ---------- | -------- |
| code        | number | 必须     |        |            |          |
| msg         | string | 必须     |        |            |          |
| data        | object | 必须     |        |            |          |
| ├─pageNum   | number | 必须     |        | 当前页     |          |
| ├─pageSize  | number | 必须     |        | 页条数     |          |
| ├─total     | number | 必须     |        | 总记录条数 |          |
| ├─pages     | number | 必须     |        | 页数       |          |
| ├─list      | object | 必须     |        | 数据       |          |
| ├──userId   | number | 必须     |        | 用户ID     |          |
| ├──totalFee | number | 必须     |        | 累计手续费 |          |


### 查询子用户盈亏



- 基本信息

**Path：** /api/v1/broker/queryDayProfits

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

**Query**

| 参数名称 | 是否必须 | 示例                                                                | 备注                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | filter=%7B%22currencyId%22%3A7%2C%22statDate%22%3A%2220200109%22%7D | 值为URLEcode编码， 编码后用大写，如：%3a 需要转换成 %3A,解码值为：{"currencyId":7,"statDate":"20200109"},参数如下所示：currencyId(必传   货币ID),statDate(必传   查询日期，如：20200109),pageNum(非必 当前页，默认：1),pageSize(非必  页条数，默认：1000，大于1000时取1000) |

- 返回数据

| 名称           | 类型   | 是否必须 | 默认值 | 备注           | 其他信息 |
| -------------- | ------ | -------- | ------ | -------------- | -------- |
| code           | number | 必须     |        |                |          |
| msg            | string | 必须     |        |                |          |
| data           | object | 必须     |        |                |          |
| ├─pageNum      | number | 必须     |        | 当前页         |          |
| ├─pageSize     | number | 必须     |        | 页条数         |          |
| ├─total        | number | 必须     |        | 总记录条数     |          |
| ├─pages        | number | 必须     |        | 页数           |          |
| ├─list         | object | 必须     |        | 数据           |          |
| ├──userId      | number | 必须     |        | 用户ID         |          |
| ├──totalProfit | number | 必须     |        | 用户当前总盈亏 |          |


### 查询子用户成交金额



- 基本信息

**Path：** /api/v1/broker/queryDayMatchAmts

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

**Query**

| 参数名称 | 是否必须 | 示例                                                                | 备注                                                                                                                                                                                                                                                                        |
| -------- | -------- | ------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | filter=%7B%22currencyId%22%3A7%2C%22statDate%22%3A%2220200109%22%7D | 值为URLEcode编码， 编码后用大写，如：%3a 需要转换成 %3A,解码值为：{"currencyId":7,"statDate":"20200109"},参数如下所示：currencyId(必传   货币ID),statDate(必传   查询日期，如：20200109),pageNum(非必 当前页，默认：1),pageSize(非必  页条数，默认：1000，大于1000时取1000) |

- 返回数据

| 名称        | 类型   | 是否必须 | 默认值 | 备注             | 其他信息 |
| ----------- | ------ | -------- | ------ | ---------------- | -------- |
| code        | number | 必须     |        |                  |          |
| msg         | string | 必须     |        |                  |          |
| data        | object | 必须     |        |                  |          |
| ├─pageNum   | number | 必须     |        | 当前页           |          |
| ├─pageSize  | number | 必须     |        | 页条数           |          |
| ├─total     | number | 必须     |        | 总记录条数       |          |
| ├─pages     | number | 必须     |        | 页数             |          |
| ├─list      | object | 必须     |        | 数据             |          |
| ├──userId   | number | 必须     |        | 用户ID           |          |
| ├──matchAmt | number | 必须     |        | 用户当前总成交额 |          |


### 查询子用户合约资产快照



- 基本信息

**Path：** /api/v1/broker/queryAssetSnapshots

**Method：** GET

**接口描述：**

- 请求参数

**Headers**

**Query**

| 参数名称 | 是否必须 | 示例                              | 备注                                                                                                                                                                                                            |
| -------- | -------- | --------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| filter   | 是       | filter=%7b%22currencyId%22%3a1%7d | 值为URLEcode编码， 编码后用大写，如：%3a 需要转换成 %3A,解码值为：{"currencyId":1},参数如下所示：currencyId(非必传   货币ID),pageNum(非必 当前页，默认：1),pageSize(非必  页条数，默认：1000，大于1000时取1000) |

- 返回数据

| 名称                | 类型   | 是否必须 | 默认值 | 备注                                           | 其他信息 |
| ------------------- | ------ | -------- | ------ | ---------------------------------------------- | -------- |
| code                | number | 必须     |        |                                                |          |
| msg                 | string | 必须     |        |                                                |          |
| data                | object | 必须     |        |                                                |          |
| ├─pageNum           | number | 必须     |        | 当前页                                         |          |
| ├─pageSize          | number | 必须     |        | 页条数                                         |          |
| ├─total             | number | 必须     |        | 总记录条数                                     |          |
| ├─pages             | number | 必须     |        | 页数                                           |          |
| ├─list              | object | 必须     |        | 数据                                           |          |
| ├──userId           | number | 必须     |        | 用户ID                                         |          |
| ├──currencyId       | number | 必须     |        | 货币ID                                         |          |
| ├──totalMoney       | number | 必须     |        | 总资产                                         |          |
| ├──orderFrozenMoney | number | 必须     |        | 委托冻结资产                                   |          |
| ├──closeProfitLoss  | number | 必须     |        | 已实现盈亏                                     |          |
| ├──startDate        | number | 必须     |        | 开始时间，最近更新日期时间，每天23：45：00更新 |          |
| ├──endDate          | number | 必须     |        | 结束日期，9999-12-13                           |          |


​	      
## B端mq对接

  - 说明
    <br>消息队列MQ主要存放B端子账号进行期货交易时产生的通知类消息。B端在接入时，MQ对应的配置需与交易所对接。</br>
   - 通知类消息说明
   强平强减强平预警消息格式：
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
      // message_type 消息类型，6001：通知类消息
      //  id           产生该消息时的时间戳（微秒），可用于业务去重
      //  account_id   子用户ID
      //  contract_id  合约ID, 0:全仓，>0:特定合约
      //  side         买卖方向，1：买，-1：卖
      //  margin_rate  保证金率，值扩大了10^18次方，B端在使用时需缩小
      //  trigger_type 类型，1：告警，2：强平，3：强减, 4: 强减对手方
      ``` 
   - 通知类消息说明
   配资到账消息格式：
     ```
      {
        "message_type":"1001",          #消息ID
        "id":125,                       #配资ID,
        "user_id":1,                    #用户ID
        "currency_id":1,                #配资货币ID
        "lending_num":200,              #配资数量
        "time":154804153000             #时间，毫秒
      }
     ```
   - 通知类消息说明  
    配资强平消息格式：
  
     ```
      {
        "message_type":"1002"        #消息ID
        "id":125,                    #配资ID,
        "user_id":1,                 #用户ID
        "currency_id":1,             #配资货币
        "coin":200.090,              #当前余额
        "time":154804153000          #时间,毫秒
      }
     ```
  - 通知类消息说明 
  配资预警消息格式：
  
     ```
      {
        "message_type":"1003"         #消息ID
        "id":125,                     #配资ID,
        "user_id":1,                  #用户ID
        "currency_id":1,              #配资货币
        "coin":200.090,               #当前余额
        "time":154804153000           #时间，毫秒
      }
     ```

   - MQ配置说明

      以下是MQ消费配置展示列,如果需要，联系我们
      
      ```java
        # 消费组ID 
        # 强平预警强平强减通知类消息group-id: GID_broker_notice, 配资到账、配资强平预警、配资强平通知类消息group-id: GID_broker_lending
        group-id: **********
        # 访问公钥匙
        access-key: **********
        # 访问私钥匙
        secret-key: **********
        # 访问url地址
        namesrv-addr: **********
        # 访问模式
        messageModel: CLUSTERING
        # 消息主题
        topic: **********
        # 消息标签
        # 强平预警强平强减通知类消息tags: TG_broker_notice, 配资到账、配资强平预警、配资强平通知类消息tags: broker-lending-msg
        tags:**********
      ```
  
  - RocketMQ参考
    - [RocketMQ参考url](https://help.aliyun.com/product/29530.html?spm=a2c4g.11186623.6.540.6ff139c69dmBkV)
    - [java对接demo](https://github.com/ccfoxexchange/rocket-consumer-client)
    - [通知模板参考](https://github.com/ccfoxexchange/ccfox-cloud-api/blob/master/%E9%80%9A%E7%9F%A5%E6%A8%A1%E6%9D%BF.xlsx)

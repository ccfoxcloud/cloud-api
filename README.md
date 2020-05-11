# **_欢迎来到 ccofx 合约云项目，[线上 Demo](https://mcloud.ccfox.com)_**

只需一周，拥有专属于您的合约交易功能

您只需要对接少量接口，即可快速接入，仅需 1 周！

=========== 欢迎联系我们：==================

**_微信：13066998399 （加好友请注明 b 端合作）_**

**_邮箱：sonic@ccfox.com_**

============== thank =====================

## 合约云 B 端对接说明

如果您符合以下其中一种或多种条件，您可以 B 端对接端方式接入合约云，快速部署属于您的衍生品合约交易系统。

- 数字资产钱包
- 币币交易所
- 集成了 USDT/ETH/BTC 钱包的项目

通过接入合约云，您将获得：

- 高比例后端手续费分成，您的流量变现最佳手段
- 极致合约交易体验， 兼容 okex + bitmex 方案
- 优化风控极致，客活率 up up up!
- 前期培训+持续运营支持，保障您的成功！

## 对接流程

合约云 B 端对接十分简单，仅需您完成以下几步即可。

1. 在 **您的系统** 中每个用户准备单独的合约交易账户，并 准备一个资金划转功能
   - 用户的资金可以在 普通账户 和 合约交易账户 之间互相划转。
2. 在**您的系统**准备一个为用户开通合约交易的确认功能，并：
   - 让用户勾同意选我们准备好的 合约用户协议
   - 用户提交开通确认后，通过 [子账户注册](#####子账号注册)接口为该用户在 **我们的系统** 注册账户，该账户是 **您 B 端母账户** 下的子账户。
3. 在 **您的系统** 中准备资金划转功能
   - 用户的资金可以在 普通账户 和 合约交易账户 之间互相划转
   - 当用户在 **您的系统** 中，由普通账户向 **合约交易账户** 转入资金时，您通过调用我们系统的[转账接口](#####转账)，将 B 端母账户的资金转入到 **用户的子账户**中。
   - 区块链转账时间比较长，为了提升用户体验，我们建议您在母账户中预存部分资金。
   - 您需要定期平衡 **您的网站中的资金** 和 **您在我们这边的 B 端母账户资金** ， 您可能会需要用到 [资产查询接口](#####资产查询) 查询 您的 B 端母账户或者您旗下的子账户的资产情况
4. 将我们准备好的 **交易页面前端**嵌入到您的网站中
   - 用户进入**交易页面前端**时，调用 [登陆接口](#####子账号登录)为用户在 我们的系统 登陆
5. 用户的有关消息（强平/强减）将通过 mq 推送，需要您根据[B 端 mq 对接](#####B端mq对接)自行对接去给您的用户发送消。项目里提供参考 [通知模板](通知模板.csv)

## 对接示意图

![对接示意图](https://assets.processon.com/chart_image/5c1c5704e4b0b71ee503e019.png)

## 如何对接

- [前端对接](https://github.com/ccfox-com/cloud)
- [服务端对接](./doc/api.md)
- [通知模板](通知模板.csv)

## [QA](./doc/QA.md)

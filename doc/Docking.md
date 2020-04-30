# 合约云 H5 2.0

## 1.根据需求修改配置文件 [.env.cloud](../.env.cloud)

### 项目配置文件说明

| key                     |  value  | 说明                                                                          |
| :---------------------- | :-----: | :---------------------------------------------------------------------------- |
| REACT_APP_API_URL       | String  | 接口请求 baseUrl                                                              |
| REACT_APP_WS_URL        | String  | ws 链接的 url                                                                 |
| REACT_APP_MOCKSYMBOL    | String  | 模拟币名称                                                                    |
| REACT_APP_BASENAME      | String  | 项目 basename,合约云用户可忽略此配置                                          |
| REACT_APP_INJECT_COOKIE | Boolean | 值为 true 使用 Cookie 注入所需字段；值为 false 使用 LocalStorage 注入所需字段 |
| REACT_APP_JSBRIDGE      | Boolean | 是否启用 jsBridge，true 为启用                                                |
| REACT_APP_ISCLOUD       | Boolean | true 为合约云用户                                                             |

## 2.对接流程

- android,ios 通过 webview 嵌入合约云

1. 根据配置中选择 Cookie 或者 localStorage 注入方式，向 webview 注入所需数据 (注意 cookie 域)

|       key       |           value           | 说明                                  |
| :-------------: | :-----------------------: | :------------------------------------ |
| \_authorization |            --             | 用户登陆 token（合约云系统的 token,） |
|     \_lang      | (zh_CN,en_US,zh_TW,.....) | 语言                                  |
|  \_upDownColor  |            0/1            | 0:绿涨红跌;1:红涨绿跌                 |

2. 原生使用 jsBridge，实现 app 与 h5 交互

## jsBridge

### 事件约定

- login (登陆)
- transfer (划转)
- deposit (充值入金)
- h5_href 需要跳转的二级页面（ex:从交易页面到历史委托），参数：h5 路径(ex:/records)，在上层打开一个新的 webView，将参数拼接在 baseUrl 上
- h5_back 从二级页面返回交易页面，关闭上层 webView(ex:从行情详情页面返回交 易页面),参数(null || string),有参数需要将参数通过 backToTrade 事件发送给 h5
- 每次进入 h5（首次进入或者从原生其他地方返回再进入，ex:划转完，回来交易页),向 h5 发送 backToH5 事件，用来拉取数据

### 具体功能事件列表

|   事件名    |      参数       |  方向   | 说明                                |
| :---------: | :-------------: | :-----: | :---------------------------------- |
|    login    |      null       | h5->app | 登陆                                |
|  transfer   |      null       | h5->app | 资产划转                            |
|    share    |     Object      | h5->app | 持仓分享,[参数详情](./SharePosi.md) |
|   deposit   |      null       | h5->app | 充值入金                            |
|   h5_href   |     string      | h5->app | 跳转二级页面                        |
|   h5_back   | null/JsonString | h5->app | 返回交易页面                        |
| backToTrade |   JsonString    | app->h5 | 接收到 h5_back 的参数，发送给 h5    |
|  backToH5   |  string(随意)   | app->h5 | 每次进入 h5                         |

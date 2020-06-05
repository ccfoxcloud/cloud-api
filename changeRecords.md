*用其他语言阅读: [English](changeRecords.en.md)*

### V1.0.0 20191105
* h5使用react.js构建
* pc使用vue.js 构建

### V1.1.0 20200604
* 升级新版本条件单
    * 条件单使用2.0版本后端接口，h5已完成集成，版本：[h5 v1.1.0](https://github.com/ccfox-com/cloud/releases)
    * 旧版本pc不支持本次更新，新版本pc代码会集成。
    * 2.0版本条件单新增触发后短信通知功能，通过消息队列传递，合约云商户可自行对接给客户发送提醒短信。
    * mq消息格式
       条件委托触发消息格式(详见[服务端对接文档](doc/api.md))：
     ```
      {
	    "conditionOrderId": "101591010830537061",	#条件委托ID
	    "userId": 274,					#用户ID
	    "contractId": 1000000,				#合约ID
	    "quantity": 999999,				#委托数量
	    "side": -1,					#买卖方向，1:买，-1：卖
	    "orderType": 3,					#委托类型，1-限价；3-市价
	    "orderPrice": null,				#委托价,	orderType：市价时为null		
	    "positionEffect": 2,				#开平标志，1开仓，2平仓
	    "marginType": 1,				#保证金类型，1全仓，2逐仓
	    "marginRate": 0.0100000000,			#保证金率,	杠杆倍数=1/marginRate
	    "triggerType": 3,				#触发类型，2：成交价，3：指数价，4：标记价
	    "curtPrice": 1000,				#下单时当前价
	    "triggerPrice": 10167.5,			#预设触发价格
	    "realTriggerPrice": 10168,			#真实触发价格
	    "conditionOrderType": 1,			#0：普通条件单，1：止盈条件单，2：止损条件单
	    "uuid": 11111,					#止盈止损-联合标识
	    "code": 0,					#执行结果码,0:下单成功，非0：失败
	    "orderId": "11591011422531540",			#委托ID
	    "status": 2,					#状态，1: 未触发，2：下单成功，3：下单失败，4：撤销
	    "updateTime": 1591087207078,			#更新时间
	    "createTime": 1591086948659			#创建时间
      }
     ```
    * 短信模板供参考：[条件单短信模版](条件单短信模板.xlsx)

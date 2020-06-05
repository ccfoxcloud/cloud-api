*Read this in other languages: [简体中文](changeRecord.md)*

### V1.0.0 20191105
* h5 is based on react.js 
* pc is based on vue.js 

### V1.1.0 20200604
* new updates:
  * Conditional Order
    * Conditional order now is using the version 2.0 Back-end API，integration is finished on H5，Version：[h5 v1.1.0](https://github.com/ccfox-com/cloud/releases)
    * The old version of PC would not support this update，the PC code will be integrated in new version.
    * The SMS notification after condition activated is added in the 2.0 Conditional order send by Message Queue，and the cloud contract business parters could do self-customizing and send notification to the client.
    * mq format
       Conditional Order Activate Message Format(further details are in the [Server-Side API Document](doc/api.en.md))：
     ```
      {
	    "conditionOrderId": "101591010830537061",	#Conditional Order ID
	    "userId": 274,					#User ID
	    "contractId": 1000000,				#Contract ID
	    "quantity": 999999,				#Order amount
	    "side": -1,					#Trading Direction，1:Buy/Long，-1：Sell/Short
	    "orderType": 3,					#Order Type，1-Limit Price；3-Market Price
	    "orderPrice": null,				#Order Price,	orderType：null	when it is market price	
	    "positionEffect": 2,				#Position Effect，1 Open，2 Close
	    "marginType": 1,				#Margin Type，1 Cross，2 Isolated
	    "marginRate": 0.0100000000,			#Margin rate,	Leverage=1/marginRate
	    "triggerType": 3,				#Trigger Type，2：Deal Price，3：Index Price，4：Mark Price
	    "curtPrice": 1000,				#The price when make the order
	    "triggerPrice": 10167.5,			# Trigger Price which is pre-set
	    "realTriggerPrice": 10168,			#Real Trigger Price
	    "conditionOrderType": 1,			#0：Normal，1：Take Profit，2：Stop Loss
	    "uuid": 11111,					#Take profit & stop loss-United signal
	    "code": 0,					#Result code,0:succeed，非0：failed
	    "orderId": "11591011422531540",			#Order ID
	    "status": 2,					#Status，1: not actived，2：Order filled，3：Order failed，4：Cancel
	    "updateTime": 1591087207078,			#Update time
	    "createTime": 1591086948659			#Create time
      }
     ```
    * SMS template：[Conditional Order SMS Template](条件单短信模板.xlsx)

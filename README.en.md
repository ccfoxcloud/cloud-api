*Read this in other languages: [简体中文](README.md)*

# **_Welcome to ccfox Cloud Contract project，[Online Demo](https://mcloud.ccfox.com)_**

Your exclusive contract trading function only requiring few API，
fast deployment that only cost one week!

=========== Contact Us：==================

**_Wechat：13066998399 （Please indicate that you are the business parter）_**

**_Email：sonic@ccfox.com_**

**_official site：[www.ccfox.com](https://www.ccfox.com)_**

============== thank =====================

## Cloud Contract Business Partner Instruction

If you match one or multiple following conditions, using CCFOX Cloud API for Business purpose is recommended for you to fastly deploy your derivatives trading system.

- Digital assets wallet
- Crypto currency exchange
- Project integrated with USDT/ETH/BTC wallet

What will bring to you:

- High proportion fee sharing from back end, the best way to convert your traffic into real money
- High quality contract trading experience, compatible with Okex and Bitmex.
- High level risk control, User Retention Ratios could keep increasing!
- Pre-training plus constant support from us will help you to be successful！


## Process

Before using our cloud API, few steps need to be taken:

1. A transfer function and individual contract transaction account for each registered user are required in **your system/application**.
   - Make sure funds could be transferred between normal account and contract transaction account.
2. A confirmation function in **your system/application** for user to allow the contract trading, then:
   - After the confirmation of user agreement is received, helping this user to register a new account in **our system** by using the [Sub-account registration](https://github.com/ccfox-com/cloud-api/blob/master/doc/api.en.md#sub-account-registration) API. This new registered account will be a child account under your **parent business account**.
3. A fund transfer function in **your application/system**.
   - Fund transfer could be done between normal account and contract transaction account.
   - When a user transfers the fund from a normal account to a **contract transaction account** in **your system/application**, the [transferring API](https://github.com/ccfox-com/cloud-api/blob/master/doc/api.en.md#transferring%20API) will be invoked in our system for transferring the fund from your parent business account to the user's child account.
   - Pre-deposit is recommended for a better user experience due to long loading time of blockchain transactions.
   - Regular fund balancing between **your application/system** and **your parent business account** would be necessary. Therefore, the [assets checking API](https://github.com/ccfox-com/cloud-api/blob/master/doc/api.en.md#assets%20checking%20API) might be useful to check your parent business account and those child accounts under your parent account.
4. Embed our front-end trading page into **your system/application** we prepared for you.
   - Invoke the API of [Login function](https://github.com/ccfox-com/cloud-api/blob/master/doc/api.en.md#Login%20function) for users to login in **our system/application** when users access the **front-end trading page**.
5. The user-related notification (e.g. deleveraging or liquidated) will be pushed by MQ(message queue), thus you may need to adjust your system/application to adapt the configuration of the [business MQ](https://github.com/ccfox-com/cloud-api/blob/master/doc/api.md#business%20MQ).The template is provided in the project [notificationTemplate](通知模板.csv)

## Demograph

![demograph](https://assets.processon.com/chart_image/5c1c5704e4b0b71ee503e019.png)

## how to deploy

- [Front-end](https://github.com/ccfox-com/cloud/blob/master/README.en.md)(Private Repository in GitHub，submit application for permission first)
- [Back-end](./doc/api.en.md)
- [NotificationTemplate](通知模板.csv)

**_NOTE:_**

[Front-end](https://github.com/ccfox-com/cloud) which is the GitHub private repository，Registration is required if there is no github account, then submit the application(mainly about the github account) to ccfox expert to apply the authorization，ccfox experts will finish the process asap then authorise your github account to visit and download the code.

## Tips for Contract list configuration

1. Confirm the required contract list, then give the feedback to ccfox
2. Ask CCFOX for the business partner symbol which is required in the code.
3. Configuring and deploying the business partner symbol in the front-end project in .env.cloud file 'REACT_APP_BROKERUUID'
4. If there are some changes in the contract list in future, please contact CCFOX

## QA

1.  Q: Is there a whitelist for limit transfer api?
    A: The whitelist will be included in production, testing period 

2.  Q: Cloud API page is used on all the front ends，For example web first request  xxx api has received and saved user accesstoken at front end, so how to ensure that both app token and access token would not expire or what is the solution for project xxx that one or both of those tokens expired.
    A: Once expired，redirection to xxx-login-pagefor re-login is required，then back to the trading page.


3.  Q: Would mcloud.ccfox.com be able to be isolated and run in the browser?
    A: This project has to use with app，one single browser would not have place for landing，therefore there will be no respond；JavaScript will send event after click the function button, and app will catch and compute then interact with next step.


4.  Q:Does the parent account be able to do the trading?
    A:Considering the convenience of reconciliation, the parent account would not be able to do the trading, we will close the trading on the parent account.

5.  Q: Would prepaid cards are supported in your API? Is the customization available on changing the proportion between prepaid card and fee.
    A: Self prepaid cards and credits are welcome to develop in your own system/application, we could provide the data of fee standards, and we would take the entire fee then the rest will be returned later to your account.  


6.  Q：How to figure out the business partner's invitation and commission system。
    A：Customized development on this section is welcome according to the self-demand, we CCFOX will provide the API for each partner.


7.  Q：How to change the mock currency symbol?
    A：In the front-end project .env.cloud, configuring 'REACT_APP_MOCKSYMBOL=< Symbol>'

8.  Q：how to add the project entrance (eg:/contract)
    A：in the front-end porject .env.cloud, configuring 'PUBLIC_URL=/contract,REACT_APP_BASENAME=/contract'
       in nginx_cloud.conf, there are two places
       `location / {
            try_files $uri $uri/ /index.html;
       }`
       add location in the following script
       `localtion /contract {
           index index.html;
           try_files $uri $uri/ /contract/index.html;
        }`
   

## Update Records
[ChangeLog](changeRecords.en.md)

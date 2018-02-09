
* [FAQ](#faq)
* [Error handling](#error-handling)

## FAQ
### Where should I start to integrate Smart-ID to my e-service?
In case you are using Java or PHP development frameworks, the easiest way to do the integration is to use our "free to use" client libraries. When you are using some other development framework, please read our API description. The Smart-ID integration API is based on JSON/REST and it’s easy to use from all the major nowadays development frameworks. For development purposes we have a publicly available demo environment, you don’t need any agreement for implementing Smart-ID support. For accessing the production service you should have an agreement. More information about service agreements and prices is available [here](https://www.smart-id.com/e-service-providers/).

### How long does it usually take to implement Smart-ID support?
We have got feedback from developers and e-service providers that implementing Smart-ID authentication is easy and usually it takes one to two days. The signing process is more complicated and the amount of development work needed depends on your current implementation. In case there is document signing based on ID-card already in place, adding the Smart-ID support should take up to one week.

### Is there a possibility to get support during Smart-ID integration?
Yes, we are happy to provide you free support for integrating the Smart-ID. Please contact us via support@sk.ee.

### What are the “ServiceName” and “RPUUID” parameters and what kind of values should I use there?
These are unique identifiers of the e-service provider. The e-service provider name is displayed to the end-user in Smart-ID app during authentication and signing. The service provider should inform SK of the ServiceName that will be used. The RPUUID is service internal parameter that is provided by SK to the e-service provider. Both parameters are fixed in agreement between e-service provider and SK ID Solutions.
 
### What are the options for implementing automated tests for the Smart-ID integration?
For making automated integration testing easier there are Smart-ID auto-responder test accounts https://github.com/SK-EID/smart-id-documentation/wiki/Environment-technical-parameters#test-accounts-for-automated-testing . These accounts are returning pre-defined status codes and responses according to specifications. This means that you don’t have to automate Smart-ID app side during the testing, this is already done by SK.

### Is the app-2-app integration available?
Yes and no. You cannot call the Smart-ID app directly from some other application. You have to do the server side integration in a way that your app is calling your server and your server is making RP API call to the Smart-ID platform. The reason for that is our security and business model.
Still we have in our 2018 Q4 roadmap some developments that will make app-2-app integration smoother (especially in iOS platform).

## Error handling
### How to handle error 471 (No suitable account of requested type found, but user has some other accounts)?
User is having Smart-ID Basic account which can only be used in Swedbank's and SEB's internet bank.

Following message should be displayed:

> You are using a Smart-ID Basic account which can only be used in Swedbank's and SEB's internet bank.
> Please upgrade your Smart-ID account. You should create a new account electornically using your ID-card or by visiting Swedbanks' or SEB's bank office. 

> See Smart-ID FAQ for more information. 
> https://www.smart-id.com/help/faq/ 

> Different registration methods: 
> https://www.smart-id.com/help/faq/#registering-585 (ENG) 

> Requirements for ID-card registration: 
> https://www.smart-id.com/help/faq/#requirements-for-id-card-registration-1509 (ENG) 

> Smart-ID registration in bank office:
> https://www.smart-id.com/help/faq/using-smart-id/actions-can-smart-id-customer-service-points/
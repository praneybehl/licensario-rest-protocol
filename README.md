Licensario REST API
===================

How Licensario works
--------------------
Licensario handles the provisioning of your users entitlements. So instead of integrating with a payment gateway
and then managing users entitlements on your own, you can leave it to Licensario.
The only thing you will need to do - is ask Licensario at feature entry point if the user has an active entitlement to
use this feature. If the answer is *yes* - you just proceed with the standard flow, if the answer is *not* - you 
proceed with the JS API flow.

Authentication
--------------
In order to request Licensario for information, you will need to authenticate your request using your credentials.
You can find these in the "API Credentials" page in the publishers site (https://publishers.licensario.com).

Authenticating your requests requires adding the **ISV_API_KEY** and **ISV_API_SECRET** headers to each request

How Licensario Identifies your users
------------------------------------
So how Licensario identifies users?

Licensario can work in 2 modes:
  1. Manage the users by itself (Licensario users)
  2. Authenticate users through OAuth2 protocol (External users)

In the 1st flow users are identified using Licensario User Id. Most queries for Licensario users will start with
/api/v1/users/*user-id*

    GET /api/v1/users/*user-id*

In the 2nd flow users will be identified using your unique user id, and the queries will start with
/api/v1/users/external/*your-user-id*

    GET /api/v1/users/external/*your-user-id*
    
The API
=======
Ensure that an **external user** exists in Licensario database:

    PUT /api/v1/users/external/*your-user-id*
    
    email=user@example.com

Query user licenses
-------------------
Return all active user licenses for specified feature/features.
If featureIds not specified - returns all user licenses for ISV features.

Query parameters
  - **FeatureIds** [optional] - comma-separated list of feature ids

If no licenses found - a **NOT_FOUND (404)** HTTP code returns

    GET /api/v1/users/*user-id*/licenses?featureIds=CREATE_TODO123,  
    
or

    GET /api/v1/users/external/*your-user-id*/licenses
    
Create a license for user
-------------------------
Sometimes you'll need to issue a license for a user programmatically - either as a bonus to a customer, 
either as part of a marketing campaign with a really complicated logic.


For these cases use the following request. The license is issued according to terms of the specified payment plan.

POST parameters:
  - **paymentPlanId** - the id of the payment plan to use for the license

    POST /api/v1/users/*user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
    
or

    POST /api/v1/users/external/*your-user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
    
Response:
license xml, example:
```xml
  <licenseCertificate id="1" userId="2" paymentPlanId="3" issueDateUTC="201205041632" isTrial="true|false">
    <features>
      <feature id="3" totalAmount="100" amountUsed="30"/>
      <feature id="5" amountUsed="1000" />
    </features>
  </licenseCertificate>
```

Note: the resulting xml contains feature usage and allocations. For unlimited feature allocations, the totalAmount is omitted.
    
Ensure that a license exists for user
-------------------------------------
Often you'll want to offer a free tier for your customers and thus will want to ensure that a free license exists for him.

Use this request to achieve this. The parameters and the output are the same as in the previous request.

Some

    PUT /api/v1/users/*user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
    
or

    PUT /api/v1/users/external/*your-user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN

Get feature allocation for user
-------------------------------
When a user wants to use a certain licensed feature (we recommend that you license all your features - even the free ones),
call this method to ensure that the user has the entitlement to use it.

REST Parameters:
  - **feature-id** - the feature whose allocation is in question

    GET /api/v1/users/*user-id*/features/*feature-id*/alloc
    
or

    GET /api/v1/users/external/*your-user-id*/features/*feature-id*/alloc

Reporting feature usage
=======================
Once the user consumed some of your resources or used a feature of your product, you'll need to inform Licensario 
about this fact for the provisioning purpose.

Licensario provides 2 ways of reporting the usage:
  - **Increment** - increment users usage of feature *X* by *N*
  - **Set** - set usage of feature *X* to *N*

You will usually use the **set** strategy to report usage that changes very quickly (for example disk usage, RAM usage, etc).
In other situations you will use the **increment** strategy as it is more convenient.


Set feature allocation for user
----------------------------------
Set usage of feature **feature-id** to **amount**.

    PUT /api/v1/users/*user-id*/features/*feature-id/alloc
    
    amount=10.0

or

    PUT /api/v1/users/external/*external_user-id*/features/*feature-id/alloc
    
    amount=10.0

Increment feature allocation for user
-------------------------------------
Increment usage of feature **feature-id** by **amount**.

Note: **amount** can be negative

    POST /api/v1/users/*user-id*/features/*feature-id/alloc
    
    amount=10.0

or

    POST /api/v1/users/external/*external_user-id*/features/*feature-id/alloc
    
    amount=10.0

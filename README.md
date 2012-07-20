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

    GET /api/v1/users/*user-id*/licenses  
    
or

    GET /api/v1/users/external/*your-user-id*/licenses
    
Create a license for user
-------------------------

    POST /api/v1/users/*user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
    
or

    POST /api/v1/users/external/*your-user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
    
Ensure that a license exists for user
-------------------------------------

    PUT /api/v1/users/*user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN
or

    PUT /api/v1/users/external/*your-user-id*/licenses
    
    paymentPlanId=PREMIUM_PLAN

Get feature allocation for user
-------------------------------

    GET /api/v1/users/*user-id*/features/*feature-id*/alloc
    
or
    GET /api/v1/users/external/*your-user-id*/features/*feature-id*/alloc
    

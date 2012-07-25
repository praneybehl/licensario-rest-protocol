# Licensario REST API

## What is Licensario
Licensario is an online service that handles the provisioning and management of your users entitlements, thus saving 
you the trouble of manually managing them and having to integrate directly with a payment gateway. It's quite simple: 
whenever an user requests access to a feature of your product you can use our REST API to check if he's authorized to 
use it. If he is not authorized you can control how to proceed by, say, offering a plan upgrade that instantly grants 
access to that feature **without disrupting the user experience**. 

Licensario's API has a RESTful architecture and can be used by any programming language, framework or tool that has a HTTP 
library, such as jQuery, Ruby, Java, C#, .NET, etc.

## Authentication
All requests to our API need to by authenticated by adding the the **ISV_API_KEY** and **ISV_API_SECRET** headers 
to each request. You can retrieve your API credentials in your [publisher's page](https://publishers.licensario.com).

## Your Users
We offer two ways of identifying your users:

1. **Storing their information at Licensario** (*licensario users*): your Users receive an internal **user_id** and their 
information are stored in our servers. You can retrieve it or change it any time through our API.

        BASE_URL = /api/v1/users/:user_id

2. **Authenticating external users via the OAuth2 Protocol** (*external users*): you can store your User information yourself and 
only provide an **external_user_id**, which identifies them in our system.

        BASE_URL = /api/v1/users/external/:external_user_id

## Actions

###Get a list of Licenses

    GET BASE_URL/licenses

* **Description**: Retrieve the **Licenses** for a given **User**.
* **Parameters**: none

2. **Create a License**
    * **Description**: Create a **License** for a given **User**.
    * **Parameters**: *paymentPlanId*

    POST BASE_URL/licenses


3. **Ensure that a License exists**
4. **Read Feature's Allocation**
5. **Update a Feature's Allocation**
6. **Increment a Feature's Allocation**




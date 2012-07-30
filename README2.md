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

        $BASE_URL = /api/v1/users/:user_id

2. **Authenticating external users via the OAuth2 Protocol** (*external users*): you can store your User information yourself and 
only provide an **external_user_id**, which identifies them in our system.

        $BASE_URL = /api/v1/users/external/:external_user_id

## Actions

### Get a list of Licenses

    GET $BASE_URL/licenses

* **Description**: Retrieve the licenses for a given user.
* **Parameters**:
    * *featureIds*: IDs of the Features.
    * *paymentPlanIds*: IDs of the Payment Plans.
* **Examples**:
    * Request:

        ```xml
          <test>
          </test>
        ```

    * Response:

        ```xml
          <test>
          </test>
        ```

### Create a License

    POST $BASE_URL/licenses

* **Description**: Create a license for a given user.
* **Parameters**:
    * *paymentPlanId*: ID of the Payment Plan.

### Ensure that a License exists

    PUT $BASE_URL/licenses

* **Description**: Ensure that a given user has the necessary license.
* **Parameters**:
    * *paymentPlanId*: ID of the payment plan selected by the user

### Read Feature's Allocation

    GET $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Retrieves the amount available (i.e. allocation) of a given feature to a given user.
* **Parameters**:
    * *feature_id*: ID of the Feature.

### Update a Feature's Allocation

    PUT $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Updates the amount available (i.e. allocation) of a given feature to a given user.
* **Parameters**:
    * *feature_id*: ID of the feature
    * *amount*: The new available amount of the Feature. Ex: *25*, *0.2*, *12.38*, etc.

### Increment a Feature's Allocation

    POST $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Increments the amount available (i.e. allocation) of a given feature to a given user by a defined amount.
* **Parameters**:
    * *feature_id*: ID of the Feature.
    * *amount*: The increment / decrement to the available amount of the Feature. Ex: *30*, *-0.5*, *15.75*, etc.

### Ensure the existence of an External User

    PUT $BASE_URL

* **Description**: Ensure that an External User exists. If it doesn't it will be created.
* **Parameters**:
    * *email*: email of the External User.


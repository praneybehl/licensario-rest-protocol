# Licensario REST API

## What is Licensario
Licensario is an online service that handles the provisioning and management of your users entitlements, thus saving 
you the trouble of manually managing them and having to integrate directly with a payment gateway. It's quite simple: 
whenever an user requests access to a feature of your product you can use our REST API to check if he's authorized to 
use it. If he is not authorized you can control how to proceed by, say, offering a plan upgrade that instantly grants 
access to that feature **without disrupting the user experience**. 

Licensario's API has a RESTful architecture and can be used by any programming language, framework or tool that has a HTTP 
library, such as Ruby, Java, C#, .NET, etc.

## Authentication
All requests to our API need to by authenticated by adding the the **ISV_API_KEY** and **ISV_API_SECRET** headers 
to each request. You can retrieve your API credentials in your [publisher's page](https://publishers.licensario.com).

##Conventions
All dates are specified in the following format: yyyymmddHHMMSS, where:

* **yyyy** - the year, including century
* **mm** - month (2 digit)
* **dd** - day (2 digit)
* **HH** - hour (2 digit)
* **MM** - minutes (2 digit)
* **SS** - seconds (2 digit)

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
* **Example**:

    ```
    GET /api/v1/users/external/1/licenses?featureIds=MANAGE_TOD5533de505b&paymentPlanIds=FREE_PLANca1b8f4ead
    ```

    ```xml
    <?xml version="1.0"?>
    <userLicenses>
      <licenseCertificate licenseId="56" userId="121" paymentPlanId="FREE_PLANca1b8f4ead" issueDateUTC="20120719173522" is_trial="true">
        <features>
          <feature id="MANAGE_TOD5533de505b" totalAmount="100.0" amountUsed="2.0"/>
        </features>
      </licenseCertificate>
      <licenseCertificate licenseId="16" userId="121" paymentPlanId="FREE_PLANca1b8f4ead" issueDateUTC="20120712232223" expirationDateUTC="20120812232223" is_trial="true">
        <features>
          <feature id="MANAGE_TOD5533de505b" totalAmount="100.0" amountUsed="23.0"/>
        </features>
      </licenseCertificate>
    </userLicenses>
    ```

### Create a License

    POST $BASE_URL/licenses

* **Description**: Create a license for a given user.
* **Parameters**:
    * *paymentPlanId*: ID of the Payment Plan.
* **Example**:

    ```
    POST /api/v1/users/external/1/licenses
    PARAMETERS: {"paymentPlanId": "FREE_PLANca1b8f4ead"}
    ```

    ```xml
    <?xml version="1.0"?>
    <licenseCertificate licenseId="708" userId="121" paymentPlanId="FREE_PLANca1b8f4ead" issueDateUTC="20120730134848" expirationDateUTC="20120831000000" is_trial="true">
      <features>
        <feature id="MANAGE_TOD5533de505b" totalAmount="100.0" amountUsed="0.0"/>
      </features>
    </licenseCertificate>
    ```
    
### Cancel a license
* **Description**: Revokes a license, optionaly specifying the revokation reason
* **Parameters**:
    * *revokeReason* (optional): Revokation reason
* **Example**:

    ```
    DELETE /api/v1/licenses[?revokeReason=Some%20reason]
    ```
    
    ```
    HTTP 401 - Invalid request or unauthorized
    HTTP 404 - License not found
    HTTP 200 - License canceled
    ```

### Ensure that a License exists

    PUT $BASE_URL/licenses

* **Description**: Ensure that a given user has the necessary license.
* **Parameters**:
    * *paymentPlanId*: ID of the payment plan selected by the user
* **Example**:

    ```
    PUT /api/v1/users/external/1/licenses
    PARAMETERS: {"paymentPlanId": "FREE_PLANca1b8f4ead"}
    ```

    ```xml
    <?xml version="1.0"?>
    <licenseCertificate licenseId="16" userId="121" paymentPlanId="FREE_PLANca1b8f4ead" issueDateUTC="20120712232223" expirationDateUTC="20120812232223" is_trial="true">
      <features>
        <feature id="MANAGE_TOD5533de505b" totalAmount="100.0" amountUsed="24.0"/>
      </features>
    </licenseCertificate>
    ```

### Read Feature's Allocation

    GET $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Retrieves the amount available (i.e. allocation) of a given feature to a given user.
* **Parameters**:
    * *feature_id*: ID of the Feature.
* **Example**:

    ```
    GET /api/v1/users/external/1/features/MANAGE_TOD5533de505b/alloc?paymentPlanId=FREE_PLANca1b8f4ead
    ```

    ```json
    {
      "total":"1200.0",
      "used":"35.0",
      "available":1165.0
    }
    ```

### Update a Feature's Allocation

    PUT $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Updates the amount available (i.e. allocation) of a given feature to a given user.
* **Parameters**:
    * *feature_id*: ID of the feature
    * *amount*: The new available amount of the Feature. Ex: *25*, *0.2*, *12.38*, etc.
* **Example**:

    ```
    PUT /api/v1/users/external/1/features/MANAGE_TOD5533de505b/alloc
    {"amount": "1", "paymentPlanId": "FREE_PLANca1b8f4ead"}
    ```

    ```http
    HTTP/1.1 200 OK
    ```

### Increment a Feature's Allocation

    POST $BASE_URL/licenses/features/:feature_id/alloc

* **Description**: Increments the amount available (i.e. allocation) of a given feature to a given user by a defined amount.
* **Parameters**:
    * *feature_id*: ID of the Feature.
    * *amount*: The increment / decrement to the available amount of the Feature. Ex: *30*, *-0.5*, *15.75*, etc.
* **Example**:

    ```
    POST /api/v1/users/external/1/features/MANAGE_TOD5533de505b/alloc
    PARAMETERS: {"amount": "1", "paymentPlanId": "FREE_PLANca1b8f4ead"}
    ```

    ```http
    HTTP/1.1 200 OK
    ```

### Ensure the existence of an External User

    PUT $BASE_URL

* **Description**: Ensure that an External User exists. If it doesn't it will be created.
* **Parameters**:
    * *email*: email of the External User.
* **Example**:

    ```
    PUT /api/v1/users/external/1
    PARAMETERS: {:email: "some@user.net"}
    ```

    ```http
    HTTP/1.1 200 OK
    ```
### Revenue
* **Description**: Gets net revenue for a specific period (or for last month if not specified)
* **Parameters**:
    * *startDate*: start date
    * *endDate*: end date

* **Example**:

    ```
    GET /api/v1/publisher/balance[?startDate=20121110090807&endDate20111009080706]
    ```
    
    ```json
    {
        "balance": "99.99"
    }
    ```

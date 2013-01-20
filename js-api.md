# Licensario JS Component
The Licensario JS Component is a javascript library that is responsible for the seamless integration of the Licensing Wizard into your 
application. The Wizard is embedded in an iframe inside your page and it's use does not disturb your own javascript scripts.

# Installation
Simply include the following code into your page and you're good to go

```html
    <script type="text/javascript" src="https://users.licensario.com/assets/api/api.js"></script>
```

# Usage
## Server side preparation
To securely specify the user that will purchase the license, a server side call is required:

```
POST http://users.licensario.com/api/v1/wizards
```

This request accepts the following parameters:

<table>
    <tr>
        <td>externalUserId</td>
        <td>The user id as it was set in PUT /api/v1/users call</td>
    </tr>
</table>

and will return a plain token for further use on client side.

## Client side wizard launch
Call **licensario.getLicense** to run the wizard. You can customize the behaviour of the method with the following parameters:

* **featureIds**: an *Array* that contains the IDs of the Features you want to control the licensing for in this particular page.
* **allowedPaymentPlanIds**: an *Array* of the Payment Plans your user will be allowed to choose from.
* **paymentPlanId**: if you want to use a specific Payment Plan you can set this *String* attibute to the ID of that Plan. This will skip 
the Payment Plan selection step of the Wizard and it precludes the utilization of the *allowedPaymentPlanIds* setting.
* **licenseTag**: associates a custom unique ID with this License (can be a presentoon id, or presentoons state hash). *String*.
* **callbackUrl**: if your User desires to use a payment method that requires a redirect after the process is finished, such as PayPal, 
you can provide the URL that your users should be redirected to.
* **apiKey**: your Licensario's API Key, which you can retrieve by going to your [publisher's page](https://publishers.licensario.com).
* **successCallback**: the function that will be called if the User successfully completes the licensing process through the Wizard. The callback will receive a single parameter - the generated license.
* **failCallback**: this function will be called if the licensing process is cancelled by the User or otherwise fails.
* **wizardToken**: a token returned to you at server-side via POST /api/v1/wizards

# Example

```javascript
    licensario.getLicense({
      featureIds: ["TODO123", "SHOW_REPORT22"],
      allowedPaymentPlanIds: [1,2,3],
      licenseTag: "my-custom-unique-id",
      externalUserId: "2",
      callbackUrl: 'http://mysite.com/MY_CALLBACK_URL',
      apiKey: "db886331e9105fc19dc9fd6df2caebab9f112c3c81877ea3a3bfcfe3076aa77d",
      successCallback: function(license){
        alert('success!');
      },
      failCallback: function(){
        alert('failure :(');
      }
    });
```

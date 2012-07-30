# Licensario JS Component
The Licensario JS Component is a javascript library that is responsible for the seamless integration of the Licensing Wizard into your 
application. The Wizard is embedded in an iframe inside your page and it's use does not disturb your own javascript scripts.

# Installation
Simply include the following code into your page and you're good to go

```html
    <script type="text/javascript" src="https://users.licensario.com/assets/api/api.js"></script>
```

# Usage
Now that you've included the component in your page you have to initialize it. In order to do so, you will need to provide some important 
parameters, such as:

* **featureIds**: an *Array* that contains the IDs of the Features you want to control the licensing for in this particular page.
* **allowedPaymentPlanIds**: an *Array* of the Payment Plans your user will be allowed to choose from.
* **paymentPlanId**: if you want to use a specific Payment Plan you can set this *Integer* attibute to the ID of that Plan. This will skip 
the Payment Plan selection step of the Wizard and it precludes the utilization of the *allowedPaymentPlanIds* setting.
* **licenseTag**: associates a custom unique ID with this License (can be a presentoon id, or presentoons state hash). *String*.
* **externalUserId**: a *String* that represents the ID of the User in your (ISV's) system.
* **callbackUrl**: if your User desires to use a payment method that requires a redirect after the process is finished, such as PayPal, 
you can provide the URL that your users should be redirected to.
* **apiKey**: your Licensario's API Key, which you can retrieve by going to your [publisher's page](https://publishers.licensario.com).
* **successCallback**: the function that will be called if the User successfully completes the licensing process through the Wizard.
* **failCallback**: this function will be called if the licensing process is cancelled by the User or otherwise fails.

# Example

Licensario JS API
=================
Licensario JS API is responsible for displaying the licensing wizard to the user. The wizard is displayed in an iframe
and can be displayed inside your site.

Installation
------------

```html
	<script type="text/javascript" src="https://users.licensario.com/assets/api/api.js"></script>
```

Usage
-----

```javascript
	licensario.init({
		baseUrl: "http://users.licensario.net",
		initCompleted: function() {
			options = {
				// shows all payment plans that contain all the features in the array
				featureIds: ["TODO123", "SHOW_REPORT22"],
				// if you want to skip payment plans step and set payment plan 1 as chose
				paymentPlanId: 1,
				// if you want to set the payment plans list to choose from to payment plans 1, 2 & 3
				allowedPaymentPlanIds: [1,2,3],
				
				// Associates a custom unique id with this license (can be a presentoon id, or presentoons state hash)
				licenseTag: "my-custom-unique-id"
				
				// The user id in ISV system
				externalUserId: "2",
				
				// If a payment method that requires redirect is selected (i.e. PayPal) - 
				// the whole page is redirected to its site. Once the wizard is finished - 
				// the user is redirected to this url
				callbackUrl,
				
				// The API key from API Credentials screen
				apiKey: "db886331e9105fc19dc9fd6df2caebab9f112c3c81877ea3a3bfcfe3076aa77d",
				
				// This will be called when licensing succeeds
				successCallback: function() {
					alert("success!");
				},
				
				// This will be called when licensing is canceled or failed
				failCallback: function() {
					alert("failure :(");
				}
			}
		}
	});
```

# Integration

MonEFlow PAY offers two ways to integrate your website or ~~mobile app~~ with our payment platforms. 

The quickest is to integrate through our out-of-the-box SDKs for Web, ~~iOS and Android~~. Set up your server, add a few lines of code to your website ~~or app~~, and you're up and running.

If you want to use your own UI and you are a PCI SAQ-D merchant, use the Direct API integration. You can also use the API to collect your shopper's card data. 

Either way, your integration can accept all the payment methods offered by adyen, ~~and support 3D Secure.~~


## API integration

Build your own custom checkout experience using our API integration.

With our online payments API integration, you'll be able to accept different payment methods, while maintaining full control over the UI and user experience.

It is highly advisable to use one of our libraries to connect with the MonEFlow PAY APIs.

<aside class="notice">
Payment with full card details is not available to merchants by default. To use this endpoint, please contact your relationship manager.
</aside>

Are you sure the full card API is for you? Great! Here is the [request](#create-payment)..


## Client SDK

Client SDK is quick and easy to integrate, accepts online payments from all major credit cards, and is customizable to your brand.

The CLient SDK will embed the a frames payment form to your website and allow your customers to enter their payment details directly on your checkout page. From these Frames, your customers' sensitive information is processed by us and exchanged for a payment nonce. This process is called tokenization. After you have a credit card nonce, you're able to create a payment, add a customer, or save the card for later.

<iframe src="" width="100%" height="250px" style="margin-top: 0!important; border:0;margin:auto; display:block;font-size:15px;color:#4c555a"> </iframe>

Try it out!

- use one of our test cards, for example,  4111 1111 1111 1111
- use any future expiry date (mm/yy)
- and the CVV is  123

### Payment methods

sure, Frames looks great! But, it also has lots of payment options:
- VISA
- MasterCard
- American Express
- JCB
- Diners Club
- Discover

### The three-step process

It doesn't take long to get started with the client SDK. To accept online payments with the MonEFlow SDK, you'll need an integration that can:

1. Create a client token from your server.
2. Create a payment nonce.
3. Perform the payment request.

<aside class="notice">

To see an example of a Web SDK integration, [check out our GitHub](#)
</aside>

#### Step 1: Create a client token
A client is used to securely transmit payment data between the shopper and the MonEFlow PAY payments platform. Create one from your server by making a POST request to the  `/merchants/<merchantID>/client_token` endpoint. Specify your  merchant id, the  client id and client secret you're using for your integration. 

The request is described in the [authentication section](#client-to-moneflow-pay-server)

<aside class="warning">
Execute the request from your server, not your website.
</aside>

The response will contain a client token. You'll use this to initialize the SDK in the next step.

#### Step 2: Create a payment nonce.
Next, add the Client SDK to your website. The SDK will render the necessary inputs, that will securely collect the shopper's payment details, and make generate a payment nonce. To do this:

##### 1. Integrate SDK

Embed the Client SDK by adding the following code to the <head> of your payment page.

```html
<script src="http://assets.nazim.io/web/1.0.0/js/client.js"></script>
<script src="http://assets.nazim.io/web/1.0.0/js/hosted-fields.js"></script>
```
Then add the payment form to your page using a div.

```html

<form id="payment-form" method="post" action="/checkouts">
	<section>
		<label for="card-number">Card Number</label>
		<div id="card-number" class="hosted-field"></div>

		<label for="expiration-date">Expiration Date</label>
		<div id="expiration-date" class="hosted-field"></div>

		<label for="cvv">CVV</label>
		<div id="cvv" class="hosted-field"></div>

	</section>
	<div class="button-container">
		<input id="nonce" name="payment_method_nonce" type="hidden" />
		<input type="submit" value="Purchase" id="idsubmit" />
	</div>
</form>

```

##### 2. initialize SDK

Initialize the sdk by adding the script below at the end of the body of your page.

This will initialise an instance of the mpgate client, generate a credit card nonce on submit and submit the payment nonce to your server.

```html

<script>
	var form = document.querySelector('#payment-form');
	var btnsubmit = document.querySelector('input[type="submit"]');

	var client_token = <client_token>;


	mpgate.client.create({
		authorization: client_token,
		debug: true
	}, function (err, clientInstance) {
		if (err) {
			console.error(err);
			return;
		}
		createHostedFields(clientInstance);
	});
	var hostedField;
	function createHostedFields(clientInstance) {
		mpgate.hostedFields.create({
			client: clientInstance,
			fields: {
				number: {
					selector: '#card-number',
					placeholder: '4111 1111 1111 1111'
				},
				cvv: {
					selector: '#cvv',
					placeholder: '123'
				},
				expirationDate: {
					selector: '#expiration-date',
					placeholder: 'MM/YYYY'
				}
			}
		}, function (err, hostedFieldsInstance) {

			hostedField = hostedFieldsInstance;
			btnsubmit.removeAttribute('disabled');
			form.addEventListener('submit', formSubmitEvent, false);
		});
	}

	var formSubmitEvent = function (event) {
		event.preventDefault();

		hostedField.tokenize(function (err, payload) {
			if (err) {
				console.log('Error', err);
				return;
			}

			// Add the nonce to the form and submit
			document.querySelector('#nonce').value = payload.nonce;
			form.submit();
		});
	};

</script>

```

#### Step 2: Perform the payment request

The payment nonce is posted to your server where it can be used to perform a [payment request](#create-payment) using the server SDK.

The funding instrument to be used is the [credit card nonce](#credit_card_nonce).


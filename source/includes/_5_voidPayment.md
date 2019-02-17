## Void authorization

Voids, or cancels, an authorization, by ID. You cannot void a fully captured authorization.

For credit cards and some  alternative payment methods, the payment is completed in two steps:
1. Authorization – The payment details of the shopper are verified, and the funds are reserved.
2. Capture –  The reserved funds are transferred from the shopper to your account.
 
When the payment has been authorised but not yet captured, you can cancel the authorisation hold on the payment. 

<aside class="notice">
Cancelling a payment can either be done in the Merchant Dashboard or using the folloing API request.
</aside>


```shell

curl -X POST \
  http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/authorization/W94Q5DF2NWIRD71XYUI8/void \
  -H 'authorization: Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6' \
  -H 'content-type: application/json' \
  -d '{
	"invoice_number":"123456",
	"items":[
			{
				"sku":"100299S"
				,"name":"Ultrawatch"
				,"description":"Smart watch"
				,"quantity":"1"
				,"price":"110.00"
				,"shipping":"20"
				,"currency":"USD"
				,"url":""
				,"image":""
				,"tangible":"true"
			}
		],
	"custom":{
		"field1":"this is a test"
	}
}


```

```javascript

var data = JSON.stringify({
  "invoice_number": "123456",
  "items": [
    {
      "sku": "100299S",
      "name": "Ultrawatch",
      "description": "Smart watch",
      "quantity": "1",
      "price": "110.00",
      "shipping": "20",
      "currency": "USD",
      "url": "",
      "image": "",
      "tangible": "true"
    }
  ],
  "custom": {
    "field1": "this is a test"
  }
});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/authorization/W94Q5DF2NWIRD71XYUI8/void");
xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```

<aside class="notice">
`POST` /v1/merchants/authorization/{authorization_id}/void
</aside>

### Path parameters 

- #### authorization_id `string` <span style="color:orange;">required</span>

    The ID of the authorization to void.

### Request body

- #### invoice_number `string`

    The invoice number to track this payment.

- #### items `array(item)` 

    List of items being paid for.

- #### custom `object`

- ##### field1 `string`

    free-form field for the use of clients. maximum length of 100 ...


### Response body

<aside class="notice">
Amount is not specified as a void will always void the full amount authorized

Successful void request will return a message with response code 10000 
</aside>



>Response for the above request

```json


```




## Capture payment

When a payment is authorized, the funds are held on the customer's card for seven days. The merchant can either directly capture a payment on its dashboard or by using this API. The transaction is canceled if the merchant doesn't capture the authorization within seven days. Authorized payments can be captured either in full or partially.

<aside class="notice">
Note that is the intent is set to sale while creating the payment, the authorized payment is automatically capture in full.
</aside>

Any capture amount less than the original transaction will be treated as a partial capture.

```shell

curl -X POST \
  http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/authorization/W94Q5DF2NWIRD71XYUI8/capture \
  -H 'authorization: Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6' \
  -H 'content-type: application/json' \
  -d '{
	"amount": "110.00",
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
  "amount": "110.00",
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

xhr.open("POST", "http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/authorization/W94Q5DF2NWIRD71XYUI8/capture");
xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```

<aside class="notice">
`POST` /v1/merchants/authorization/{authorization_id}/capture
</aside>

### Path parameters 

- #### authorization_id `string` <span style="color:orange;">required</span>

    The ID of the authorization to capture.

### Request body

- #### amount `string`

    The amount to capture. If not specified, the amount authorized is captured in full. If specified, the amount should be less or equal the originally authorized amount. The currency used for this capture, is the same as the one used for authorization

- #### invoice_number `string`

    The invoice number to track this payment.

- #### items `array(item)` 

    List of items being paid for.

- #### custom `object`

- ##### field1 `string`

    free-form field for the use of clients. maximum length of 100 ...


### Response body

<aside class="notice">
Successful capture request will return a message with response code 10000 
</aside>


>Response for the above request

```json


```


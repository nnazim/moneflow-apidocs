
## Refund payments

A *sale* is a completed payment.
A *captured payment* is an authorized and captured payment.
You can refund sales and captured payments.

There are two types of refunds you might need to process:

1. **Full refund**
	A full refund returns the total amount of the transaction to the customer — it can only be performed once.

2. **Partial refund**
	A partial refund returns a sum less than the captured amount. A payment can be refunded multiple times, but cannot exceed the original payment amount.

<aside class="notice">
Refunds can be made in via the Dashboard or by using this API endpoint . Once processed, it is not possible to cancel a refund.
</aside>

Any refunds for less than the original captured amount will be considered partial refunds.


```shell

curl -X POST \
  http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/capture/W94Q5D31NWIRD90XYUI8/refund \
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

xhr.open("POST", "http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/capture/W94Q5D31NWIRD90XYUI8/refund");

xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```

<aside class="notice">
`POST` /v1/merchants/capture/{capture_id}/refund
</aside>

### Path parameters 

- #### capture_id `string` <span style="color:orange;">required</span>

    The ID of the captured payment to refund.

### Request body

- #### amount `string`

    The refund amount. If not specified, the amount captured is refunded in full. If specified, the amount should be less or equal the originally captured amount. The currency used for this refund, is the same as the one used for authorization

- #### invoice_number `string`

    The invoice number to track this payment.

- #### items `array(item)` 

    List of items being paid for.

- #### custom `object`

    - ##### field1 `string`

        free-form field for the use of clients. maximum length of 100 ...


### Response body

<aside class="notice">
Successful refund request will return a message with either a 10000 response code ~~or a 10100 response code for flagged transactions~~
</aside>


>Response for the above request

```json


```
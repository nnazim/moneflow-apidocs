# Payment

To help you through the payment process, our Payments REST API easily and securely accept online and ~~mobile~~ payments. 

To help you in the various steps of the process, our API provides necessary actions associated with payments

## Payment process

### Step 1: Payment
A payment can be of 2 Intent:

   1. `Sale` : Authorize the transaction. After successful authorization, the transaction is immediately captured.
   2. `Authorize`: Authorize the transaction

### Step 2: Capture or Void
In case of a successful payment with the intent `sale`, this step does not apply.

If the transaction was only authorized:

- If the merchant is comfortable with the sale and can fulfill the customer's needs, then the merchant will `Capture` the payment, thus finalizing the transaction, and the funds will transfer to the merchant.<br/><br/>
  
- If for whatever reason, the merchant is not able to fulfill the needs of the customer, then the merchant can `Void` the transaction. This is the simplest way to cancel a payment. Voiding a payment removes the hold on the funds, and returns them to the customer. The payment is now canceled, and there are no further actions to take.

### Step 3: Refund
If a customer changes their mind, or something goes wrong, and the transaction needs to be canceled after capture: a refund is processed. A refund returns the funds to the customer and reverses the sale.


## Create payment


>Authorize a transaction using full card number 

```shell
curl -X POST \
  http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/payment \
  -H 'authorization: Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6' \
  -H 'content-type: application/json' \
  -d '{
	"intent":"authorize"
	,"payer":{
		"payment_type":"CC"
		,"funding_instrument":{
			"credit_card":{
				"number":"4111111111111111"
				,"expire_month":12
				,"expire_year":2020
				,"cvv2":"123"
				,"first_name":"John"
				,"last_name":"Smith"
				,"billing_address":{
					"line1":"18 Test street"
					,"city":"london"
					,"country_code":"GB"
					,"postal_code":"W1C"
					,"state":""
					,"phone":{
						"country_code":"+44"
						,"number":"0988739990"
					}
				}
			}
		}
		,"payer_info":{
			"email":"john.smith@gmail.com"
		}
	}
	,"payee":{
		"email":"mail@merchantdomain.com"
		,"merchant_id":"w3z8dfhkzvfq0j9n"
	}
	,"transaction":{
		"amount":{
			"currency":"USD"
			,"total":"169.99"
			,"details":{
				"subtotal":"159.99"
				,"shipping":"10"
			}
		}
		,"description":"purchase"
		,"items":[
			{"sku":"100299S"
			,"name":"Ultrawatch"
			,"description":"Smart watch"
			,"quantity":"1"
			,"price":"85.99"
			,"shipping":"4"
			,"currency":"USD"
			,"url":""
			,"image":""
			,"tangible":"true"
			},
			{"sku":"100269S"
			,"name":"Drone"
			,"description":"drone x"
			,"quantity":"2"
			,"price":"37"
			,"shipping":"3"
			,"currency":"USD"
			,"url":""
			,"image":""
			,"tangible":"true"
			}
			]
		,"shipping_address":{
			"recipient_name":"Tom Hanks"
		}
		,"soft_descriptor":{
			"text":"test.com"
		}
		,"invoice_number":"123455"
	}
	,"custom":{
		"field1":"this is a test"
	}
}'

```

```javascript

var data = JSON.stringify({
  "intent": "authorize",
  "payer": {
    "payment_type": "CC",
    "funding_instrument": {
      "credit_card": {
        "number": "4111111111111111",
        "expire_month": 12,
        "expire_year": 2020,
        "cvv2": "123",
        "first_name": "John",
        "last_name": "Smith",
        "billing_address": {
          "line1": "18 Test street",
          "city": "london",
          "country_code": "GB",
          "postal_code": "W1C",
          "state": "",
          "phone": {
            "country_code": "+44",
            "number": "0988739990"
          }
        }
      }
    },
    "payer_info": {
      "email": "john.smith@gmail.com"
    }
  },
  "payee": {
    "email": "mail@merchantdomain.com",
    "merchant_id": "w3z8dfhkzvfq0j9n"
  },
  "transaction": {
    "amount": {
      "currency": "USD",
      "total": "169.99",
      "details": {
        "subtotal": "159.99",
        "shipping": "10"
      }
    },
    "description": "purchase",
    "items": [
      {
        "sku": "100299S",
        "name": "Ultrawatch",
        "description": "Smart watch",
        "quantity": "1",
        "price": "85.99",
        "shipping": "4",
        "currency": "USD",
        "url": "",
        "image": "",
        "tangible": "true"
      },
      {
        "sku": "100269S",
        "name": "Drone",
        "description": "drone x",
        "quantity": "2",
        "price": "37",
        "shipping": "3",
        "currency": "USD",
        "url": "",
        "image": "",
        "tangible": "true"
      }
    ],
    "shipping_address": {
      "recipient_name": "Tom Hanks"
    },
    "soft_descriptor": {
      "text": "test.com"
    },
    "invoice_number": "123455"
  },
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

xhr.open("POST", "http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/payment");
xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);


```

>Authorize a transaction using credit card nonce

```shell
curl -X POST \
  http://api.nazim.io/v1/merchants/w3z8dfhkzvfq0j9n/payment \
  -H 'authorization: Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6' \
  -H 'content-type: application/json' \
  -d '{
	"intent":"authorize"
	,"payer":{
		"payment_type":"CC"
		,"funding_instrument":{
			"credit_card_nonce":{
				"nonce":"0080e4d1-02f5-49ad-9462-444de2cdf8c9"
				,"first_name":"John"
				,"last_name":"Smith"
				,"billing_address":{
					"line1":"18 Test street"
					,"city":"london"
					,"country_code":"GB"
					,"postal_code":"W1C"
					,"state":""
					,"phone":{
						"country_code":"+44"
						,"number":"0988739990"
					}
				}
			}
		}
		,"payer_info":{
			"email":"john.smith@gmail.com"
		}
	}
	,"transaction":{
		"amount":{
			"currency":"USD"
			,"total":"169.99"
		}
		,"description":"purchase"		
		,"invoice_number":"123455"
	}
}'

```

```javascript

var data = JSON.stringify({
  "intent": "authorize",
  "payer": {
    "payment_type": "CC",
    "funding_instrument": {
      "credit_card": {
        "credit_card_nonce": {
        "nonce": "0080e4d1-02f5-49ad-9462-444de2cdf8c9",
        "first_name": "John",
        "last_name": "Smith",
        "billing_address": {
          "line1": "18 Test street",
          "city": "london",
          "country_code": "GB",
          "postal_code": "W1C",
          "state": "",
          "phone": {
            "country_code": "+44",
            "number": "0988739990"
          }
        }
      }
    },
    "payer_info": {
      "email": "john.smith@gmail.com"
    }
  },
  "transaction": {
    "amount": {
      "currency": "USD",
      "total": "169.99"
    },
    "description": "purchase",    
    "invoice_number": "123455"
  }
});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "/v1/merchants/w3z8dfhkzvfq0j9n/payment");
xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);


```

<aside class="notice">
`POST` /v1/merchants/`<merchant_id>`/payment
</aside>

Creates a sale or an authorizad payment to be captured later. To create a sale, or authorization, include the payment details in the JSON request body. Set the intent to sale or authorize

Include payer and transaction details. The combination of `payment_method` and `funding_instrument` determines the type of payment that is created. 

### Header parameters

The request require a `Basic` authentication in the header. For more information about HTTP request headers, see [HTTP request headers.](#http-request-header) 

### Request body

- #### _intent_ `enum` <span style="color:orange;">required</span>

    The payment intent. Value is:

    - `sale`. Makes an immediate payment.
    - `authorize`. Authorizes a payment for capture later

    Allowed values: `sale`, `authorize`.

- #### _payer_ `object` <span style="color:orange;">required</span>
   The source of the funds for this payment. 

- #### _transaction_ `object` <span style="color:orange;">required</span>
  Defines what the payment is for and who fulfills the payment. 

- #### _custom_ custom `object` 
  Free-form field for the use of clients

- #### _payee_ `object`
   The payee who receives the funds and fulfills the order.



#### Payer


- ##### _payment_type_ `enum` <span style="color:orange;">required</span>
 
    Payment type being used. Value is:
  - `CC` : Credit card 
  - ~~`LP` : Local Payment~~

- ##### _payer_info_ `object` <span style="color:orange;">required</span>
   
    Information related to the Payer. 
    
- ##### _funding_instrument_ `object` <span style="color:orange;">required</span>

    Funding instrument to be used to fund the payment. The object  must include either a credit_card or credit_card_token or credit_card_nonce object. If the array contains more than one instrument, the payment is declined.


#### payer_info

- ##### _email_ `string` <span style="color:orange;">required</span>

  
    Email address representing the payer. 127 characters max.


- ##### _ip_ `string` <span style="color:lightblue;">optional</span>


    IP Address of the payer


- ##### _name_ `string` <span style="color:lightblue;">optional</span>


    Full Name of the payer  

- ##### *birth_date*`string` 


    Birth date of the Payer in ISO8601 format (yyyy-mm-dd).


#### funding_instrument
Only one of the object described below is <span style="color:orange;">required</span>:


- ##### _credit_card_ `object` 

    The credit card details. You can use this instrument to fund a payment. 


- ##### _credit_card_nonce_ `object` 

    The tokenized credit card details. You can use this instrument to fund only one payment .


- ##### ~~_credit_card_token_ `object`~~ 

    The tokenized credit card details. You can use this instrument to fund a payment.



#### credit_card

- #####  number `string` <span style="color:orange;">required</span>:
  The credit card number. Value is numeric characters only with no spaces or punctuation. Must conform to the modulo and length required by each credit card type. 

- #####  expire_month `integer` <span style="color:orange;">required</span>
  The expiration month with no leading zero. Value is from `1` to `12`.

- ##### expire_year `integer` <span style="color:orange;">required</span>
  The four-digit expiration year.

- #####  cvv2 `string` 
  The three- to four-digit card validation code.

- ##### name `string` 
  The card holder's name as displayed on the card.

- ##### billing_address `object`  <span style="color:lightblue;">optional</span>
  The billing address for this card.

<br>

#### credit_card_nonce

- #####  nonce `string` 
  one time use tokenized credit card details

- ##### name `string` 
  The card holder's name as displayed on the card.

- ##### billing_address `object`  <span style="color:lightblue;">optional</span>
  The billing address for this card.

<br>

#### ~~credit_card_token~~
- #####  ~~credit_card_id `string`~~
  ~~ID of credit card previously stored in vault~~

- ##### ~~name `string`~~
  ~~The card holder's name as displayed on the card.~~

- #####  cvv2 `string` 
  ~~The three- to four-digit card validation code.~~

- ##### ~~billing_address `object`  <span style="color:lightblue;">optional</span>~~
  ~~The billing address for this card.~~


#### billing_address

- ##### line1 `string`  <span style="color:orange;">required</span>

    The first line of the address. For example, number, street, and so on.

    Maximum length: 100.

- ##### line2 `string`

    The second line of the address. For example, suite or apartment number.

    Maximum length: 100.

- ##### city `string`

    The city name.

    Maximum length: 64.

- ##### country_code `string`  <span style="color:orange;">required</span>

    two-character ISO 3166-1 code that identifies the country or region.
    **Note:** The country code for Great Britain is `GB` and not `UK` as used in the top- level domain names for that country. 

    Minimum length: 2.

    Maximum length: 2.

    Pattern: `^([A-Z]{2}|C2)$`.


- ##### postal_code `string`

    The postal code, which is the zip code or equivalent. 


- ##### state `string`

    2 letter code for US states, and the equivalent for other countries.


- ##### phone `object`

    Information related phone number.



#### phone 
- ##### country_code `string`  <span style="color:orange;">required</span>
    Country code (from in E.164 format)

- ##### number 
    In-country phone number (from in E.164 format). Maximum length is 25 characters.



#### transaction 

- ##### amount `object` <span style="color:orange;">required</span>

    The amount to collect.

- ##### type `string`

    Describe the transaction type. Possible value:

    - 1 : Regular
    - ~~2 : Subscription~~
    - 3 : MOTO

    Defaulted to 1 (regular) if not provided


- ##### mode`string`

    Describe the transaction mode. Possible value:

    - 1 : Non 3D
    - ~~2 : 3D~~
    - ~~3 : Local payment~~

    Defaulted to 1 (regular) if not provided


- ##### description `string`

    Description of what is being paid for.  


- ##### items `array(item)` 

    List of items being paid for.


- ##### shipping_address `object` <span style="color:orange;">'required' if billing address is not provided</span>

    The shipping address.


- ##### shipping_method `string`

    Shipping method used for this payment like USPSParcel etc.


- ##### notify_url `string`

    URL to send payment notifications(Maximum length: 2048.)


- ##### order_url `string`

    Url on merchant site pertaining to this payment.(Maximum length: 2048.)


- ##### invoice_number `string`

    invoice number to track this payment (Maximum length: 50.)


- ##### soft_descriptor `object`

    Dynamic Soft descriptor used when charging this funding source.


#### amount

- ##### currency `string ` <span style="color:orange;">required</span>

    The [three-character ISO-4217 currency code](#currency-codes). 


- ##### total `string` <span style="color:orange;">required</span>

    The total amount charged to the payee by the payer. Maximum length is 10 characters, which includes:

    - Seven digits before the decimal point.
    - The decimal point.
    - Two digits after the decimal point.
  

#### shipping_address

- ##### recipient_name `string`

    Name of the recipient at this address.


- ##### line1 `string`  <span style="color:orange;">required</span>

    The first line of the address. For example, number, street, and so on.

    Maximum length: 100.


- ##### line2 `string`

    The second line of the address. For example, suite or apartment number.

    Maximum length: 100.

- ##### city `string`

    The city name.

    Maximum length: 64.


- ##### country_code `string`  <span style="color:orange;">required</span>

    two-character ISO 3166-1 code that identifies the country or region.

    **Note:** The country code for Great Britain is `GB` and not `UK` as used in the top- level domain names for that country. 

    Minimum length: 2.

    Maximum length: 2.

    Pattern: `^([A-Z]{2}|C2)$`.


- ##### postal_code `string`

    The postal code, which is the zip code or equivalent. 


- ##### state `string`

    2 letter code for US states, and the equivalent for other countries.


- ##### phone `object`

    Information related phone number.



#### soft_descriptor

- ##### text `string`

    The soft descriptor text to use to charge this funding source. If greater than the maximum allowed length, the API truncates the string.

    Maximum length: 22.

- ##### ~~city `string`~~

    ~~The soft descriptor city to use to charge this funding source.~~


- ##### ~~country `string`~~

    ~~The soft descriptor country to use to charge this funding source.~~

    ~~two-character ISO 3166-1 code that identifies the country or region.~~


#### item

- ##### sku `string`

    The stock keeping unit (SKU) for the item.

- ##### name `string`

    The item name. If this value is greater than the maximum allowed length, the API truncates the string.

- ##### description `string`
  
    The item description.

- ##### quantity `string`

    The item quantity. Must be a whole number.
    Maximum length: 10.

    Pattern: `^[0-9]{0,10}$`.

- ##### price `string`

    The item cost. Supports two decimal places.
    Pattern: `^[0-9]{0,10}(\.[0-9]{0,2})?$`

- ##### shipping `string`
    
    The shipping cost

- ##### currency `string`

    The three-character ISO-4217 currency code that identifies the currency.

- ##### image `string`

    URI to an image representing the item

- ##### tangible `bool`

    Identifies if the item is a tangible or not

- ##### id `string`

    Unique id

  
#### custom

- ##### field1 `string`

    free-form field for the use of clients. maximum length of 100

- ##### field2 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field3 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field4 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field5 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field6 `string`
    
    free-form field for the use of clients. maximum length of 100

- ##### field7 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field8 `string`
    
    free-form field for the use of clients. maximum length of 100

- ##### field9 `string`
  
    free-form field for the use of clients. maximum length of 100

- ##### field10 `string`
  
    free-form field for the use of clients. maximum length of 100

### Response body

<aside class="notice">
Successful authorizaton request will return a message with either a 10000 response code ~~or a 10100 response code for flagged transactions~~
</aside>


>Response for the above request
```json


```


### Understanding the response

By looking at the status, responseMessage and responseCode fields in the payment response you can find out why the payment has not gone through. It's possible the payment has an invalid or expired card or a valid card with an insufficient available balance.

The following pages can be used as references when understanding the response message:



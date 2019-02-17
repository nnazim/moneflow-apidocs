
# API Request

To construct a REST API request, combine these components:

| Component                                      | Description                                                  |
| ---------------------------------------------- | ------------------------------------------------------------ |
| The HTTP method                                | - `GET`. Requests data from a resource.<br/>- `POST`. Submits data to a resource to process.<br/>- `PUT`. Updates a resource.<br/>- `PATCH`. Partially updates a resource.<br/>- `DELETE`. Deletes a resource. |
| The URL to the API service                     | Live. `https://api.moneflow.com`                             |
| The URI to the resource                        | The resource to query, submit data to, update, or delete. <br/><br/> For example, `v1/merchants/<merchant_id/>`. <br/><br/> Where `<merchant_id>` id the unique identifier of the business registered with MonEFlow PAY|
| Query parameters                               | Optional. Controls which data appears in the response. Use to filter, limit the size of, and sort the data in an API response. |
| [HTTP request headers](#http-request-header) | Includes the `Authorization` header with the access token.   |
| A JSON request body                            | Required for most `GET`, `POST`, `PUT`, and `PATCH` calls.   |

## HTTP Request Header

| Header | Description  |
|--|--|
| Authorization |  Required to make API calls. <br/> <br/>On client SDK we use a bearer token in the `Authorization` header <br/> Click here to see how to get a client token <br/><br/> To make REST API calls, include a `Basic` authentication scheme in the header|
|Content-Type| Required for operations with a request body. <br/><br/> Specifies the request format. The syntax is:<br/><br/> `Content-Type: application/<format>` <br/><br/> Where  `format`  is  `json`.|



# Payment Nonce

The payment nonce is a string returned by the client SDK to represent a payment method. This string is a reference to the payer payment information that was provided in your payment form and should be sent to your server where it can be used with the server SDKs to create a new payment request.

## Get Credit card nonce

```shell

curl -X POST \
  http://api.nazim.io/v1/merchants/<merchant_id>/credit-card-nonce \
  -H 'authorization: Bearer <client_token>' \
  -H 'content-type: application/json' \
  -d '{
	"number": "4005520201264821"
	,"expire_month": 12
	,"expire_year": 2020
	,"cvv2": "123"
}'

```

```javascript
var data = JSON.stringify({
  "number": "4005520201264821",
  "expire_month": 12,
  "expire_year": 2020,
  "cvv2": "123"
});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "http://api.nazim.io/v1/merchants/merchant_id/credit-card-nonce");
xhr.setRequestHeader("authorization", "Bearer <client_token>");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "brand_code": "VISA",
        "credit_card_nonce": "dd959bad-0943-4d2e-9baf-fa64656420b7",
        "last4": "4821",
        "type": "Visa",
        "expire_month": 12,
        "expire_year": 2020,
        "expires_on": "2018-11-03T14:20:53Z",
        "issued_on": "2018-11-02T14:20:53Z"
    }
}
```

<aside class="notice">
`POST` /v1/merchants/<merchant_id>/credit-card-nonce
</aside>

Retrieves a credit card nonce from the credit card information. 

<aside class="notice">
Used by the client SDK to retrieve a credit card nonce. This should not be called on server
</aside>


### Request body

- #### number `string` <span style="color:orange;">required</span>
      
    Credit card number. 
    Numeric characters only with no spaces or punctuation. 
    The string must conform with modulo and length required by each credit card type. 

- #### expire_month `integer` <span style="color:orange;">required</span>
    Expiration month with no leading zero. 
    Acceptable values are 1 through 12.

- #### expire_year `integer` <span style="color:orange;">required</span>
    Expiration year 
    Acceptable values should be 4 digits

- #### cvv2 `string` 
    3-4 digit card validation code.



### Response body 

- #### data `object`


    - ##### brand_code `string`        
  
        The credit card brand. Value is visa, mastercard, discover, or amex.

    - ##### credit_card_nonce `string`     
     
        Credit card payment nonce used to perform server side transaction request
        
    - ##### last4 `string`        
  
        The last four digits of the stored credit card number.

    - ##### type `string`        
  
        The credit card type. Value is visa, mastercard, discover, or amex.

    - ##### expire_month `integer` 
   
        The expiration month with no leading zero. Value is from 1 to 12.

    - ##### expire_year `integer`
  
        The four-digit expiration year.     

    - ##### expires_on `string` [Internet date and time format](https://tools.ietf.org/html/rfc3339#section-5.6 "external link")        
      The date and time when the payment nonce expires

    - ##### issued_on `string` [Internet date and time format](https://tools.ietf.org/html/rfc3339#section-5.6 "external link")
      The date and time when the payment nonce was issued


# Authentication

When enrolled with MonEFlow PAY, a set of client ID and secret credentials are provided together with your merchant ID. 

<aside class="notice">
Client ID and secret can be generated from your merchant dashboard.
The merchant ID is provided to you upon creation of a business (Business specific)
</aside>

## Server-to-server

```shell
# Example header

curl https://API_endpoint_here \
  -H "Authorization: Basic <encoded string>" \ 

```

```javascript
// Example header

curl https://API_endpoint_here \
  -H "Authorization: Basic <encoded string>" \ 

```

To initiate a request to any MonEFlow PAY's endpoints from your server, the `Authorization` field of your HTTPS header must include an Basic Authentication scheme.

<aside class="warning">
Use HTTPS for all API requests, HTTP requests will not be successful.
</aside>

The "Basic" HTTP authentication scheme is defined in [RFC 7617](https://tools.ietf.org/html/rfc7617), which transmits credentials as Client ID/Secret pairs, encoded using base64. For e.g :

1. Build a string of the `<clientID>:<ClientSecret>`
2. BASE64 encode the string.
3. Supply an Authorization header with content Basic followed by the encoded string



## Client to MonEFlow PAY server

> Retrieve client token

```shell

curl -X POST \
  https://api.moneflow.com/v1/merchants/<merchantID>/client_token \
  -H 'authorization: Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6' \
  -H 'content-type: application/json' \
  -d ''\''grant_type=client_credentials'\'''

```

```javascript

var data = "'grant_type=client_credentials'";

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://api.moneflow.com/v1/merchants/<merchantID>/client_token");
xhr.setRequestHeader("authorization", "Basic ODZidWQ0Y2JremlxOXZmYzoweHI1ZDkwOHo2bmo4a2h6");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```

> The above command returns JSON structured like this:

```json
{
    "accessToken": "<client_token>",
    "tokenType": "Bearer",
    "expiresOn": "2018-11-02T21:33:09Z",
    "issuedOn": "2018-11-01T21:33:09Z"
}
```

Together with every client side request, we need to send a client token. To retrieve a this, you pass the Basic Authentication in the `Authorization` header of a get access token request.

In exchange of these credentials the MonEFlow PAY server will issue a client token. 

> This sample request uses a bearer token to get payment nonce for a client:

```shell

curl -X POST \
  http://API_endpoint_here \
  -H 'authorization: Bearer <client_token>' \

```

```javascript

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "http://API_endpoint_here");
xhr.setRequestHeader("authorization", "Bearer <client_token>");

xhr.send(data);

```

Include this bearer token in API requests in the `Authorization` header with the Bearer authentication scheme.
# Overview

We provide complementary client and server SDKs to complete your integration:

-   The client SDKs enable you to securely collect payment information from your customers
-   The server SDKs enable you to act on the collected payment information

We have SDKs for the following client platforms and server languages:

| **Client SDKs** | **Server SDKs** |
|--|--|
| - JavaScript | - .NET <br /> - PHP|

The client SDKs require authorization. In this  guide, we'll use a client token generated on server side to perform authorization.
The server SDKs uses your client Id and secret key to initiate any server-to-server request. To initiate a payment request you can either use a full card (for PCI Compliant Merchant) or a **Payment Nonce**.

## How it works
The diagram below illustrates the step required to return a payment nonce and then use that to process a payment.

<!--img src="images/Sequence_diagram.JPG" class="lang-specific ruby"-->
<div class="mermaid" class="mermaid-div">sequenceDiagram
Your Client ->> Your Server: Request a client token
Your Server-->>Your Client:Generate and send the client token
Your Client ->> MonEFlow Pay Server: submit payment information
MonEFlow Pay Server -->> Your Client: returns a payment nonce
Your Client ->> Your Server: Send payment nonce
Your Server ->> MonEFlow Pay Server: Create a transaction
MonEFlow Pay Server -->> Your Server: Generate a response
Your Server-->>Your Client: Send a response
</div>
<br/><br/>

 1. Your ~~app or~~ web front-end requests a client token from your server
    in order to initialize the client SDK
 2. Your server generates and sends a **client token** back to your client
    with the server SDK
 3. Once the client SDK is initialized and the customer has submitted
    payment information, the SDK communicates that information and
    returns a **payment nonce**
 4. You then send the payment nonce to your server
 5. Your server code receives the payment nonce from your client
    and then uses the server SDK to **create a transaction** or perform
    other functions.
    

## Client Token

A client token is a signed data blob that includes configuration and authorization information required by the **client SDK**. ~~These should not be reused; a new client token should be generated for each request that's sent to MonEFlow PAY. For security, we will revoke client tokens if they are reused excessively within a short time period.~~ [Currently client tokens have a lifetime of 8 hours, after which it expires.]

Your server is responsible for generating the client token, which contains all of the necessary configuration information to set up the client SDKs. When your server provides a client token to your client, it authenticates the application to communicate directly to MonEFlow PAY.

Your client is responsible for obtaining the client token from your server and initializing the client SDK.

## Payment Nonce

A payment method nonce is a secure, one-time-use reference to payment information. It's the key element that allows your server to communicate sensitive payment information to MonEFlow PAY without ever touching the raw data.

### Security

Security is important for all payment method types, but it's particularly critical for cards.

The Payment Card Industry Security Standards Council mandates compliance with their Data Security Standard (PCI DSS), and the less exposure your business has to raw card data, the easier it is to demonstrate compliance. Using payment nonces in place of raw card data helps keep your PCI compliance burden to a minimum.

### Usage

Our servers generate payment nonces in response to requests from our client and server SDKs.

In general, your client will be responsible for receiving payment nonces from MonEFlow PAY and sending them to your server. Your server will then be responsible for sending those payment nonces back to MonEFlow PAY on requests to perform certain actions.

You'll need payment nonces for two main purposes:

1. To create transactions
2. To create or update payment methods in the Vault for repeat use

### Lifespan

A payment nonce may only be used once. ~~If it is not used, it expires 3 hours after being created.~~ [Yet to be implemented]
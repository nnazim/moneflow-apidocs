---
title: Documentation

site_url: http://nazim.io

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='#'>Contact us</a>
  - <a href='#'>Documentation Powered by Ruby on rails</a>

includes:
  - 1_overview
  - 2_1_authentication
  - 2_2_integration
  - 3_paymentNonce
  - 4_payment
  - 5_voidPayment
  - 6_capturePayment
  - 7_RefundPayment
  - 8_RC

search: true
---

# Get Started

The MonEFlow Pay API is is constructed around a [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) structure. By using familiar and predictable URLs, our API works with HTTP response codes to define and signal errors. HTTP methods (or verbs as they are commonly called) are used to determine the action of a request. API responses and errors return in [JSON](http://www.json.org/)




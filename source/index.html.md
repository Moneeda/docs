---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - shell

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Moneeda API

## Introduction

Welcome to the Moneeda API documentation!

## API Limits

We are testing everything, so no limits for now. Act responsibly and please don't DDOS.

## Disclaimer

Use this API carefully as real money is being moved.

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: Bearer MY_MONEEDA_TOKEN"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');
```

> Make sure to replace `MY_MONEEDA_TOKEN` with your token

Moneeda uses a token to allow access to the API. You can register a new user and retrieve a new token at our [developer portal](https://moneeda.com/developers).

Moneeda expects for the token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer MY_MONEEDA_TOKEN`

<aside class="notice">
You must replace <code>MY_MONEEDA_TOKEN</code> with your personal token.
</aside>

# Public endpoints

All public endpoints do not require any specific API key. If you are going to heavily
call the endpoint, consider using a Websocket instead.

## Public client instance

> Create a new instance of the public client

```shell
// there is no real differentiation in the url between public/authenticated endpoints
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

const publicClient = moneeda.public();

// An exchange can be passed optionally
const publicClient2 = moneeda.public('BTX');

// or set
publicClient.setExchange('BTX');
```

If you are using the provided `moneeda libraries`, you can easily get an instance
of the public client

### Exchanges
Moneeda will use the defined exchange of the public instance by default.

To set an exchange you can pass it as a `parameter` to the constructor or just
set it as needed (great to make multiple calls to different exchanges).

<aside class="warning">
Be careful if you are going to call multiple exchanges in different endpoints. You should
have a new instance in each endpoint to avoid concurrency problems.
</aside>

## Get All Products

> Returns all the products of an exchange

```shell
curl "https://api.moneeda.com/api/exchanges/EXCHANGE/products"
  -H "Authorization: meowmeowmeow"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

moneeda.public(EXCHANGE).products();
// or
const publicClient = moneeda.public()
publicClient.setExchange(EXCHANGE)
publicClient.products()

```

> The above command returns JSON structured like this:

```json
[
  {
    "id": "BTC-USD",
    "originalId": "btcusd",
    "base_currency": "BTC",
    "quote_currency": "USD",
    "base_min_size": "0.002",
    "base_max_size": "2000",
    "quote_increment": "5"
  },
  {
    "id": "LTC-USD",
    "originalId": "ltcusd",
    "base_currency": "LTC",
    "quote_currency": "USD",
    "base_min_size": "0.002",
    "base_max_size": "2000",
    "quote_increment": "5"
  }
]
```

> Make sure to replace `EXCHANGE` with your desired exchange


This endpoint retrieves all the products of an exchange.

### HTTP Request

`GET https://api.moneeda.com/api/EXCHANGE/products`

### Parameters

Parameter | Description
--------- | -----------
EXCHANGE | The desired exchange to retrieve the products.


## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

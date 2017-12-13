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

## Products

> Returns all the products of an exchange

```shell
curl "https://api.moneeda.com/api/exchanges/EXCHANGE/products"
  -H "Authorization: Bearer MY_MONEEDA_TOKEN"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

moneeda.public(EXCHANGE)
      .products()
      .then((products) => console.log(products));

// or with async/await
const products = await moneeda.public(EXCHANGE).products()

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


Returns a Promise that resolves all products available for the defined exchange

### HTTP Request

`GET https://api.moneeda.com/api/EXCHANGE/products`

### Parameters

Parameter | Description
--------- | -----------
EXCHANGE | The desired exchange to retrieve the products.


## Ticker

> Returns a product ticker

```shell
curl "https://api.moneeda.com/api/exchanges/EXCHANGE/ticker?product=MY-PRODUCT"
  -H "Authorization: Bearer MY_MONEEDA_TOKEN"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

moneeda.public(EXCHANGE)
      .ticker(MY-PRODUCT)
      .then((ticker) => console.log(ticker));

// or with async/await
const ticker = await moneeda.public(EXCHANGE).ticker('BTC-USD')

```

> The above command returns JSON structured like this:

```json
{
    "price": 16940,
    "size": 134.16484512,
    "bid": 16930,
    "ask": 16939,
    "volume": 52005.64483952
}
```

> Make sure to replace `EXCHANGE` with your desired exchange

Returns a promise that resolves the ticker for an specific product

### HTTP Request

`GET https://api.moneeda.com/api/EXCHANGE/ticker`

### Parameters

Parameter | Description
--------- | -----------
EXCHANGE | The desired exchange to retrieve the products.

### Query Parameters

Parameter | Default | Description
--------- |-------- | -----------
product | - |The desired product.

## Tickers

> Returns all the products' tickers

```shell
curl "https://api.moneeda.com/api/exchanges/EXCHANGE/alltickers"
  -H "Authorization: Bearer MY_MONEEDA_TOKEN"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

moneeda.public(EXCHANGE)
      .allTickers()
      .then((tickers) => console.log(tickers));

// or with async/await
const tickers = await moneeda.public(EXCHANGE).allTickers()

```

> The above command returns JSON structured like this:

```json
[
  {
    "product": "BTC-ETC",
    "price": 0.00174066,
    "high": 0.00194,
    "low": 0.00158393,
    "bid": 0.00174068,
    "ask": 0.00174982,
    "volume": 3345.87474393
  },
  {
    "product": "BTC-ETH",
    "price": 0.03795013,
    "high": 0.03945476,
    "low": 0.02975,
    "bid": 0.03795013,
    "ask": 0.03799998,
    "volume": 14772.50221729
  }
]
```

> Make sure to replace `EXCHANGE` with your desired exchange

Returns a promise that resolves the tickers of all the available products

### HTTP Request

`GET https://api.moneeda.com/api/EXCHANGE/alltickers`

### Parameters

Parameter | Description
--------- | -----------
EXCHANGE | The desired exchange to retrieve the products.

<aside class="notice">
This endpoint is not supported the following exchanges:
<strong>GDX, LQI</strong>
</aside>



## Candles

> Returns the candles for a specific product

```shell
curl "https://api.moneeda.com/api/exchanges/EXCHANGE/candles?product=MY-PRODUCT&period=30m"
  -H "Authorization: Bearer MY_MONEEDA_TOKEN"
```

```javascript
const Moneeda = require('moneeda-node');
const moneeda = new Moneeda('MY_MONEEDA_TOKEN');

moneeda.public(EXCHANGE)
      .candles(PRODUCT, PERIOD)
      .then((candles) => console.log(candles));

// or with async/await
const candles = await moneeda.public(EXCHANGE).candles('BTC-USD', '30m')

```

> The above command returns JSON structured like this:

```json
[
  {
    "T": 1513126200,
    "O": 17840.01,
    "C": 17840.01,
    "H": 17840.01,
    "L": 17840,
    "V": 5.50505191
  },
  {
    "T": 1513125780,
    "O": 17872,
    "C": 17871,
    "H": 17872.01,
    "L": 17871,
    "V": 19.252282129999994
  },
  ...
]
```

> Make sure to replace `EXCHANGE` with your desired exchange

Returns a promise that resolves the candles of a defined product for a specific period

### HTTP Request

`GET https://api.moneeda.com/api/EXCHANGE/candles`

### Parameters

Parameter | Description
--------- | -----------
EXCHANGE | The desired exchange to retrieve the products.

### Query Parameters

Parameter | Default | Description
--------- |-------- | -----------
product | - |The desired product.
period | 30m | The period desired. Should be one of the following: 1m, 5m, 15m, 30m

<aside class="notice">
This endpoint is not supported the following exchanges:
<strong>LQI</strong>
</aside>













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

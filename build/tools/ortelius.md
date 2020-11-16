# Ortelius API

This API allows clients to interact with [Ortelius](https://github.com/ava-labs/ortelius), the Avalanche indexer.

## Format

This API uses simple GET HTTP requests using URL query parameters and returns JSON data.

## Versioning

The current version of the API is version 2, and is accessed at the following path off the root of your server:

```test
/v2
```

Information about the version 1 API can be found here: asdf

## Data Types

In addition to the integers, strings, and booleans, the following data types are used throughout the API:

| Name | Description | Examples
| --- | --- | --- 
| `id` | A CB58 encoded object identifier, such as a chain, transaction, or asset ID | `2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM`
| `address` | A bech-32 encoded address | `fuji1wycv8n7d2fg9aq6unp23pnj4q0arv03ysya8jw`
| `datetime` | A Unix timestamp as an integer, or an RFC3339 formatted string | `1599696000`, `2020-09-10T00:00:00Z`

## List Parameters

All endpoints for listing resources accept the following parameters:

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| `limit` | `int` | The maximum number of items to return | `500` | `500`
| `offset` | `int` | The number of items to skip | `0` | None
| `query` | `string` | An ID prefix to filter items by | None | None
| `startTime` | `datetime` | Limits to items created on or after a given time | `0` | Now
| `endTime` | `datetime` | Limits to items created on or before a given time | Now | Now

## Available Endpoints

### Overview

The root of the API give an overview of the constants for the active Avalanche network being indexed.

#### **Params**

None

#### **Example Call**

```shell
curl "http://127.0.0.1:8080/v2"
```

#### **Example Response**

```json
{
  "network_id": 1,
  "chains": {
    "11111111111111111111111111111111LpoYY": {
      "chainID": "11111111111111111111111111111111LpoYY",
      "chainAlias": "p",
      "vm": "pvm",
      "avaxAssetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "networkID": 1
    },
    "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM": {
      "chainID": "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM",
      "chainAlias": "x",
      "vm": "avm",
      "avaxAssetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "networkID": 1
    }
  }
}
```

### Search

Find a transaction or address from an ID or ID prefix.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| `query` | `string` | An ID prefix to filter items by | None | None

#### **Example Call**

```shell
curl "http://127.0.0.1:8080/v2/search?query=2jEugPDFN89KXLEXtf5"
```


#### **Example Response**

```json

```

### Aggregate

Calculate aggregate transaction data over a time frame.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| chainID | id | A chain ID to filter results by. Can be supplied multiple times. | None | None
| assetID | id | An asset ID to filter results by. | None | None

#### **Example Call**

```shell
curl "http://127.0.0.1:8080/v2/aggregates?startTime=2020-09-21T00:00:00Z&endTime=2020-10-21T00:00:00Z"
```

#### **Example Response**

```json
{
  "startTime": "2020-09-21T00:00:00Z",
  "endTime": "2020-10-21T00:00:00Z",
  "aggregates": {
    "startTime": "2020-09-21T00:00:00Z",
    "endTime": "2020-10-21T00:00:00Z",
    "transactionVolume": "1652211194850792239",
    "transactionCount": 135966,
    "addressCount": 19567,
    "outputCount": 283221,
    "assetCount": 180
  }
}

### Address Chains

Returns the chains that 1 or more addresses has been seen on.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| address | address | An address to filter results by. Can be supplied multiple times. | None | None

#### **Example Call**

```shell
curl "http://127.0.0.1:8080/v2/addressChains?address=avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u&address=avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3"
```

#### **Example Response**

```json
{
  "addressChains": {
    "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3": [
      "11111111111111111111111111111111LpoYY"
    ],
    "avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u": [
      "11111111111111111111111111111111LpoYY",
      "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM"
    ]
  }
}
```










-----

### List Transactions

Find transactions confirmed transactions from the network.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| chainID | id | A chain ID to filter results by. Can be supplied multiple times. | None | None
| address | address | An address to filter results by. Can be supplied multiple times. | None | None
| assetID | id | An asset ID to filter results by. | None | None
| disableGenesis | bool | When true, the data for the Genesis vertex is not returned. | true | N/A
| sort | string | A method to sort results by, can be `timestamp-asc` or `timestamp-desc`  | `timestamp-desc` | N/A

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/transactions?limit=1&chainID=11111111111111111111111111111111LpoYY&offset=100"
```

#### **Example Response**


```json
{
  "startTime": "0001-01-01T00:00:00Z",
  "endTime": "2020-11-16T04:18:07Z",
  "transactions": [
    {
      "id": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX",
      "chainID": "11111111111111111111111111111111LpoYY",
      "type": "add_validator",
      "inputs": [
        {
          "output": {
            "id": "G2Jq9fj6atW1jvJDTXJKHSkMhRWdrsFuafPpR98DK3izZdfqT",
            "transactionID": "11111111111111111111111111111111LpoYY",
            "outputIndex": 14025,
            "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
            "outputType": 7,
            "amount": "2000000000000",
            "locktime": 0,
            "threshold": 1,
            "addresses": [
              "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3"
            ],
            "timestamp": "2020-09-10T00:00:00Z",
            "redeemingTransactionID": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX"
          },
          "credentials": [
            {
              "address": "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3",
              "public_key": "AgSmTeCLGsNhKvSbRIi01jswlr2fV+C/tv3v86Ty4eDQ",
              "signature": "Ms5KquahoTfLGeIl5s6iP5r1fj15lm5MmrMahu8X7L0m5UTyRBRmcXnniURFaJP6X8dCL9f46t8zYawXscdgkQE="
            }
          ]
        }
      ],
      "outputs": [
        {
          "id": "U7M4jk3y7KGWPmSoeS4WhBX6qNHGtkDtQ5dSzYiaw4rmZ92yE",
          "transactionID": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX",
          "outputIndex": 0,
          "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
          "outputType": 7,
          "amount": "2000000000000",
          "locktime": 0,
          "threshold": 1,
          "addresses": [
            "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3"
          ],
          "timestamp": "2020-10-10T07:09:44Z",
          "redeemingTransactionID": ""
        }
      ],
      "memo": "AAAAAA==",
      "inputTotals": {
        "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": "2000000000000"
      },
      "outputTotals": {
        "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": "2000000000000"
      },
      "reusedAddressTotals": null,
      "timestamp": "2020-10-10T07:09:44Z",
      "txFee": 0,
      "genesis": false
    }
  ]
}
``` 

### Get Transaction

Find a single transaction by its ID.

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/transactions/2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX"
```

#### **Example Response**

```json
{
  "id": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX",
  "chainID": "11111111111111111111111111111111LpoYY",
  "type": "add_validator",
  "inputs": [
    {
      "output": {
        "id": "G2Jq9fj6atW1jvJDTXJKHSkMhRWdrsFuafPpR98DK3izZdfqT",
        "transactionID": "11111111111111111111111111111111LpoYY",
        "outputIndex": 14025,
        "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
        "outputType": 7,
        "amount": "2000000000000",
        "locktime": 0,
        "threshold": 1,
        "addresses": [
          "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3"
        ],
        "timestamp": "2020-09-10T00:00:00Z",
        "redeemingTransactionID": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX"
      },
      "credentials": [
        {
          "address": "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3",
          "public_key": "AgSmTeCLGsNhKvSbRIi01jswlr2fV+C/tv3v86Ty4eDQ",
          "signature": "Ms5KquahoTfLGeIl5s6iP5r1fj15lm5MmrMahu8X7L0m5UTyRBRmcXnniURFaJP6X8dCL9f46t8zYawXscdgkQE="
        }
      ]
    }
  ],
  "outputs": [
    {
      "id": "U7M4jk3y7KGWPmSoeS4WhBX6qNHGtkDtQ5dSzYiaw4rmZ92yE",
      "transactionID": "2jEugPDFN89KXLEXtf5oKp5spsJawTht2zP4kKJjkQwwRsDdLX",
      "outputIndex": 0,
      "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "outputType": 7,
      "amount": "2000000000000",
      "locktime": 0,
      "threshold": 1,
      "addresses": [
        "avax14q43wu6wp8fs745dt6y5s0a02vx57ypq4xc5s3"
      ],
      "timestamp": "2020-10-10T07:09:44Z",
      "redeemingTransactionID": ""
    }
  ],
  "memo": "AAAAAA==",
  "inputTotals": {
    "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": "2000000000000"
  },
  "outputTotals": {
    "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": "2000000000000"
  },
  "reusedAddressTotals": null,
  "timestamp": "2020-10-10T07:09:44Z",
  "txFee": 0,
  "genesis": false
}
```

### List Addresses

Find addresses that have been involved in confirmed transactions.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| chainID | id | A chain ID to filter results by. Can be supplied multiple times. | None | None
| address | address | An address to filter results by. Can be supplied multiple times. | None | None

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/addresses?limit=1"
```

#### **Example Response**

```json
{
  "addresses": [
    {
      "address": "avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u",
      "publicKey": null,
      "assets": {
        "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": {
          "id": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
          "transactionCount": 2,
          "utxoCount": 17,
          "balance": "39561999999996",
          "totalReceived": "39561999999996",
          "totalSent": "0"
        }
      }
    }
  ]
}
```

### Get Address

Find a single address by its ID.

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/addresses/avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u"
```

#### **Example Response**

```json
{
  "address": "avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u",
  "publicKey": null,
  "assets": {
    "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z": {
      "id": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "transactionCount": 2,
      "utxoCount": 17,
      "balance": "39561999999996",
      "totalReceived": "39561999999996",
      "totalSent": "0"
    }
  }
}
```

### List Assets

Find assets that have been created on the X-chain.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| enableAggregate | bool | When true, aggregated data about the asset will be included. | false | N/A

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/assets?limit=1&enableAggregate=true"
```

#### **Example Response**

```json
{
  "assets": [
    {
      "id": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "chainID": "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM",
      "name": "Avalanche",
      "symbol": "AVAX",
      "alias": "AVAX",
      "currentSupply": "24509771588234718",
      "timestamp": "2020-09-10T00:00:00Z",
      "denomination": 9,
      "variableCap": 0,
      "aggregates": {
        "day": {
          "startTime": "2020-11-15T04:47:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "0",
          "transactionCount": 0,
          "addressCount": 0,
          "outputCount": 0,
          "assetCount": 0
        },
        "hour": {
          "startTime": "2020-11-16T03:47:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "0",
          "transactionCount": 0,
          "addressCount": 0,
          "outputCount": 0,
          "assetCount": 0
        },
        "minute": {
          "startTime": "2020-11-16T04:46:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "0",
          "transactionCount": 0,
          "addressCount": 0,
          "outputCount": 0,
          "assetCount": 0
        },
        "month": {
          "startTime": "2020-10-17T04:47:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "0",
          "transactionCount": 0,
          "addressCount": 0,
          "outputCount": 0,
          "assetCount": 0
        },
        "week": {
          "startTime": "2020-11-09T04:47:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "0",
          "transactionCount": 0,
          "addressCount": 0,
          "outputCount": 0,
          "assetCount": 0
        },
        "year": {
          "startTime": "2019-11-17T04:47:00Z",
          "endTime": "2020-11-16T04:47:00Z",
          "transactionVolume": "6637657099999996",
          "transactionCount": 1,
          "addressCount": 159,
          "outputCount": 1,
          "assetCount": 817
        }
      }
    }
  ]
}
```

### Get Asset

Find a single asset by its ID.

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/assets/FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z?enableAggregate=true"
```

#### **Example Response**

```json
{
  "id": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
  "chainID": "2oYMBNV4eNHyqk2fjjV5nVQLDbtmNJzq5s3qs3Lo6ftnC6FByM",
  "name": "Avalanche",
  "symbol": "AVAX",
  "alias": "AVAX",
  "currentSupply": "24509771588234718",
  "timestamp": "2020-09-10T00:00:00Z",
  "denomination": 9,
  "variableCap": 0,
  "aggregates": {
    "day": {
      "startTime": "2020-11-15T04:50:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "0",
      "transactionCount": 0,
      "addressCount": 0,
      "outputCount": 0,
      "assetCount": 0
    },
    "hour": {
      "startTime": "2020-11-16T03:50:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "0",
      "transactionCount": 0,
      "addressCount": 0,
      "outputCount": 0,
      "assetCount": 0
    },
    "minute": {
      "startTime": "2020-11-16T04:49:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "0",
      "transactionCount": 0,
      "addressCount": 0,
      "outputCount": 0,
      "assetCount": 0
    },
    "month": {
      "startTime": "2020-10-17T04:50:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "0",
      "transactionCount": 0,
      "addressCount": 0,
      "outputCount": 0,
      "assetCount": 0
    },
    "week": {
      "startTime": "2020-11-09T04:50:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "0",
      "transactionCount": 0,
      "addressCount": 0,
      "outputCount": 0,
      "assetCount": 0
    },
    "year": {
      "startTime": "2019-11-17T04:50:00Z",
      "endTime": "2020-11-16T04:50:00Z",
      "transactionVolume": "6637657099999996",
      "transactionCount": 1,
      "addressCount": 159,
      "outputCount": 1,
      "assetCount": 817
    }
  }
}
```








---










### List Outputs

Find outputs that have been created by a transaction confirmed on the network.

#### **Params**

| Name | Type | Description | Default | Max
| --- | --- | --- | --- | ---
| chainID | id | A chain ID to filter results by. Can be supplied multiple times. | None | None
| address | address | An address to filter results by. Can be supplied multiple times. | None | None
| spent | bool | If set, results will be filtered by whether they're spent (true) or unspent (false) | None | N/A

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/outputs?limit=1&spent=false"
```

#### **Example Response**

```json
{
  "outputs": [
    {
      "id": "114RMPhYM7do7cDX7KWSqFeLkbUXFrLKcqPL4GMdjTvemPzvc",
      "transactionID": "dhU8aMRrtMWvBWSh41aTxUbwArYootNUBcL3N3UJXVPL8H9ip",
      "outputIndex": 4,
      "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
      "outputType": 7,
      "amount": "2327176470588",
      "locktime": 0,
      "threshold": 1,
      "addresses": [
        "avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u"
      ],
      "timestamp": "2020-09-10T00:00:00Z",
      "redeemingTransactionID": ""
    }
  ]
}
```

### Get Output

Find a single output by its ID.

#### **Example Call**

```sh
curl "http://127.0.0.1:8080/v2/outputs/114RMPhYM7do7cDX7KWSqFeLkbUXFrLKcqPL4GMdjTvemPzvc"
```

#### **Example Response**

```json
{
  "id": "114RMPhYM7do7cDX7KWSqFeLkbUXFrLKcqPL4GMdjTvemPzvc",
  "transactionID": "dhU8aMRrtMWvBWSh41aTxUbwArYootNUBcL3N3UJXVPL8H9ip",
  "outputIndex": 4,
  "assetID": "FvwEAhmxKfeiG8SnEvq42hc6whRyY3EFYAvebMqDNDGCgxN5Z",
  "outputType": 7,
  "amount": "2327176470588",
  "locktime": 0,
  "threshold": 1,
  "addresses": [
    "avax1y8cyrzn2kg4udccs5d625gkac7a99pe452cy5u"
  ],
  "timestamp": "2020-09-10T00:00:00Z",
  "redeemingTransactionID": ""
}
```

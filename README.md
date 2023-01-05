# T.I.T.S. API
## Contents
- [API Details](#api-details)
	- [Data-Endpoint](#data-endpoint)
		- [Get List of available items](#get-list-of-available-items)
	- [Input-Endpoint](#input-endpoint)
		- [Throw Items](#throw-items)
	- [Event-Endpoint](#event-endpoint)
		- [On Getting Hit](#on-getting-hit)

# API Details
The T.I.T.S. Websocket Server runs on `ws://localhost:42069` .
These API documentations are EXTREMELY early in the development of the T.I.T.S. API and there is a lot of room for expansion!

All requests & responses have the following headers:

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "myMessageRequest"
}
```
**`RESPONSE`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "myMessageResponse"
}
```
# Data-Endpoint
The Data-Endpoint is designed around fetching data from T.I.T.S. (Example: Getting a list of all available items)
The Endpoint is accessed over the address `ws:localhost:42069/Data`.
## Get List of available items
This request lets you get a list of all available items in the users Model-Importer

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSItemListRequest"
}
```
**`RESPONSE`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSItemListResponse",
  "data": {
    "items": [
      {
        "ID": "60c678fc-60d1-4740-92d8-30f8bf42afc9",
        "name": "apple"
      },
      {
        "ID": "16d75b54-f4f4-4f97-a946-8ddb21eb2820",
        "name": "banana"
      }
    ]
  }
}
```
# Input-Endpoint
The Input-Endpoint is designed around the plugin sending instructions for T.I.T.S. to do something (like throwing items)
The Endpoint is accessed over the address `ws://localhost:42069/Input`.
## Throw Items
This request lets you control the throws of T.I.T.S.

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSThrowItemsRequest"
  "data":{
  	"items": [
  		"d1a7f8d2-0aee-404f-b75e-4c2004d6122d",
		"c502b3a7-49ed-4154-acbb-7fdbec078f9f"
    ],
    "delayTime": 0.05,
    "amountOfThrows": 50
  }
}
```
**`RESPONSE`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSThrowItemsResponse",
  "data": {
    "numberOfThrownItems": 50
  }
}
```
# Event-Endpoint
The Event-Endpoint is designed around receiving events whenever something happens in T.I.T.S. (Example: Event gets triggered upon the user getting hit)
The Endpoint is accessed over the address `ws:localhost:42069/Events`.
## On Getting Hit
This Event gets broadcasted whenever the player gets hit!

**`BROADCAST`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSHitEvent",
  "data": {
    "itemID": "d1a7f8d2-0aee-404f-b75e-4c2004d6122d",
    "strength": 24.5,
    "direction": {
      "x": 0.3261459,
      "y": 0.9256075,
      "z": 0.1920408,
      "magnitude": 1.0,
      "sqrMagnitude": 1.0
    }
  }
}
```

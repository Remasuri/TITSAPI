# T.I.T.S. API
## Contents
- [API Details](#api-details)
	- [Websocket-Endpoint](#websocket-endpoint)
		- [Get List of available items](#get-list-of-available-items)
		- [Throw Items](#throw-items)
		- [Get List of available Triggers](#get-list-of-available-triggers)
		- [Activate a trigger](#activate-a-trigger)
	- [Event-Endpoint](#event-endpoint)
		- [On Getting Hit](#on-getting-hit)
		- [On Trigger Activated](#on-trigger-activated)
		- [On Trigger Ended](#on-trigger-ended)

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
# Websocket-Endpoint
The Data-Endpoint is designed around fetching data from T.I.T.S. (Example: Getting a list of all available items)
The Endpoint is accessed over the address `ws:localhost:42069/websocket`.
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
        "name": "apple",
	"encodedImage": "someBase64EncodedString"
      },
      {
        "ID": "16d75b54-f4f4-4f97-a946-8ddb21eb2820",
        "name": "banana",
	"encodedImage": "someBase64EncodedString"
      }
    ]
  }
}
```
`encodedImage` is a Base64 encoded PNG-Image with the dimensions 256x256 pixels.
## Throw Items
This request lets you control the throws of T.I.T.S.

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSThrowItemsRequest",
  "data":{
  	"items": [
  		"d1a7f8d2-0aee-404f-b75e-4c2004d6122d",
		"c502b3a7-49ed-4154-acbb-7fdbec078f9f"
    ],
    "delayTime": 0.05,
    "amountOfThrows": 50,
    "errorOnMissingID": false
  }
}
```
`items` is a list of Item-IDs, where T.I.T.S. will randomly select one of those IDs each throw. You can include duplicate IDs to increase the probability of an item.
`delayTime` refers to the time in s between the throws. By default it is set to 0.05s.
`amountOfThrows` tells T.I.T.S. how many items to throw. 
`errorOnMissingID` tells T.I.T.S. to send an Error-Response if one of the Item-IDs was not found.

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
## Get List of available Triggers
This request will send oyu a list of all available triggers in the users T.I.T.S.-Setup.

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerListRequest"
}
```

**`RESPONSE`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerListResponse",
  "triggerCount": 2,
  "data": {
    "triggers": [
      {
        "ID": "670950d5-ff0e-44a2-96ee-69ab9363da31",
        "name": "New Follow"
      },
      {
        "ID": "ebb7170b-40e6-4832-8b05-bc6c809d79f1",
        "name": "Throw Bananas"
      }
    ]
  }
}
```
## Activate a Trigger

**`REQUEST`**
```json
{
  "apiName": "TITSPublicApi",
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerActivateRequest",
  "data": {
    "triggerID": "a0ce3831-3c3e-4474-b4e4-19ab98dc73dd"
  }
}
```
`triggerID` refers to the ID of the trigger you want to activate

**`RESPONSE`**
```json
{
  "apiName": "TITSPublicApi",
  "timestamp": 133178785185437633,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerActivateResponse"
}
```
# Event-Endpoint
The Event-Endpoint is designed around receiving events whenever something happens in T.I.T.S. (Example: Event gets triggered upon the user getting hit)
The Endpoint is accessed over the address `ws:localhost:42069/events`.
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

## On Trigger Activated
This event gets broadcasted whenever a trigger gets activated!

**`BROADCAST`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerActivatedEvent",
  "data": {
    "triggerID": "d1a7f8d2-0aee-404f-b75e-4c2004d6122d"
  }
}
```

## On Trigger Ended
This event gets broadcasted whenever a trigger threw its last item!

**`BROADCAST`**
```json
{ 
  "apiName": "TITSPublicApi",
  "timestamp": 133173178563183532,
  "apiVersion": "1.0",
  "requestID": "someID",
  "messageType": "TITSTriggerEndedEvent",
  "data": {
    "triggerID": "d1a7f8d2-0aee-404f-b75e-4c2004d6122d"
  }
}
```

---
title: Campaigns API
date: 2016-02-29 14:27:54
---

# Campaigns

> `?cuid=<customer ID>` query string parameter is needed for all Campaigns endpoints below.

### **POST** /campaign

```javascript
{
    name: STRING,
    start: INT (Timestamp),
    end: INT (Timestamp),
    frequency: INT,
    dailySpend: FLOAT (2 decimal places)
}
```
_Create a new campaign for the given customer_

**Response** will include `{cid: [CAMPAIGN ID]}` to be used in future API calls

### **GET** /campaign
**Requires cuid as query string** 
```javascript
[
  // Example Response
  {
    "cid": INT (cid),
    "cuid": INT (cuid as supplied),
    "start": 1432339200,
    "end": 1436572740,
    "name": "Campaign 1",
    "enabled": BIT (true or false),
    "freq": INT (Offer frequency cap),
    "dailySpend": FLOAT (2 decimal places)
  },
  {
    "cid": INT (cid),
    "cuid": INT (cuid as supplied),
    "start": 1433980800,
    "end": 1435967940,
    "name": "Campaign 2",
    "enabled": BIT (true or false),
    "freq": INT (Offer frequency cap),
    "dailySpend": FLOAT (2 decimal places)
  }
]
```
_Get a list of owned campaigns by the advertiser.._

### **GET** /campaign/**:cid**
**Requires cuid as query string** 
```javascript
// Example Response
{
  "cid": INT (cid),
  "cuid": INT (cuid as supplied),
  "start": 1432339200,
  "end": 1436572740,
  "name": "Campaign 1",
  "enabled": BIT (true or false),
  "freq": INT (Offer frequency cap),
  "dailySpend": FLOAT (2 decimal places)
}
```
_Get a specific campaign owned by the advertiser._

### **PUT** /campaign/**:cid**

__*__ Denotes optional parameter
```javascript
{
    *name: STRING,
    *start: INT (Timestamp),
    *end: INT (Timestamp),
    *frequency: INT,
    *dailySpend: FLOAT (2 decimal places)
}
```
_Edit a campaign owned by the advertiser. Only supplied fields are edited. $set behaviour_

### **PUT** /campaign/**:cid**/pause

_Pause the given campaign_

### **PUT** /campaign/**:cid**/resume

_Resume the given campaign_

### **DELETE** /campaign/**:cid**

_Removes the campaign from API access._

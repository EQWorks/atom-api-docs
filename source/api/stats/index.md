---
title: Stats API
date: 2016-02-29 14:27:54
---

# Stats

In the following routes :id refers to CID or FID or PID. All of the following requires `cuid` as query string.

### **GET** /stats/:id/summary

_Get the summary stats for a campaign_

Sample response:

```javascript
{
  "id": <:id>,
  "imps": 0,
  "clicks": 0,
  "actions": 0,
  "spend": 0,
  "ctr": 0,
  "cnvr": 0
}
```

### **GET** /stats/:id/timeseries

* `start` can be supplied as query string in form of Unix timestamp *  
* `end` can be supplied as query string in form of Unix timestamp *  

_Get the timeseries stats for a campaign_

Sample response:

```javascript
{
  "id": <:id>,
  "timeseries": [
    {
      "date": <UNIX TIMESTAMP>,
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "spend": 0,
      "ctr": 0,
      "cnvr": 0
    },
    {
      "date": <UNIX TIMESTAMP>,
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "spend": 0,
      "ctr": 0,
      "cnvr": 0
    }
  ]
}
```

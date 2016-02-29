---
title: Stats Breakdowns
date: 2016-02-29 14:34:01
---

# **Breakdown Reporting**

**The following routes provide fast serving data for immediate data visualization**  
**This data will not accept any query parameters, and will be updated daily**

### **GET** /stats/[ID]/domains/breakdown
_Get top 10 domains served to, along with their relative percentage and stats_  
```javascript
// Example Response
{
  "id": [ID],
  "last_updated": [UNIX Timestamp],
  "breakdown": [
    {
      "name": [TLD],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
    {
      "name": [TLD],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
  ]
}
```

### **GET** /stats/[ID]/apps/breakdown
_Get top 10 apps served to, along with their relative percentage and stats_  
```javascript
// Example Response
{
  "id": [ID],
  "last_updated": [UNIX Timestamp],
  "breakdown": [
    {
      "name": [APP NAME],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
    {
      "name": [APP NAME],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
  ]
}
```

### **GET** /stats/[ID]/os/breakdown
_Get stats of each of the OS served to at a high level_  
```javascript
// Example Response
{
  "id": [ID],
  "last_updated": [UNIX Timestamp],
  "breakdown": [
    {
      "name": [OS NAME],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
    {
      "name": [OS NAME],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
  ]
}
```

### **GET** /stats/[ID]/banners/breakdown
_Get basic stats on top 10 banners, including its relative percentage of amount served_  
```javascript
// Example Response
{
  "id": [ID],
  "last_updated": [UNIX Timestamp],
  "breakdown": [
    {
      "name": [Banner Name],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
    {
      "name": [Banner Name],
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
      "percentage": [For ease of use in charting]
    },
  ]
}
```

### **GET** /stats/[ID]/actions/breakdown
_Get basic stats on actions to show the user funnel_  
```javascript
// Example Response
{
  "id": [ID],
  "last_updated": [UNIX Timestamp],
  "breakdown": [
    {
      "name": [Action Name],
      "hierarchy": [The lower the number, the earlier in the pipeline]
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
    },
    {
      "name": [Action Name],
      "hierarchy": [The lower the number, the earlier in the pipeline]
      "imps": 0,
      "clicks": 0,
      "actions": 0,
      "ctr": 0,
    }
  ]
}
```

---
title: Persona API
date: 2016-02-29 14:27:54
---

# Persona

> `?cuid=<customer ID>` query string parameter is needed for all Personas endpoints below.

### **POST** /campaign/**:cid**/persona

__*__ Denotes optional parameter
```javascript
{
    name: STRING,
    geo: {
       geos: [STRING ("CA"), STRING ("CA-ON"), STRING ("CA-ON-Toronto"), ...]
       OR
       radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]
    },
    *cat: [ STRING (IAB1), STRING (IAB20), ... ],
    device: { mobile: BOOL, tablet: BOOL, desktop: BOOL },
    *demo: {
      gender: { male: BOOL, female: BOOL, unknown: BOOL } OR null
      age: { "13to17": true, "18to34": true, "35to49": true, "50+": true} or null
    }
}
```
_Create a new persona for the given campaign_

`cat` must be a valid IAB category of format IAB# or IAB#-# for sub-categories. These will greatly limit the targetting scope to be cautious

`geo` can include either a list of `geos`: [] or a single `radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]`.One or the other **must** be supplied. `geos` must be of the form '[Country Shortcode]-[State/Province Shortcode]-[City Name], with variations allowed for whole countries *eg: 'CA' or 'US'* or whole provinces/states *eg: 'CA-ON, CA-QC, US-FL'.

**Startdate and Enddate will be inherited from the campaign**

**Response** will include `{pid: [PERSONA ID]}` to be used in future API calls

### **GET** /campaign/**:cid**/persona

_Get the list of personas belonging to a campaign._

### **GET** /campaign/**:cid**/persona/**:pid**

_Get a specific persona belonging to a campaign_

### **PUT** /campaign/**:cid**/persona/**:pid**

__*__ Denotes optional parameter
```javascript
{
    *name: STRING,
    *geo: {
       geos: [STRING ("CA"), STRING ("CA-ON"), STRING ("CA-ON-Toronto"), ...]
       OR
       radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]
    },
    *cat: [ STRING (IAB1), STRING (IAB20), ... ],
    *device: { mobile: BOOL, tablet: BOOL, desktop: BOOL },
    *demo: {
      gender: { male: BOOL, female: BOOL, unknown: BOOL } OR null
      age: { "13to17": true, "18to34": true, "35to49": true, "50+": true} or null
    }
}
```
**Consult the `POST /campaign/**:cid**/persona` for field information

_Edit a persona. Only supplied fields are edited. $set behaviour_

### **PUT** /campaign/**:cid**/persona/**:pid**/pause

_Pause the given persona_

### **PUT** /campaign/**:cid**/persona/**:pid**/resume

_Resume the given persona_

### **DELETE** /campaign/**:cid**/persona/**:pid**

_Removes the persona from API access._

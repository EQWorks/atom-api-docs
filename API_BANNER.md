# Banners

> `?cuid=<customer ID>` query string parameter is needed for all Banners endpoints below.

### **POST** /campaign/**:cid**/persona/**:pid**/banner
```javascript
{
    name: STRING,
    height: INT (Pixels),
    width: INT (Pixels),
    clickUrl: STRING,
    creativeUrl: STRING
}
```
_Create and attach a banner under a given persona_

**Banners must conform to allowed ad sizes, and content guidlines**

`creativeURL` is the path to an image banner

**Response** will include `{bid: [BANNER ID]}` to be used in future API calls

### **GET** /campaign/**:cid**/persona/**:pid**/banner
_Get the list of banners for a given persona._

### **PUT** /campaign/**:cid**/persona/**:pid**/banner
```javascript
{
    name: STRING,
    height: INT (Pixels),
    width: INT (Pixels),
    clickUrl: STRING,
    creativeUrl: STRING
}
```
_Edit a specific banner for a given persona_

**Banners must conform to allowed ad sizes, and content guidlines**

`creativeURL` is the path to an image banner

### **DELETE** /campaign/**:cid**/persona/**:pid**/banner/**:bid**
_Detach and hide the given banner._

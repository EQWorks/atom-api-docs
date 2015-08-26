# ATOM API

CRUD API for ATOM, exposing interactions with Customer, Campaign, Persona, and Banners.

## Authentication

All APIs are authenticated by client supplied API `key/token` pair. Authorization on what the `key/token` pair can access depends on the privilege of the `key`.

We utilize SSL to secure the transfer of these values.

There are three ways for client to supply API `key` and `token`:

* __HTTP Header__ keyed on `x-access-key` and `x-access-token`, i.e.:

  `x-access-key: {API_KEY}; x-access-token: {API_TOKEN}`

* __HTTP Body__ in JSON with fields `key` and `token`, i.e.:

  `{ "key": "{API_KEY}", "token": "{API_TOKEN}"`

* __URL Query String__ keyed on `key` and `token`, i.e.:

  `key={API_KEY}&token={API_TOKEN}`

When your API `key/token` are supplied incorrectly, you will get a few possible `HTTP 4xx` returns with JSON body with information in field `message` that are:

* `403` - Missing key
* `403` - Missing token
* `403` - Invalid token
* `404` - No matching key

Under rare conditions, you should also anticipate some `HTTP 500` returns with JSON body explaining which stage the server failed. You can always try again later, and/or contact us to resolve it.

## CRUD API

Most routes in this API will expect a query string of `cuid=[CUSTOMER ID]`, which is returned upon successful creation of a customer.

### Customers
####**POST** /customer

__*__ Denotes optional parameter
```javascript
{
    company: STRING,
    *email: STRING,
    *name: STRING,
    *address: STRING,
    *country: STRING,
    *city: STRING,
    *zip: STRING,
    *state: STRING,
    *phone: STRING,
    *website: STRING
}
```
_Create a new customer record in ATOM_

**Response** will include `{cuid: [ATOM CUSTOMER ID]}` to be used in future API calls

####**GET** /customer/**:cuid**
_Get the information about a specific customer_

### Campaigns
####**POST** /campaign

**Requires cuid as query string**
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

####**GET** /campaign
**Requires cuid as query string**

_Get a list of owned campaigns by the advertiser.._

####**GET** /campaign/**:cid**
**Requires cuid as query string**

_Get a specific campaign owned by the advertiser._

####**PUT** /campaign/**:cid**
**Requires cuid as query string**

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

####**PUT** /campaign/**:cid**/pause
**Requires cuid as query string**

_Pause the given campaign_

####**PUT** /campaign/**:cid**/resume
**Requires cuid as query string**

_Resume the given campaign_

####**DELETE** /campaign/**:cid**
**Requires cuid as query string**

_Removes the campaign from API access._


### Personas
####**POST** /campaign/**:cid**/persona
**Requires cuid as query string**

__*__ Denotes optional parameter
```javascript
{
    name: STRING,
    *start: INT (Timestamp),
    *end: INT (Timestamp),
    geo: {
       geos: [STRING ("CA"), STRING ("CA-ON"), STRING ("CA-ON-Toronto"), ...]
       OR
       radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]
    },
    *cat: [ STRING (IAB1), STRING (IAB2-2), ... ],
    device: { mobile: BOOL, tablet: BOOL, desktop: BOOL },
    *demo: {
      gender: { male: BOOL, female: BOOL, unknown: BOOL } OR null
      age: { "13to18": BOOL, "18to24": BOOL, "25to34": BOOL, "35to54": BOOL, "55+": BOOL } or null
    }
}
```
_Create a new persona for the given campaign_

`start` and `end` will default to the campaign run times (reccomended not to include), and will be modified with changes to the campaign.

`cat` must be a valid IAB category of format IAB# or IAB#-# for sub-categories. These will greatly limit the targetting scope to be cautious

`geo` can include either a list of `geos`: [] or a single `radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]`.One or the other **must** be supplied. `geos` must be of the form '[Country Shortcode]-[State/Province Shortcode]-[City Name], with variations allowed for whole countries *eg: 'CA' or 'US'* or whole provinces/states *eg: 'CA-ON, CA-QC, US-FL'.

**Response** will include `{pid: [PERSONA ID]}` to be used in future API calls

####**GET** /campaign/**:cid**/persona
**Requires cuid as query string**

_Get the list of personas belonging to a campaign._

####**GET** /campaign/**:cid**/persona/**:pid**
**Requires cuid as query string**

_Get a specific persona belonging to a campaign_

####**PUT** /campaign/**:cid**/persona/**:pid**
**Requires cuid as query string**

__*__ Denotes optional parameter
```javascript
{
    *name: STRING,
    *start: INT (Timestamp),
    *end: INT (Timestamp),
    *geo: {
       geos: [STRING ("CA"), STRING ("CA-ON"), STRING ("CA-ON-Toronto"), ...]
       OR
       radius: INT (meters), geocode: [FLOAT (Lat), FLOAT (Long)]
    },
    *cat: [ STRING (IAB1), STRING (IAB2-2), ... ],
    *device: { mobile: BOOL, tablet: BOOL, desktop: BOOL },
    *demo: {
      gender: { male: BOOL, female: BOOL, unknown: BOOL } OR null
      age: { "13to18": BOOL, "18to24": BOOL, "25to34": BOOL, "35to54": BOOL, "55+": BOOL } or null
    }
}
```
**Consult the `POST /campaign/**:cid**/persona` for field information

_Edit a persona. Only supplied fields are edited. $set behaviour_

####**PUT** /campaign/**:cid**/persona/**:pid**/pause
**Requires cuid as query string**

_Pause the given persona_

####**PUT** /campaign/**:cid**/persona/**:pid**/resume
**Requires cuid as query string**

_Resume the given persona_

####**DELETE** /campaign/**:cid**/persona/**:pid**
**Requires cuid as query string**

_Removes the persona from API access._


### Banners
####**POST** /campaign/**:cid**/persona/**:pid**/banner
**Requires cuid as query string**
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

####**GET** /campaign/**:cid**/persona/**:pid**/banner
_Get the list of banners for a given persona._

####**PUT** /campaign/**:cid**/persona/**:pid**/banner
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

####**DELETE** /campaign/**:cid**/persona/**:pid**/banner/**:bid**
_Detach and hide the given banner._

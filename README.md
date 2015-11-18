# ATOM API

CRUD API for ATOM, exposing interactions with Customer, Campaign, Persona, and Banners.

Contact us for any issues, concerns and questions at <api@atom.works>.

## Authentication

There are **2** levels of API 'key/token' pairs for interaction with the API:

**App level super key/token** Gives your App access to the *Customer Create*, and the *Get Many Customers* routes.

**Customer level key/token** This is provided as a response to the Customer Creation method, and is to be used for interactions with campaigns/banners/personas/etc. for that specific customer (user).

We utilize SSL to secure the transfer of these values.

There are two ways for client to supply API `key` and `token`:

* __HTTP Header__ keyed on `x-access-key` and `x-access-token`, or for App level `x-access-superkey` and `x-access-supertoken`. i.e.:
  * Customer level: `x-access-key: {API_KEY}; x-access-token: {API_TOKEN}`
  * App level: `x-access-superkey: {SUPER_API_KEY}; x-access-supertoken: {SUPER_API_TOKEN}`

* __URL Query String__ keyed on `key` and `token`, or for App level `superkey` and `supertoken`. i.e.:
  * Customer level: `key={API_KEY}&token={API_TOKEN}`
  * App level: `superkey={SUPER_API_KEY}&supertoken={SUPER_API_TOKEN}`

When your API `key/token` are supplied incorrectly, you will get a few possible `HTTP 4xx` returns with JSON body with information in field `message` that are:

* `403` - Missing key
* `403` - Missing token
* `403` - Invalid token
* `404` - No matching key

Under rare conditions, you should also anticipate some `HTTP 500` returns with JSON body explaining which stage the server failed. You can always try again later, and/or contact us to resolve it.

## CRUD API

Most routes in this API will expect a query string of `cuid=[CUSTOMER ID]`, which is returned upon successful creation of a customer.

### Customers
####**POST** /customer (__Requires App level super key/token__)

__*__ Denotes optional parameter
```javascript
{
    company: STRING,
    email: STRING,
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

**Response** will include:
```javascript
{
    cuid: [ATOM CUSTOMER ID],
    key: [AUTH_KEY],
    token: [AUTH_TOKEN]
}
```
These are to be used for that specific customer in future API calls


####**GET** /customer/ (__Requires App level super key/token__)
_Get an array of customer objects belonging to that SUPER USER_


####**GET** /customer/**:cuid**
_Get the information about a specific customer (self)_


####_GET_ /customer/_:email_/passcode (__Requires App level super key/token__)

Sends passcode to a specific customer by `:email`. This is to support the auth workflow that is very similar to <atom.works> the web UI. Your app auth workflow should be something like:

1. Get `:email` from your app user
2. Call this API endpoint to get the passcode generated and sent to the provided `:email`
3. Get passcode from your app user
4. Call `/customer/:identifier/auth` endpoint to retrieve this app user's __Customer level key/token__

__Note:__ this currently only supports emails that match existing <atom.works> or <insights.eqworks.com> web UI users.

On success, it returns status code 200 and JSON
```json
{
  "message": "We have emailed the passcode to: <:email>"
}
```

On failure, the expected errors are:

* `500` - server side error, usually a JSON with `message` field will be returned to give insights.
* `404` - User not found.
* `401` - User not allowed for API access.


####_GET_ /customer/:identifier/auth (__Requires App level super key/token__)

Get newly generated or renewed __Customer level key/token__ with given `:identifier`, which can either be a `cuid` (Customer ID) or `email`. When `:identifier` is `email`, an additional query string arg `passcode=<passcode>` must exist in order for the endpoint to work.

On success, it returns status code 200 and JSON
```json
{
  "cuid": "<Customer ID required by other endpoints>",
  "key": "<Customer level API key>",
  "token": "<Customer level API token>"
}
```

On failure, the expected errors are:

* `500` - server side error, usually a JSON with `message` field will be returned to give insights.
* `404` - User not found.
* `401` - Expired or invalid passcode.
* `401` - User not allowed for API access.


### Campaigns

> `?cuid=<customer ID>` query string parameter is needed for all Campaigns endpoints below.

####**POST** /campaign

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

_Get a list of owned campaigns by the advertiser.._

####**GET** /campaign/**:cid**

_Get a specific campaign owned by the advertiser._

####**PUT** /campaign/**:cid**

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

_Pause the given campaign_

####**PUT** /campaign/**:cid**/resume

_Resume the given campaign_

####**DELETE** /campaign/**:cid**

_Removes the campaign from API access._


### Personas

> `?cuid=<customer ID>` query string parameter is needed for all Personas endpoints below.

####**POST** /campaign/**:cid**/persona

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

####**GET** /campaign/**:cid**/persona

_Get the list of personas belonging to a campaign._

####**GET** /campaign/**:cid**/persona/**:pid**

_Get a specific persona belonging to a campaign_

####**PUT** /campaign/**:cid**/persona/**:pid**

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

####**PUT** /campaign/**:cid**/persona/**:pid**/pause

_Pause the given persona_

####**PUT** /campaign/**:cid**/persona/**:pid**/resume

_Resume the given persona_

####**DELETE** /campaign/**:cid**/persona/**:pid**

_Removes the persona from API access._


### Banners

> `?cuid=<customer ID>` query string parameter is needed for all Banners endpoints below.

####**POST** /campaign/**:cid**/persona/**:pid**/banner
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

### Stats

> `?cuid=<customer ID>` query string parameter is needed for all Stats endpoints below.

####**GET** /stats/campaign/**:cid**
_Get the summary stats for a campaign_

####**GET** /stats/campaign/**:cid**/timeseries
*start can be supplied as query string in form of timestamp*
*end can be supplied as query string in form of timestamp*
_Get the timeseries stats for a campaign_

####**GET** /stats/campaign/**:cid**/persona/**:pid**
_Get the summary stats for a persona_

####**GET** /stats/campaign/**:cid**/persona/**:pid**/timeseries
*start can be supplied as query string in form of timestamp*
*end can be supplied as query string in form of timestamp*
_Get the timeseries stats for a persona_

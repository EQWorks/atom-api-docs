# Customers

### **POST** /customer (__Requires App level super key/token__)

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


### **GET** /customer/ (__Requires App level super key/token__)
_Get an array of customer objects belonging to that SUPER USER_


### **GET** /customer/**:cuid**
_Get the information about a specific customer (self)_


### _GET_ /customer/_:email_/sso (__Requires App level super key/token__)

"Single Sign On" to `ATOM` powered platforms, including [ATOM](https://atom.works) and white labeled ones, for a specific customer by `:email`.

On success, it redirects the user to designated `ATOM` powered platform with a status code 302.

On failure, the expected errors:

* `500` - server side error, usually a JSON with `message` field will be returned to give insights.
* `404` - User not found.
* `401` - User not allowed for API access.

### _GET_ /customer/_:email_/passcode (__Requires App level super key/token__, and special provisioning on App level access)

Sends passcode to a specific customer by `:email`. This is to support the auth workflow that is very similar to <atom.works> the web UI. Your app auth workflow should be something like:

1. Get `:email` from your app user
2. Call this API endpoint to get the passcode generated and sent to the provided `:email`
3. Get passcode from your app user
4. Call `/customer/:identifier/auth` endpoint to retrieve this app user's __Customer level key/token__

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


### _GET_ /customer/:identifier/auth (__Requires App level super key/token__)

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

### _GET_ /customer/:email/auth/nopasscode (__Requires App level super key/token__, and special provisioning on App level access)

Similar to [`/customer/:identifier/auth`](#get-customeridentifierauth-requires-app-level-super-keytoken) except that `:identifier` is limited to only `:email` and behaves exactly like when `:identifier` is a `cuid` (Customer ID) where no passcode is checked. Responses are also identical to `/customer/:identifier/auth` endpoint.


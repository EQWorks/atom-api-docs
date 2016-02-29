# API Authentication

There are __2__ levels of API 'key/token' pairs for interaction with all APIs:

* App level super key/token - Gives your App access to most [Customers API](API_CUSTOMER.md) endpoints.
* Customer level key/token - Provided as a response upon successful customer creation, or credentials renewal. Used to gain access to all Customer specific [CRUD APIs](README.md#crud-apis), and [Stats APIs](README.md#stats-apis).

There are two ways for client to supply API `key` and `token`:

* __(Recommended) HTTP Header__ keyed on `x-access-key` and `x-access-token`, or for App level `x-access-superkey` and `x-access-supertoken`. i.e.:
  	* Customer level: `x-access-key: {API_KEY}; x-access-token: {API_TOKEN}`
  	* App level: `x-access-superkey: {SUPER_API_KEY}; x-access-supertoken: {SUPER_API_TOKEN}`

* __URL Query String__ keyed on `key` and `token`, or for App level `superkey` and `supertoken`. i.e.:
  	* Customer level: `key={API_KEY}&token={API_TOKEN}`
  	* App level: `superkey={SUPER_API_KEY}&supertoken={SUPER_API_TOKEN}`

### Security

We utilize SSL to secure the transfer of these values. However, whenever possible, it is recommended to supply credentials through __HTTP Header__.

### Errors

When your API `key/token` are supplied incorrectly, you will get a few possible `HTTP 4xx` returns with JSON body that contains information in field `message`:

* `403` - Missing key
* `403` - Missing token
* `403` - Invalid token
* `404` - No matching key

Under rare conditions, you should also anticipate some `HTTP 500` returns with JSON body explaining which stage the server failed. You can always try again later, and/or contact us to resolve it.

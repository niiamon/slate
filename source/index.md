---
title: Lukaduka API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='http://www.lukaduka.com/'>Lukaduka Website</a>
  - <a href='http://www.lukaduka.com/signup'>Sign Up for a Lukaduka account</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Lukaduka API! You can use our API to access Lukaduka API endpoints, which can get information on shops, products, sales, orders and order entities in our database.

# Authentication

Lukaduka API server uses OAuth 2.0 for authentication. OAuth services are provided via the Doorkeeper gem. 

In order to make calls to the API server, API clients need a valid API access token from the API server. API clients make a request for an access token at the beginning of their interaction with the API server. All client access tokens have a validity of 2 hours. When the access token expires, the client can send a `refresh_token` request to the API server to expire their current access token and send them a new access token.

## Login

A. A user of a Lukaduka API client logs in using username and password credentials

B. The API client makes a password grant OAuth token request to the API server using the user credentials:

> B.  send POST /oauth/token

```javascript
{
  "grant_type"    : "password",
  "username"      : "user@example.com",
  "password"      : "sekret",
  "client_id"     : "the_client_id",
  "client_secret" : "the_client_secret"
}
```

C. If the user credentials are valid, the API server returns the a JSON object with the following structure to the client: 

> C. Password grant type response 

```javascript
{
  access_token: "f47c579d37193adb22744ff919f5e8b82f166d267d36347dd8b3284255dcb74e",
  expires_in: 7200,
  refresh_token: "c1d34cfbc20a7bf80f808c813c9b53c170f9d9b7d1b467183b7893ff16e363b7",
  scope: "public",
  token_type: "bearer"
}
```

D. If the user credentials are not valid, the API server will return an HTTP 401 error code to the API client and terminate the request

## Refresh Token

A. API client makes a request to API server

B. API server detects that the access token has expired and sends back an `HTTP 401 Unauthorized` response to the client

C. Upon receiving `HTTP 401 Unauthorized` response from the API server, the API client sends a refresh_token request to the API server to request for a new access token:

> C. POST /oauth/token

```javascript
{
  "grant_type"    : "refresh_token",
  "username"      : "user@example.com",
  "password"      : "sekret",
  "client_id"     : "the_client_id",
  "client_secret" : "the_client_secret"
}
```

D. If the expired access token (along with the `client ID` and `client secret`) is valid, the API server will send a new set of access token credentials of the following structure to the API client: 

> D. Refresh token response 

```javascript
{
  access_token: "f47c579d37193adb22744ff919f5e8b82f166d267d36347dd8b3284255dcb74e",
  expires_in: 7200,
  refresh_token: "c1d34cfbc20a7bf80f808c813c9b53c170f9d9b7d1b467183b7893ff16e363b7",
  scope: "public",
  token_type: "bearer"
}
```

E. If any of the data in the `refresh_token` request are invalid, the API server will return a `HTTP 401 Unauthorized` response to the API client.

## Logout

A. Send a `DELETE` request to the API server

> A. ```DELETE /users/signout```

B. If successful, `HTTP 200` will be returned. Appropriate error message will be returned in any other case.

# Request Headers 

Clients must use request headers accordingly:

A. All GET requests return JSON and thus must set compatible accept headers, either application/json or \*/\*.

B. All PUT or PATCH requests must contain a valid JSON body with the Content-Type header set to application/json.

# Required Request Parameters

All authenticated requests to the API server must contain the following parameters:

> Required parameters example

```javascript
?v=HEAD&access_token=a5d75c788e87e76b89a6238dcf1241dae1234
```

A. `v`: This parameter sets the API version that is requested.

B: `access_token`: This parameter sets the client authentication token received after a successful login to the API server.

## Request Interceptors

It would make sense for a client application to have an HTTP request/response interceptor that intercepts all departing HTTP requests from the application and then appends the two required parameters transparently to the internal code. 

Some links to request interceptors for popular frameworks:

#### AngularJS:

- [Official AngularJS Documentation](https://docs.angularjs.org/api/ng/service/$http)
- [Lukaduka Merchant Dashboard implementation](https://github.com/niiamon/lukaduka-merchant-dashboard/blob/develop/angular-lukaduka-merchant-backend/app/scripts/app.js): Here we intercept a 401, refresh the OAuth token against the API server and then reissue the request transparently to the human
- [Bruno Scopelliti](http://blog.brunoscopelliti.com/http-response-interceptors)

#### Android:

- [Official Android Documentation](http://developer.android.com/reference/org/apache/http/HttpRequestInterceptor.html)
- [Square Retrofit](https://github.com/square/retrofit) - This is a client for Android which enables Android apps consume resources from an API server

### iOS:

- [NSURLProtocol](http://nshipster.com/nsurlprotocol/)
- [AFNetworking](https://github.com/AFNetworking/AFNetworking)
- [Agent](https://github.com/hallas/agent)
- [Cumulus](https://github.com/FivesquareSoftware/Cumulus)

# API Endpoints

# Models

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import 'kittn'

api = Kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Isis",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import 'kittn'

api = Kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/3"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Isis",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">If you're not using an administrator API key, note that some kittens will return 403 Forbidden if they are hidden for admins only.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the cat to retrieve

  
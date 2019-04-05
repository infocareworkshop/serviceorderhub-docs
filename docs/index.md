# API basics

## Naming convention

### Endpoints

All endpoints are named with following schema:

`https://serviceorderhub.com/api/version/action` or
`https://serviceorderhub.com/api/version/entity/action`

Currently `v1` and `v2` are valid `version`. Words inside path string
must be separated with `-` sign. By default all endpoints of previous versions are
allowed in next.  

## Input

### Charset

Only `utf-8` charset is supported.

### Content type

Set `Content-Type` header to `application/json` if you want to pass JSON data in your request.

`Content-Type: application/json`

### Encoding

You can pass `gzip` or `deflate` inside `Accept-Encoding` header,
or omit it.

`Accept-Encoding: gzip`

### Parameters

All parameters inside query string are notation indifferent. You can use
either `camelCase`, `snake_case` or even `kebab-case`. Any of
these notations are valid. Next in this document only camelCase will be
used. Any unrecognized parameters (not described in this document) will
throw validation error and `400` status code.

For GET requests parameters should be passed with GET parameters inside
query string.

Example:

`/api/v1/accessory?accessToken=my_key&productType=1009`

For POST requests only `accessToken` is allowed inside GET parameters.
Anything other must be stored inside request body.

### Languages

Language and locale are defined inside user settings. Typically one user
is assigned to single country and single locale (but this can be
improved in future).

You can pass `Accept-Language` header to determine in which locale
output data should be. For example:

`Accept-Language: sv-se, da-dk`

Currently only followed locales are supported:

-  sv-se
-  da-dk
-  nb-no
-  fi-fi

*This list may be continued*

**Note** This feature works only for "common" data like accessory, or
product types.

### Accept data types

You can pass `Accept` HTTP header. Following types are valid:

-  `text/html`
-  `application/json`
-  `text/xml` - Currently not implemented

Response will contain the same `Content-type` as request's `Accept`.
When `text/html` is selected, output will be wrapped inside simple
HTML page (This is useful for debug).

## Output

Currently only JSON and HTML output is supported. *XML may be added in
future*

### JSON

Each response is wrapped with object contained `data`
field. Payload is stored inside `data` field. When everything is ok
response body will be like this:

```
{
  "data": [ // Payload
    { "id": 1, name: "Something" },
    { "id": 2, name: "Something other" }
  ]
}
```

Also some extra fields may be added, for example offset/limit for
pagination

When an error is occurred response body will be like this:

```
{
  "error": {                // Exists only if there are any recognized reason of the error
    "message": "Validation error"
  },
  "details": { // Optional
  
  }
}
```

### HTML

HTML output is just a "prettified" version of JSON

## Authentication

Each request to API must be followed with special **authentication
token**. Requests without it will be rejected with `401` status code.

There are some different ways to add your access token to request:

### X-Hub-Api-Token header

You can pass your access key using **X-Hub-Api-Token** header.

`X-Hub-Api-Token: YOUR_TOKEN`

### X-Service-Provider header

Usually one user is assigned to one service provider.
But in real life service provider can be located in multiple countries or be a mediator. This leads to creating many service providers and users respectively.
To allow single user to represent multiple providers we have option named "*Allow all service providers*".
Users with this option enabled can specify service provider id they want to represent using this header.

`X-Service-Provider: 1`

### accessToken with GET parameters

Just add `accessToken=YOUR_TOKEN` to GET params. For example:

`/some-endpoint/?param1=X&param2=Y&accessToken=YOUR_TOKEN`

### Other methods

Other auth methods will be added soon.

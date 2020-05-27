# Webhooks

During the service's life cycle both participants (the partner, who orders the service, and the service provider) update information in the service order hub.

On certain events we send webhooks. Today we support two transports for them: HTTP to the URL you provide or putting a message into an AMQP queue on your side. Both can be sent only in JSON.

For HTTP transport we support GET/POST/PUT/PATH/DELETE methods and any custom constant headers.

We can set up for each webhook which version of API will be used to generate body (Except Status transition where only `v3` is available).

By default we use `v1`.

## New status

For more information about statuses [read here](Working%20with%20cases/#post-apiv3casestatus)

Example:
```
{
  "guid": "a7afaf9-7c6e-4d0e-b756-16b4afa1382f", // Case guid
  "name": "Repair finished",
  "key": "final",
  "createdAt": "2019-01-22T11:29:38.258Z",
  "updatedAt": "2019-01-22T11:29:38.258Z",
  "declaredAt": "2019-01-22T11:27:34.233Z"
}
```

## Status transition

We can set up webhooks to send them conditionally:

- If new status has specific key
- If some specific status was changed to any other
- If some specific status changed to another specific status

The webhook body will be almost the same as for new status, but with addition of `prevKey` and `case` fields.

```
{
  "id": 123,
  "guid": "a7afaf9-7c6e-4d0e-b756-16b4afa1382f", // Case guid
  "name": "Repair finished",
  "key": "final", // Current status key
  "prevKey": "arrived", // Previous status key
  "createdAt": "2019-01-22T11:29:38.258Z",
  "updatedAt": "2019-01-22T11:29:38.258Z",
  "declaredAt": "2019-01-22T11:27:34.233Z",
  "case": {
    // Case data
  }
}
```

We use partner statuses after mapping for status webhooks for `key` and `prevKey`.
When there is no any mapping found, we use provider status as is. 
We can add some statuses to "black list" and webhooks won't be sent and they will be invisible.
This feature is useful when provider has many intermediate statuses and partner is interested only in some of them.

Also note that both "New status" and "Status transition" can be fired simultaneously.


## New cost proposal

For more information about cost proposals [read here](Cost%20proposal%20API/)

Example:
```
{
  "id": 25532, // unique cost proposal id
  "guid": "f5afa004-f48c-4380-9f9b-cafaea0dd7de", // Case guid
  "externalId": null,
  "name": "RUR", //most common values are "Repair", "RUR" (Return UnRepaired), "Scrap"
  "priceNet": 280,
  "currency": "SEK",
  "isActive": true,
  "dateValidUntil": null,
  "reaction": null,
  "comment": null,
  "reactionDate": null,
  "reactionName": null,
  "details": [
    {
      "line": 1,
      "text": "Return unrepaired fee",
      "type": "RUR",
      "priceNet": 280,
      "quantity": 1
    }
  ],
  "createdAt": "2019-11-25T11:30:29.000Z",
  "updatedAt": "2019-11-25T10:43:20.523Z"
}
```

## New message

For more information about messages [read here](Messages/)

This webhook can't be send to the party who actually sent the message, only to the party receieving it

Example:
```
{
  "id": 59076, // message's unique id
  "guid": "f5afa004-f48c-4380-9f9b-cc9afa0dd7de", // case guid
  "content": "The external workshop updated the status to Product received.",
  "type": "serviceComment",
  "createdAt": "2019-11-25T10:33:54.283Z",
  "updatedAt": "2019-11-25T10:33:54.283Z"
}
```

## New file uploaded

For more information about file upload [read here](Working%20with%20cases/#post-apiv3casefiles)

Example:
```
{
  "guid": "f5afa004-f48c-4380-9f9b-cc9afa0dd7de", // case guid
  "files": [
    {
      "url": "http://publicly-available-link-to-download-the-file.com/2a6bc44d-81b2-4512-8b83-e6da349fab3f-201920072424556.pdf",
      "name": "201920072424556.pdf",
      "type": "other" // FileType
    }
  ]
}
```
For more information about file types [read here](Data%20types%20and%20structures/#FileType)
## Case updated

The webhook's body is similar to the input of [POST /api/v2/case/update](Working%20with%20cases/#post-apiv2caseupdate)

If any key was removed, we send it with `null` value.

Unchanged keys are not being sent in the webhook.

If a key has a new value, we send the new value assigned to that key.

Array is considered as a single value.

The webhook's body will always contain the `guid` field. Also it can have some top-level keys: `orderData`, `productData`, `customer`, `consumer`, `pickupDst`, `returnDst`.

This webhook can be sent either to a partner or to a provider. It depends on the role of the user who submitted the update request.
Also it can be triggered both by an API request and using the Web interface.

Example:
```
{
  "guid": "989d0f73-f87a-4dfe-98eb-554a44233cef",
  "orderData": {
    "refNo": "REF",
    "serial": null
  },
  "productData": {
    "model": "Tesla Cybertruck",
    "accessory": [
      "1180"
    ]
  },
  "consumer": {
    "firstName": "Elon",
    "lastName": "Musk"
  },
  "sender": {
    "address": "Somestreet, 123"
  }
}
```





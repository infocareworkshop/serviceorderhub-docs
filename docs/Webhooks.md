# Webhooks

During the service's life cycle both participants (the partner, who orders the service, and the service provider) update information in the service order hub.

On certain events we send webhooks. Today we support two transports for them: HTTP POST to the URL you provide or putting a message into an AMQP queue on your side. Both can be sent only in JSON.

## Status update

For more information about statuses [read here](Working%20with%20cases/#post-apiv2casefiles)

Example:
```
{
  "guid": "a7afaf9-7c6e-4d0e-b756-16b4afa1382f", // Case guid
  "name": "Repair finished",
  "key": "final",
  "createdAt": "2019-01-22T11:29:38.258Z",
  "updatedAt": "2019-01-22T11:29:38.258Z"
}
```

## New cost proposal

For more information about cost proposals [read here](Cost%20proposal%20API/)

Example:
```
{
  "id": 25532, // unique cost proposal id
  "guid": "f5afa004-f48c-4380-9f9b-cafaea0dd7de", // case id
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
  "messageType": "serviceComment",
  "createdAt": "2019-11-25T10:33:54.283Z",
  "updatedAt": "2019-11-25T10:33:54.283Z"
}
```

## New file uploaded

For more information about file upload [read here](Working%20with%20cases/#post-apiv2casefiles)

Example:
```
{
  "guid": "f5afa004-f48c-4380-9f9b-cc9afa0dd7de", // case guid
  "files": [
    {
      "url": "http://publicly-available-link-to-download-the-file.com/2a6bc44d-81b2-4512-8b83-e6da349fab3f-201920072424556.pdf",
      "name": "201920072424556.pdf"
    }
  ]
}
```
## Case updated

The webhook body is similar to the input of [POST /api/v2/case/update](Working%20with%20cases/#post-apiv2caseupdate)

If some key was removed, we send it with `null` value.

If some key wasn't changed, we don't send it.

If some key has a new value, we send the new value assigned to that key.

Array considered as a single value.

The webhok body will always contain `guid` field. Also it can have some top-level keys: `orderData`, `productData`, `customer`, `consumer`, `pickupDst`, `returnDst`.

The webhook will only trigger if the user who sent the request doesn't belong to any of service providers.
Also it could be triggered both by API request and Web interface.

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





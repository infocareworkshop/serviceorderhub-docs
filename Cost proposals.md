# Cost proposal API

This part of API allows partners to get cost proposals and approve them. Service providers can create or update
cost proposals for cases.

**Note** Mandatory parameters are marked with  \*  sign.

## GET /api/v1/cost-proposals

Get all cost proposals attached to specific *Case*.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| guid\*                 | Guid       | Guid of the case                        |
| active                 | Boolean    | Get proposals allowed for approve only  |

### Example:
```
/api/v1/cost-proposals?accessToken=key&guid=3ebad3cc-61d3-479a-bd92-510e8774ce5a&active=true
```

### Output:
```js
[
  {
    "id": 2,
    "externalId": ["123456", "test"],
    "name": "repair",
    "priceNet": "200.00",
    "currency": "SEK",
    "dateValidUntil": null,
    "isActive": true,
    "dateAccepted": null,
    "approverName": null,
    "details": [
      {
        "line": 1,
        "text": "shipping",
        "type": null,
        "priceNet": 100,
        "quantity": 0
      },
    ]
  },
  {
    "id": 5,
    "externalId": 213214,
    "name": "Scrap",
    "priceNet": "1860.50",
    "currency": "SEK",
    "dateValidUntil": null,
    "dateAccepted": "2017-07-14T10:14:13.037Z",
    "approverName": "Somebody",
    "details": null,
    "createdAt": "2017-07-12T10:14:13.037Z",
    "updatedAt": "2017-07-14T15:54:09.613Z"
  }
]
```

## POST api/v1/cost-proposals/approve

Approve *Cost Proposal*.

*Note* You can approve only one Cost Proposal for case.

### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| id\*                   | Integer    | Id of Cost Proposal (GET parameter)     |
| approverName\*         | String     | Who perfoms this approve (POST body)    |

### Example:
```
/api/v1/cost-proposals/approve?accessToken=key&id=2

{
 "approverName": "John Smith" 
}
```

### Output:
```js
{
  data: { /* Cost Proposal */}
}
```

## POST api/v1/cost-proposals

Add or modify multiple *Cost Proposals*.

*Note* You can approve only one Cost Proposal for case.

### Example:
```
/api/v1/cost-proposals/

[
  {
    "guid": "3ebad3cc-61d3-479a-bd92-510e8774ce5a", // Case's guid
    "externalId": ["123456", "test"], // Array of strings
    "name": "Scrap", // Type of work, pair externalId and name must be unique for Service provider
    "priceNet": 1860.5, // Price (optional)
    "currency": "SEK", // Currency, 3 capital letters (optional)
    "timeStamp": "2017-07-12 13:14:13.037", // date created (optional)
    "isActive": true, // Can we approve this Cost Proposal 
    "dateAccepted": "2017-07-14 13:14:13.037", // when this Cost proposal was been accepted (optional)
    "approverName": "John Smith" // Who performed approve (optional)
    "details": [ // Optional array of details
      {
        "line": 1, // Counter from 1
        "text": "shipping", // Type of work (text)
        "type": null, // Type of work (id)
        "priceNet": 100, // Price
        "quantity": 0 // Quantity
      },
      ...
    ]
  }
]
```

### Output:
```js
{
  data: [/* List of created Cost Proposals */]
}
```

If same pair (externalId, name) passes second time, then an update will be performed.

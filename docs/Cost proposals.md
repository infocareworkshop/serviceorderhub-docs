# Cost proposal API


This part of API allows partners to get cost proposals and approve them.
Service providers can create or update cost proposals for cases.

## Cost proposal concept

Cost proposals in the Service Order Hub can have two states, either they are approved or not. There is no such state as "rejected". Instead it can be many cost proposals with "id" as a unique key. If the partner doesn't want to approve a certain cost proposal they should contact the provider using the messages endpoints of this API, so the provider may issue another one.

One case may have many cost proposals with different ids and names, e.g. four cost proposals: two cost proposals with name "repair", one with name "return", and one with name "scrap". Cost proposals' names can help to group cost proposals on service partner's. Those names are different depending on service provider. 

In the example above, two cost proposals with the name "repair" could appear when the service provider issued another cost proposal for repair maybe because the previous was not correct or accepted. It's easy to sort cost proposals with the same name (read "type") by date since they have timestamps. The older one should also have the parameter "active" set to "false". 

The Service Order Hub stores and returns all cost proposals ever created for the case. The service partner can accept only one cost proposal. If the case already has an accepted cost proposal and the partner accepts another, the previous cost proposal loses the flag "accepted". Please note that once a cost proposal is accepted it may be a legal reason for the service provider to invoice the corresponding amount to the service partner even if the partner later changed their mind and asked for another cost proposal. These conditions are a subject of direct negotiations between service partner and service provider.

It's also not necessary that every case can contain all possible cost proposals. It can contain three, two, ten or only one, e.g. one cost proposal with name "repair".

## Details

Cost proposals can contain an array with details for storing cost specification. The parameter `type` is a free text parameter, but it has some recommended values.


| type       | Description                              |
|------------|------------------------------------------|
| labor      | Costs related to labor                   |
| shipping   | Costs related to shipping                |
| sparePart  | Costs related to spare parts             |
| deductible | Costs related to insurance deductibles   |


**Note** Mandatory parameters are marked with \* sign.

**New in V2** In v2 you can also reject cost proposals.

## GET /api/v3/cost-proposals

Get all cost proposals attached to specific *Case*.

### Access

Partner, Provider

### Input


| Name     | Type      | Description                              |
|----------|-----------|------------------------------------------|
| guid\*   | Guid      | Guid of the case                         |
| active   | Boolean   | Get proposals allowed for approval only  |

### Example

```
/api/v3/cost-proposals?accessToken=key&guid=3ebad3cc-61d3-479a-bd92-510e8774ce5a&active=true
```

### Output

```
[
  {
    "currency": "SEK",
    "details": [
      {
        "line": 1,
        "priceNet": 100,
        "quantity": 0,
        "text": "shipping",
        "type": null
      }
    ],
    "externalId": [
      "654321",
      "test"
    ],
    "guid": "c41cb189-e901-4c1e-8133-1ee630a7fa56",
    "isActive": true,
    "name": "Scrap",
    "priceNet": 100,
    "reaction": "approved",
    "reactionDate": "2019-03-28 00:00:00.000",
    "reactionName": "John Smith",
    "timeStamp": "2019-03-28 00:00:00.000"
  },
  {
    "currency": "SEK",
    "details": [
      {
        "line": 1,
        "priceNet": 1000,
        "quantity": 0,
        "text": "shipping",
        "type": null
      }
    ],
    "externalId": [
      "654322",
      "test"
    ],
    "guid": "c41cb189-e901-4c1e-8133-1ee630a7fa56",
    "isActive": true,
    "name": "Repair",
    "priceNet": 1000,
    "timeStamp": "2019-03-28 00:00:00.000"
  }
]
```


## POST api/v3/cost-proposals

Add or modify multiple *Cost Proposals*.

*Note* You can approve only one Cost Proposal per case.

### Access

Provider

### Example

```
[
  {
    "guid": "3ebad3cc-61d3-479a-bd92-510e8774ce5a", // Case's guid
    "externalId": ["123456", "test"], // Array of strings
    "name": "Scrap", // Type of work, pair externalId and name must be unique for Service provider
    "priceNet": 1860.5, // Price (optional)
    "currency": "SEK", // Currency in ISO 4217 (3 capital letters) (optional)
    "timeStamp": "2017-07-12 13:14:13.037", // date created (optional)
    "isActive": true, // Can this Cost Proposal be approved?
    "reaction": "approved", // "approved" or "rejected" (optional) 
    "reactionDate": "2017-07-14 13:14:13.037", // when this Cost proposal was accepted or rejected (optional)
    "reactionName": "John Smith" // Who performed approval (optional)
    "comment": "Some text", // Some text explanation about approval or rejection
    "details": [ // Optional array of details (specification)
      {
        "line": 1, // Counter from 1
        "text": "Two-way delivery", // Text description
        "type": shipping, // Type of line
        "priceNet": 100, // Price
        "quantity": 1 // Quantity
      },
      ...
    ]
  }
]
```

### Output:

```
{
  "data": [/* List of created Cost Proposals v3 */]
}
```

If the same pair (externalId, name) passes second time, then an update will be performed.

## POST /api/v3/cost-proposals/feedback

Reject or approve single cost proposal

### Access

Partner, Provider

### Input (request body)

| Name             | Type      | Description                            |
|------------------|-----------|----------------------------------------|
| id\*             | Integer   | Id of Cost Proposal                    |
| name\*           | String    | Who perfoms this operation             |
| reaction\*       | String    | `approved` or `rejected`               |
| comment          | String    | Explanation                            |

### Example
```
{
	"id": 8600,
	"name": "John Smith",
	"reaction": "approved",
	"comment": "Optional comment"
}
```

### Output

Same as [here](Cost%20proposals/#get-apiv3cost-proposals)


## GET /api/v1/cost-proposals – Deprecated

Get all cost proposals attached to specific *Case*.

### Access

Partner, Provider

### Input


| Name     | Type      | Description                              |
|----------|-----------|------------------------------------------|
| guid\*   | Guid      | Guid of the case                         |
| active   | Boolean   | Get proposals allowed for approval only  |

### Example

```
/api/v1/cost-proposals?accessToken=key&guid=3ebad3cc-61d3-479a-bd92-510e8774ce5a&active=true
```

### Output

```
[
  {
    "id": 2,
    "externalId": ["123456", "test"],
    "name": "repair",
    "priceNet": "600.00",
    "currency": "SEK",
    "dateValidUntil": null,
    "isActive": true,
    "dateAccepted": null,
    "approverName": null,
    "details": [
      {
        "line": 1,
        "text": "XGHF12 LCD Screen",
        "type": sparePart,
        "priceNet": 500,
        "quantity": 1
      },
      {
        "line": 2,
        "text": "",
        "type": shipping,
        "priceNet": 100,
        "quantity": 1
      }
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

## POST api/v1/cost-proposals/approve – Deprecated

Approve *Cost Proposal*.

*Note* You can approve only one Cost Proposal for case.

### Access

Partner, Provider

### Input

| Name             | Type      | Description                            |
|------------------|-----------|----------------------------------------|
| id\*             | Integer   | Id of Cost Proposal (GET parameter)    |
| approverName\*   | String    | Who perfoms this approve (POST body)   |

### Example

```
/api/v1/cost-proposals/approve?accessToken=key&id=2

{
 "approverName": "John Smith"
}
```

### Output

```
{
"data": { /* Cost Proposal v1 */}
}
```

## POST api/v1/cost-proposals – Deprecated

Add or modify multiple *Cost Proposals*.

*Note* You can approve only one Cost Proposal per case.

### Access

Provider

### Example

```
/api/v1/cost-proposals/

[
  {
    "guid": "3ebad3cc-61d3-479a-bd92-510e8774ce5a", // Case's guid
    "externalId": ["123456", "test"], // Array of strings
    "name": "Scrap", // Type of work, pair externalId and name must be unique for Service provider
    "priceNet": 1860.5, // Price (optional)
    "currency": "SEK", // Currency in ISO 4217 (3 capital letters) (optional)
    "timeStamp": "2017-07-12 13:14:13.037", // date created (optional)
    "isActive": true, // Can this Cost Proposal be approved?
    "dateAccepted": "2017-07-14 13:14:13.037", // when this Cost proposal was accepted (optional)
    "approverName": "John Smith" // Who performed approval (optional)
    "details": [ // Optional array of details (specification)
      {
        "line": 1, // Counter from 1
        "text": "Two-way delivery", // Text description
        "type": shipping, // Type of line
        "priceNet": 100, // Price
        "quantity": 1 // Quantity
      },
      ...
    ]
  }
]
```

### Output:

```
{
  "data": [/* List of created Cost Proposals v1 */]
}
```

If the same pair (externalId, name) passes second time, then an update will be performed.

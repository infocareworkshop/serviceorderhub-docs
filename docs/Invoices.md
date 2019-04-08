# Invoices

## POST /api/v2/invoices

Add or update invoices

### Access

Provider

### Input

Array of Invoices

### Invoice

| Name                      | Type                   | Description                                    |
|---------------------------|------------------------|------------------------------------------------|
| id<sup>1</sup>            | Int                    | Internal id                                    |
| guid\*                    | Guid                   | Guid of the case                               |
| externalId\*              | String(255)            |                                                |
| type                      | String                 | `invoice` or `credit note`, default `invoice`  |
| customData                | Object                 | Any data related to this invoice               |
| priceNet                  | Number                 |                                                |
| currency                  | String(3)              | SEK, NOK, etc...                               |
| isActive                  | Boolean                |                                                |
| dueDate                   | Date                   |                                                |
| isSent                    | Boolean                |                                                |
| isPaid                    | Boolean                |                                                |
| status                    | String(255)            |                                                |
| details                   | Array of InvoiceDetail |                                                |
| createdAt                 | Date                   |                                                |
| updatedAt                 | Date                   |                                                |
| case                      | Case                   |                                                |
| linkedInvoice<sup>2</sup> | String|Invoice         | For credit notes only                          |

<sup>1</sup> Read only field

<sup>2</sup> 

### InvoiceDetail

| Name                 | Type              | Description                               |
|----------------------|-------------------|-------------------------------------------|
| name\*               | String            |                                           |
| count\*              | Int               |                                           |
| article              | String            |                                           |
| vatRate\*            | Number            |                                           |
| vatAmount\*          | Number            |                                           |
| priceExclVat\*       | Number            |                                           |
| priceInclVat\*       | Number            |                                           |
| totalAmountExclVat\* | Number            |                                           |
| totalAmountInclVat\* | Number            |                                           |


### Example

```
[
  {
    "currency": "SEK",
    "customData": {
      "test": true
    },
    "details": [
      {
        "count": 1,
        "name": "test",
        "priceExclVat": 1000,
        "priceInclVat": 1200,
        "totalAmountExclVat": 1000,
        "totalAmountInclVat": 1200,
        "vatAmount": 200,
        "vatRate": 0.2
      }
    ],
    "dueDate": "2019-01-01",
    "externalId": "100001",
    "guid": "734b0739-7dfe-4f6e-a2a7-6a2871458309",
    "priceNet": 10000,
    "type": "invoice"
  },
  {
    "currency": "SEK",
    "customData": {
      "result": 42,
      "test": true
    },
    "details": [
      {
        "count": 1,
        "name": "test",
        "priceExclVat": 1000,
        "priceInclVat": 1200,
        "totalAmountExclVat": 1000,
        "totalAmountInclVat": 1200,
        "vatAmount": 200,
        "vatRate": 0.2
      }
    ],
    "dueDate": "2019-01-01",
    "externalId": "100002",
    "guid": "734b0739-7dfe-4f6e-a2a7-6a2871458309",
    "priceNet": 30000,
    "type": "invoice"
  }
]
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

## POST /api/v2/invoices/find

Find invoices

Allowed fields for search: 

- guid
- externalId
- type
- customData
- priceNet
- currency
- dueDate
- details
- isSent
- isPaid
- status
- createdAt
- updatedAt

### Access

Partner, Provider

### Input

| Name                | Type     | Description               |
|---------------------|----------|---------------------------|
| filters             | Object   | Search query              |
| pagination          | Object   | Pagination settings       |
| pagination.offset   | Int      | Default - 0               |
| pagination.limit    | Int      | Default - 10, max - 100   |

### Example

```
{
  "filters": {
    "createdAt": { "$gt": "2019-01-01" }
  },
  "pagination": {
    "offset": 0,
    "limit": 10
  }
}
```

### Output

```
{
  "data": [
    {
      "case": { /* Attached case */ }
      "createdAt": "2019-04-03T12:30:12.000Z",
      "currency": "SEK",
      "customData": {
        "test": true
      },
      "details": [
        {
          "count": 1,
          "name": "test",
          "priceExclVat": 1000,
          "priceInclVat": 1200,
          "totalAmountExclVat": 1000,
          "totalAmountInclVat": 1200,
          "vatAmount": 200,
          "vatRate": 0.2
        }
      ],
      "dueDate": "2019-01-01T00:00:00.000Z",
      "externalId": "1234567890",
      "guid": "e3df4d74-f559-4e65-b57d-37b5360fb46d",
      "id": 60,
      "isActive": null,
      "isPaid": false,
      "isSent": true,
      "linkedInvoice": null,
      "priceNet": 10000,
      "status": "sent",
      "type": "invoice",
      "updatedAt": "2019-04-03T12:31:04.000Z"
    },
    // ...
  ],
  "pagination": {
    "count": 42,
    "offset": 0
  }
}
```

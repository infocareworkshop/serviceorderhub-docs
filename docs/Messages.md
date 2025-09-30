# Messages API

Service partners can attach messages to cases that can be viewed by
Service provider and exported to their internal systems.

**Note** Mandatory parameters are marked with \* sign.

## POST /api/v3/case-messages/create

Add a new message to specific *Case*. 

Users can reply to other messages using `parentMessageId`.

### Access

Partner, Provider

### Input (request body)


| Name                   | Type         | Description                                |
|------------------------|--------------|--------------------------------------------|
| guid\*                 | Guid         | Guid of the case                           |
| content                | String       | Message text                               |
| type\*<sup>1</sup>     | String(32)   | Message type                               |
| declaredAt<sup>2</sup> | Date         | Actual time when the status was added      |
| externalId             | String(512)  | Reference to the message in another system |
| parentMessageId        | Int          | Our messageId of the message to reply on   |
| userEmail              | String(512)  | Email of the person who wrote the message  |
| userName               | String(512)  | Name of the person who wrote the message   |

<sup>1</sup> Possible values are: `serviceComment`, `customerCenterReply`, `costProposalRejectRur`, `costProposalRejectScrap`, `statusRequest`, `spareParts`, `reminder`, `costProposal`, `shipment`, `forwarding`

<sup>2</sup> If empty, the current time will be used.

### Example

```
{
  "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
  "content": "Test message",
  "type": "serviceComment",
  "declaredAt": "2017-07-25T10:12:54.003Z",
  "externalId": "test-42"
}
```

### Output

```
{
  "id": 1,
  "caseId": 7748,
  "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
  "senderAccountId": 1009,
  "senderType": "partner",
  "content": "Test message",
  "type": "serviceComment",
  "externalId": "test-42",
  "confirmedAt": null,
  "createdAt": "2017-07-26T14:02:02.170Z",
  "updatedAt": "2017-07-26T14:02:02.170Z",
  "declaredAt": "2017-07-25T10:12:54.003Z"
}
```

### Error message

```
{
  "error": {
    "message": "child \"guid\" fails because [\"guid\" is required]"
  }
}
```

**New in V3**

We changed `messageType` to `type` to make naming more consistent.

We added `externalId` to keep track of messages across different systems.

## GET /api/v3/case-messages

Get all messages attached to specific *Case*.

### Access

Partner, Provider

### Input:

| Name     | Type   | Description        |
|----------|--------|--------------------|
| guid\*   | Guid   | Guid of the case   |

### Example:

```
/api/v3/case-messages?accessToken=my_key&guid=1e0abcb7-b94c-4072-a9c0-37072a1ef015
```

### Output

```
[
  {
    "id": 1,
    "caseId": 7748,
    "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
    "senderAccountId": 1009,
    "content": "Hello!",
    "type": "serviceComment",
    "senderType": "provider",
    "externalId": "test-42",
    "confirmedAt": null,
    "createdAt": "2017-07-26T13:43:44.512Z",
    "updatedAt": "2017-07-26T13:43:44.512Z",
    "declaredAt": "2017-07-26T13:43:44.512Z"
  },
  {
    "id": 2,
    "caseId": 7748,
    "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
    "senderAccountId": 1009,
    "content": "Test comemnt",
    "type": "serviceComment",
    "senderType": "provider",
    "externalId": null,
    "confirmedAt": null,
    "createdAt": "2017-07-26T13:54:53.058Z",
    "updatedAt": "2017-07-26T13:54:53.058Z",
    "declaredAt": "2017-07-26T13:54:53.058Z"
  }
]
```

**New in V3**

We changed `messageType` to `type` to make naming more consistent.

We added `externalId` and `confirmedAt`.

We added `senderType` that can be `"partner"`, `"provider"` or `null`

## POST /api/v3/case-messages/confirm

Confirm messages is used to confirm, that the message has been delivered and read by a person responsible of reading it. 
This endpoint will attempt to process as many ids as possible and report any identifiers it was unable to process. **No rollback** will be performed in case of an error.

### Access

Partner, Provider

### Input (request body)

Array of message Ids to confirm (not externalId!)

### Example

```
[4377, 4378, 4379, 4380]
```

### Output

```
{
  "success": true,
  "errors": [],
  "data": {}
}
```

### Error handling

**Partial success**

This endpoint will return **success** (200 code) even if some messages weren't confirmed due to an error, so you should check the "errors" section to make sure everything was processed. Example error:

```
{
  "success": true,
  "errors": [
    {
      "id": 11,
      "message": "Not found" // This message most likely was deleted
    },
    {
      "id": 22,
      "message": "No access" // You don't have access to this case, or use wrong credentials
    }
  ],
  "data": {}
}
```

**Invalid format**

If the request is invalid, a standard error message and the corresponding error code will be sent:

```
{
  "error": {
    "message": "\"value\" at position 0 fails because [\"0\" must be a number]"
  }
}
```

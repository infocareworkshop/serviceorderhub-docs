# Messages API

Service partners can attach messages to cases that can be viewed by
Service provider and exported to their internal systems.

**Note** Mandatory parameters are marked with \* sign.

## POST /api/v3/case-messages/create

Add a new message to specific *Case*.

### Access

Partner, Provider

### Input (request body)


| Name            | Type         | Description                                |
|-----------------|--------------|--------------------------------------------|
| guid\*          | Guid         | Guid of the case                           |
| content         | String       | Message text                               |
| type\*          | String(32)   | Message type (always `serviceComment`)     |
| declaredAt<sup>1</sup> | Date      | Actual time when the status was added |

<sup>1</sup> If empty, the current time will be used.

### Example

```
{
  "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
  "content": "Test message",
  "type": "serviceComment",
  "declaredAt": "2017-07-25T10:12:54.003Z"
}
```

### Output

```
{
  "id": 1,
  "caseId": 7748,
  "guid": "1e0abcb7-b94c-4072-a9c0-37072a1ef015",
  "senderAccountId": 1009,
  "content": "Test message",
  "type": "serviceComment",
  "createdAt": "2017-07-26T14:02:02.170Z",
  "updatedAt": "2017-07-26T14:02:02.170Z",
  "declaredAt": "2017-07-25T10:12:54.003Z"
}
```

**New in V3**

We changed `messageType` to `type` to make naming more consistent.

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
    "createdAt": "2017-07-26T13:54:53.058Z",
    "updatedAt": "2017-07-26T13:54:53.058Z",
    "declaredAt": "2017-07-26T13:54:53.058Z"
  }
]
```

**New in V3**

We changed `messageType` to `type` to make naming more consistent.

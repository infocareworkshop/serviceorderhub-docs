# Other

**Note** Mandatory parameters are marked with \* sign.

## POST /api/v3/case/send-completed-email

Resend final email to customers.

### Access

Partner

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| guid\*             | GUID          | Guid of the case           |
| email              | String        | Receiver's email           |

### Example

```
{
  "guid": "d647067f-20d4-4ac3-891e-a4549e727bee",
  "email": "test@example.com"
}
```

### Output

```
{
  "data": {
     "result": true // Or string with an information message
  }
}
```
    
If `email` is empty then fields of the case will be used.

If email cant't be sent then a string with message will be returned in `result` field (Sometimes emails are disabled in settings).

## GET /api/v3/routing

Determine which workshop will be used to process a case with specific parameters. 
This method also returns competence data.

### Access

Partner

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType     | Originator    | Originator type            |
| bookingType        | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |
| serviceLocation    | Int           | Serivce location Id        |

All parameters are optional by default, but when provided data is not enough the system will throw an error.

### Output for valid case (status code 200)

```
{
  "data": {
    "serviceProvider": {
      "id": 1,
      "name": "InfoCare Workshop",
      "contactData": {
        "id": 30347,
        "name": null,
        "firstName": null,
        "lastName": null,
        "organizationName": "InfoCare Workshop",
        "organizationNumber": null,
        "addName": null,
        "address": "Arabygatan 9",
        "postalCode": "1234",
        "city": "Växjö",
        "countryCode": null,
        "phone": "123456",
        "mobile": null,
        "email": "test@example.com",
        "floor": null,
        "entrance": null,
        "doorCode": null,
        "createdAt": "2017-08-01T14:01:24.083Z"
      }
    },
    "serviceProviderCompetence": {
      "sesam": "Sesam31",
      "persDep": "CSVxo",
      "departmentId": 1,
      "primarySesam": true,
      "id": 1,
      "overrideCode": "infocare-override"
    },
    "servicePartner": {
      "id": 1,
      "name": "InfoCare Workshop SE"
    },
    "servicePartnerCompetence": {
      "id": 1,
      "overrideCode": "infocare-override"
    }
  }
}
```

### Output when there is no suitable competence to process the case (status code 404)

```
{
  "error": {
    "message": "Can't process this case"
  },
  "data": null
}
```
    
### Output when data is not enough (status code 400)

```
{
  "error": "Not enough data",
  "data": {
    "paramsNeeded": [
      "manufacturer",
      "serviceType"
    ]
  }
}
```

## POST /api/v3/requests/log

Save external request to log

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| type\*             | String        |                            |
| status\*           | String        | `success` | `error`        |
| url                | Url           |                            |
| meta\*             | Object        |                            |
| meta.caseId\*      | Int           | Id of the linked case      |
| request            | String        |                            |
| response           | String        |                            |
| message            | String        |                            |

### Example

```
{
  "type": "testRequest",
  "status": "success",
  "url": "https://umbarov.com/",
  "meta": {
    "caseId": 7680
  },
  "request": "{}",
  "response": "Some cool content"
}
```

### Output

External request

## POST /api/v3/requests/reply

Send a reply to webhook.

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| guid\*             | GUID          | Request ID                 |
| status\*           | string        | Status of the reques       |
| message            | string        | Short description          |

Valid values for `status` are `success`, `error` of `toRetry`.

Send `status` or `error` to disable repetition. `toRetry` will resend this request later if there are some attempts left.

# Working with cases

## GET /api/v2/case

Get all info about a certain Case.

### Access

Partner, Provider

### Input

| Name   | Type   | Description   |
|--------|--------|---------------|
| guid\* | GUID   | Case's GUID   |
| includeRequests | Number  | 1   |

If you want to see all requests to external systems performed for this case you can pass
`includeRequests` parameter. Requests will be stored in `externalRequests` field.

### Output

```
{
  "id": 7676,
  "country": "SE",
  "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063",
  "partnerId": 1,
  "serviceProviderId": 2,
  "manufacturerId": 1006,
  "currentStatus": "rejected", // current status of the case
  "productData": { // ProductData
    "model": "test",
    "accessory": [],
    "problemText": "test",
    "purchaseDate": "2017-04-01"
  },
  "orderData": {
    "shipmentData": {
      "guid": "517e4502-c0f9-409c-94bc-8d3a0eb1b2f6",
      "service": "senderella",
      "serviceId": "1000",
      "packageNumber": "00370726201095743185",
      "shipmentNumber": 0,
      "senderAddress": {
        "city": "test",
        "name": "test",
        "address": "test",
        "country": "SE",
        "postalCode": "1234"
      },
      "receiverAddress": {
        "name": "Workshop name",
        "address": "some address",
        "postalCode": "5678",
        "city": "Some city",
        "country": "SE"
      },
    },
    "bookingType": "companyToPrivate",
    "originatorType": "private",
    "clientPostalCode": "2333",
    "pickupDestination": "customer",
    "returnDestination": "customer",
    "integrationsFinished": true
  },
  "serviceTypeId": 1,
  "serviceLocationId": 1,
  "productTypeId": 1004,
  "createdAt": "2017-04-06T13:02:36.506Z",
  "sender": { // ContactData (without type?)
    "id": 29969,
    "name": null,
    "firstName": "test",
    "lastName": "test",
    "organizationName": null,
    "organizationNumber": null,
    "addName": null,
    "address": "testes",
    "postalCode": "2333",
    "city": "rew",
    "countryCode": "se",
    "phone": null,
    "mobile": "23432432234",
    "email": "teses@test.com",
    "floor": null,
    "entrance": null,
    "doorCode": null,
    "createdAt": "2017-04-06T13:02:36.998Z"
  },
  "receiver": ..., // ContactData
  "pickupDst": ..., // ContactData
  "returnDst": ..., // ContactData
  "serviceType": {
    "id": 1,
    "name": "Garanti",
    "properties": {
      "requirePurchaseDate": true
    },
    "externalData": null
  },
  "productType": {
    "id": 1004,
    "name": "Notebook",
    "properties": {
      "requireSerial": true
    },
    "externalData": null
  },
  "manufacturer": {
    "id": 1006,
    "name": "Acer",
    "externalData": null
  },
  "serviceLocation": {
    "id": 1,
    "name": "Verkstadsreparation",
    "externalData": null
  },
  "serviceProvider": {
    "id": 2,
    "name": "TV Repair workshop"
  },
  "servicePartner": {
    "id": 1,
    "name": "InfoCare Workshop"
  },
  "freightTrackings": [
    {
      "id": 3,
      "caseId": 7676,
      "colliNumber": "00370726200178686586",
      "code": "XMLSVK6",
      "textStatus": "Transportinstruktion",
      "eventDateTime": "2017-04-06T13:04:00.000Z"
    }
  ]
}
```

## POST /api/v2/search-cases

Get list of cases

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
    "orderData": { 
      "partnerSpecific": { 
        "partnerName": { 
          "internalReference": 3112312312 
        }
      }
    }
  }
}
```

You can also use special keys `$lt` and `$gt` for "<" and ">"
conditions

```
{
  "filters": { 
    "createdAt": { 
      "$gt": "2017-01-01" 
    }
  }
}
```

*Allowed keys for querying:*

id, guid, partnerId, serviceProviderId, manufacturerId, currentStatus,
productData.\*, orderData.\*, serviceTypeId, serviceLocationId,
productTypeId, createdAt

All Partner and Service provider specific data is stored in
`orderData.partnerSpecific.<urlSlug>` and
`orderData.providerSpecific.<urlSlug>` objects.

Service partners can view only partnerSpecific data they own.

Service providers can view all data from all service partners whose cases they "own".

### Output

```
{
  "data": [
    { Case 1 },
    { Case 2 }
  ],
  "pagination": {
    "offset": 0,
    "count": 194
  }
}
```

## POST /api/v2/case/accept

Endpoint for the service provider to confirm that the case was accepted
for repair and imported to their production system.

### Access

Provider

### Input

| Name     | Type     | Description                             |
|----------|----------|-----------------------------------------|
| guid\*   | GUID     | Case's GUID                             |

### Example

```
{
  "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063"
}
```

## POST /api/v2/case/cancel

Endpoint for the service provider to notify ServiceOrderHub that the
case import was canceled and the provider revokes the acceptance to
handle the service case.

### Access

Provider

### Input

| Name     | Type     | Description                             |
|----------|----------|-----------------------------------------|
| guid\*   | GUID     | Case's GUID                             |
| reason   | String   | Why this case was rejected (Optional)   |

### Example

```
{
  "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063"
}
```

### Output

```
{
  "data": {
    "result": true
  }
}
```

## POST /api/v2/unexported-cases

Get a list of cases unaccepted by the Service Provider.

The request has the same body structure, parameters and output as [/api/v2/search-cases](Working with cases.md#post-apiv2search-cases)

### Access

Provider


## GET /api/v2/case/export

Get case with all additional info 

### Access

Provider

### Output

```
{
  "consumer": null,
  "country": "SE",
  "createdAt": "2017-08-01T16:01:29.918Z",
  "currentStatus": null,
  "customer": {
    "addName": null,
    "address": "test, 123",
    "city": "Testcity",
    "countryCode": "SE",
    "createdAt": "2017-08-01T16:01:30.045Z",
    "doorCode": null,
    "email": "test@example.com",
    "entrance": null,
    "firstName": "Test",
    "floor": null,
    "id": 30367,
    "lastName": "Test",
    "mobile": "1234567890",
    "name": null,
    "organizationName": null,
    "organizationNumber": null,
    "phone": null,
    "postalCode": "1234"
  },
  "externalData": {
    "alternatives": [
      {
        "departmentId": 1,
        "providerSpecificData": 123
      }
    ],
    "departmentId": 1,
    "providerSpecificData": 123
  },
  "guid": "6a38f75a-0426-4451-be7e-761c3fafa159",
  "id": 7783,
  "manufacturer": {
    "externalData": "Acer info",
    "id": 1006,
    "name": "Acer"
  },
  "manufacturerId": 1006,
  "orderData": {
    "activityNumber": "2567774",
    "files": [
      {
        "name": "1.jpg",
        "url": "https://example.com/1.jpg"
      },
      {
        "name": "2.jpg",
        "url": "https://example.com/2.jpg"
      }
    ],
    "infocareSesamOriginator": "Originator",
    "myVeryCustomData": 150,
    "originatorType": "private",
    "pickupDestination": "customer",
    "refNo": "testref",
    "returnDestination": "customer",
    "serviceProviderExportStatus": true,
    "shipmentData": {
      "guid": "8b5939d6-b8f2-40f0-a874-96b52f9fbe0a",
      "packageNumber": "00370716483417486489",
      "receiverAddress": {
        "address": "Teststreet, 42",
        "city": "Testcity",
        "countryCode": "SE",
        "email": "test@example.com",
        "mobilePhoneNumber": null,
        "name": "Workshop",
        "phoneNumber": "1234567890",
        "postalCode": "123 45"
      },
      "senderAddress": {
        "address": "test, 123",
        "city": "Testcity",
        "countryCode": "SE",
        "email": "test@example.com",
        "mobilePhoneNumber": "1234567890",
        "name": "Test Test",
        "phoneNumber": "1234567890",
        "postalCode": "1234"
      },
      "service": "senderella",
      "serviceId": "1000",
      "shipmentNumber": 0
    }
  },
  "partnerId": 1,
  "pickupDst": null,
  "productData": {
    "accessory": [
      "1099",
      "1107"
    ],
    "accessoryNames": [
      "Strömadapter",
      "Mus"
    ],
    "model": "abc-123",
    "otherAccessory": "something other",
    "problem": "1001.dd6f843a-e985-7fc5-d41d-17d37d6e0bb8",
    "problemDetails": {
      "261b0f00-5426-d92d-ec3d-c1d59c08c83e": "XP"
    },
    "purchaseDate": "2017-08-01"
  },
  "productType": {
    "externalData": null,
    "id": 1009,
    "name": "All-in-one desktop",
    "properties": {
      "allowPassword": true,
      "requireSerial": true
    }
  },
  "productTypeId": 1009,
  "receiver": {
    "addName": null,
    "address": "test, 123",
    "city": "Testcity",
    "countryCode": "SE",
    "createdAt": "2017-08-01T16:01:30.284Z",
    "doorCode": null,
    "email": "test@example.com",
    "entrance": null,
    "firstName": "Test",
    "floor": null,
    "id": 30369,
    "lastName": "Test",
    "mobile": "1234567890",
    "name": null,
    "organizationName": null,
    "organizationNumber": null,
    "phone": null,
    "postalCode": "1234"
  },
  "returnDst": null,
  "sender": {
    "addName": null,
    "address": "test, 123",
    "city": "Testcity",
    "countryCode": "SE",
    "createdAt": "2017-08-01T16:01:30.167Z",
    "doorCode": null,
    "email": "test@example.com",
    "entrance": null,
    "firstName": "Test",
    "floor": null,
    "id": 30368,
    "lastName": "Test",
    "mobile": "1234567890",
    "name": null,
    "organizationName": null,
    "organizationNumber": null,
    "phone": null,
    "postalCode": "1234"
  },
  "serviceLocation": {
    "externalData": null,
    "id": 2,
    "name": "På plats reparation"
  },
  "serviceLocationId": 2,
  "servicePartner": {
    "id": 1,
    "name": "Partner name"
  },
  "serviceProvider": {
    "id": 1,
    "name": "Workshop"
  },
  "serviceProviderDepartment": {
    "externalData": null,
    "id": 1,
    "name": "Workshop name"
  },
  "serviceProviderDepartmentId": 1,
  "serviceProviderId": 1,
  "serviceType": {
    "externalData": null,
    "id": 1,
    "name": "In warranty",
    "properties": {
      "requirePurchaseDate": true
    }
  },
  "serviceTypeId": 1,
  "shippingMethod": null,
  "shippingMethodId": null
}
```




## POST /api/v2/case/update

Modify Case.

### Access

Partner, Provider

### Input

| Name          | Type     | Description        |
|---------------|----------|--------------------|
| guid\*        | GUID     | Guid of the Case   |
| orderData     | Object   | Modified fields    |
| productData   | Object   | Modified fields    |
| customer      | Object   | Modified fields    |
| consumer      | Object   | Modified fields    |
| pickupDst     | Object   | Modified fields    |
| returnDst     | Object   | Modified fields    |

### Example

```
{
  "guid": "d2379b74-d9b0-40cd-92e7-d1f48f398f43",
  "orderData": {
    "infocareSesamOriginator": "Microsoft",
    "consents": ["hello", "world"]
  }
}
```

### Output

```
{
  "result": true
}
```

## POST /api/v2/case/finish

In some case partner doesn't want to send case to provider just after creation. 
In this case you can initiate this process by demand using this endpoint.

### Access

Partner

### Input

| Name     | Type     | Description                             |
|----------|----------|-----------------------------------------|
| guid\*   | GUID     | Case's GUID                             |

### Output

```
{
  "result": true
}
```


## POST /api/v2/case/status

Add status to case.

### Access

Provider

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| guid\*             | GUID          | Guid of the case           |
| key\*              | String        | Status key                 |

### Example

```
{
  "guid": "a7180ff9-7c6e-4d0e-b756-16b49531382f",
  "key": "final" 
}
```
  
### Output

```
{
  "data": {
    "id": 98, // Status Id
    "caseId": 7927, // Case Id
    "guid": "a7180ff9-7c6e-4d0e-b756-16b49531382f", // Case guid
    "key": "final",
    "createdAt": "2019-01-22T11:29:38.258Z",
    "updatedAt": "2019-01-22T11:29:38.258Z"
  }
}
```

## POST /api/v2/case/files

Add files to case. Accepts guid and array of file objects.

### Access

Partner, Provider

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| guid\*             | GUID          | Guid of the case           |
| files\*            | Array         | File objects to save       |

### File object

| Name               | Type          | Description                     |
|--------------------|---------------|---------------------------------|
| name\*             | String        | Name of the file with extension |
| url                | String        | URL                             |
| data               | String        | Base64 encoded content          |


Fields *url* and *data* are mutually exclusive.

### Example

```
{
    "guid": "e3df4d74-f559-4e65-b57d-37b5360fb46d",
    "files": {
        {
             "name": "cat.jpg",
             "url": "http://example.com/cat-image"
        },
        {
            "name": "doge.jpg",
            "data": "base64 encoded content"
        }
    }
}
```

### Output:

```
{
  "data": {
     "result": true
  }
}
```
# Service order API

**Note** Mandatory parameters are marked with  *  sign.

## GET /api/v1/brands

Get brands.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| originatorType\*       | Originator | Originator type                         |
| serviceType            | Int        | Service type Id                         |
| brand                  | Int        | Brand Id                                |
| productType            | Int        | Product type Id                         |

### Output:
```js
[
  {
    "id": 1006,
    "name": "Acer"
  },
  {
    "id": 1012,
    "name": "LG"
  }
]
```
## GET /api/v1/service-types

Get service types.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| originatorType\*       | Originator | Originator type                         |
| serviceType            | Int        | Service type Id                         |
| brand                  | Int        | Brand Id                                |
| productType            | Int        | Product type Id                         |

### Output:
```js
[
  {
    "id": 1,
    "name": "Garanti",
    "properties": {
      "requirePurchaseDate": true
    }
  },
  {
    "id": 2,
    "name": "Försäkringsreparation",
    "properties": {
      "requireInsuranceNumber": true
    }
  }
]
```

Field `properties` contains info about required fields

## GET /api/v1/product-types

Get product types.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| originatorType\*       | Originator | Originator type                         |
| serviceType\*          | Int        | Service type Id                         |
| brand\*                | Int        | Brand Id                                |

### Output:
```js
[
  {
    "id": 1009,
    "name": "All-in-one desktop",
    "properties": {
      "requireSerial": true
    }
  },
  {
    "id": 1004,
    "name": "Mobile phone",
    "properties": {
      "requireSerial": true,
      "requireImei": true
    }
  }
]
```

Field `properties` contains info about required fields.

## GET /api/v1/service-locations

Get service locations.

### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| originatorType\*       | Originator | Originator type                         |
| serviceType\*          | Int        | Service type Id                         |
| brand\*                | Int        | Brand Id                                |
| productType\*          | Int        | Product type Id                         |

### Output:
```js
[
  {
    "id": 2,
    "name": "På plats reparation"
  },
  {
    "id": 1,
    "name": "Verkstadsreparation"
  }
]
```

## GET /api/v1/accessory

Get accessory.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| productType\*          | Int        | Product type Id                         |

### Output:
```js
[
  {
    "id": 1106,
    "name": "Tangentbord"
  },
  {
    "id": 1107,
    "name": "Mus"
  },
  {
    "id": 1108,
    "name": "Strömkabel"
  }
]
```

## GET /api/v1/shipping-methods

Get allowed shipping methods.

### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| originatorType         | Originator | Originator type                         |
| serviceType            | Int        | Service type Id                         |
| brand                  | Int        | Brand Id                                |
| productType            | Int        | Product type Id                         |

### Output:
```js
[
  {
    "id": 3,
    "name": "PostNord Växjö Pallet",
    "properties": {
      "requirePickupDate": false,
      "goodsTypeList": null
    }
  },
  {
    "id": 2,
    "name": "PostNord Växjö Parcel",
    "properties": {
      "requirePickupDate": false,
      "goodsTypeList": ['X', 'Y', 'Z'],
    }
  }
]
```

Field `properties` contains info about required fields.

## GET /api/v1/shipping/pickup-dates
Get allowed pickup dates for this shipping methods.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| shippingMethod         | Int        | `id` from `shipping-methods` endpoint   |
| postalCode             | String     | Postal code (3-10) chars                |

### Output:
```js
[
  "2017-05-23T00:00:00.000Z",
  "2017-05-26T00:00:00.000Z",
  "2017-06-08T00:00:00.000Z",
  "2017-06-09T00:00:00.000Z",
  "2017-06-21T00:00:00.000Z",
  "2017-06-22T00:00:00.000Z"
]
```

## POST /api/v1/case/validate

Validate Case. All parameters are same as in `case/create` endpoint. In case of validation errors status will be "ok" and debug info will be passed through `data` field.

**Note** Currently we don't have a possibility to validate shipments and we can't guarantee that shipment will be created for the Case passed this validation.

### Output:
When data is correct:
```js
{
  "result": true
}
```
When data is incorrect:
```js
{
  "result": false,
  "message": "Fields validation failed",
  "validation": {
    "location": [
      "Value is not present in possible options"
    ],
    "consumer.postalCode": [
      "Wrong format",
      "Some other error"
    ],
    "acceptConditions": [
      "Field must be equal to true"
    ],
  }
}
```

## POST /api/v1/case/create

Create a new Case.
### Input:
| Name                   | Type        | Description                                     |
| ---------------------- | ----------- | ----------------------------------------------- |
| serviceType*           | Int         | Id from `service-types`                         |
| manufacturer*          | Int         | Id from `brands`                                |
| productType*           | Int         | Id from `product-types`                         |
| shipping**             | Int         | Id from `shipping-methods`                      |
| location*              | Int         | Id from `service-locations`                     |
| order*                 | OrderData   | Order data                                      |
| product*               | ProductData | Product data                           				 |
| customer*              | ContactData | Info about user who books this order            |
| consumer*              | ContactData | Info about end user                             |
| pickupDst              | ContactData | Where shipment will be picked up                |
| returnDst              | ContactData | Where shipment should be delivered after repair |
| originatorType*        | Originator  | Originator type                                 |
| acceptConditions*      | Boolean     | Terms and condition acceptance. Should be `true`|
| pickupDestination      | String      | `customer` or `custom`                          |
| returnDestination      | String      | `customer` or `custom`                          |

\*\* `shipping` can be assigned automatically by the Service Order Hub if shipping is required
according to the business rules, but no shipping method id was sent.

### Example:

```
POST /api/v1/case/create?accessToken=my_key HTTP/1.1
HOST: serviceorderhub.com
accept: application/json
content-length: 728
content-type: application/json

{
  "serviceType": 4,
  "manufacturer": 1012,
  "productType": 1001,
  "location": 3,
  "order": {
    "infocareSesamOriginator": "OtherSE"
  },
  "product": {
    "model": "0",
    "accessory": {
      "1093": true,
      "1094": true,
      "1095": false,
      "1096": false,
      "2501": false
    },
    "otherAccessory": "desc",
    "password": "1345",
    "problemText": "Picture/sound problems / Bad picture. Is periodic or constant?: Periodic"
  },
  "customer": {
    "type": 1,
    "organizationName": "test",
    "organizationNumber": "testorg",
    "mobile": "234234324",
    "email": "test@test.com",
    "address": "test",
    "postalCode": "1234",
    "city": "test"
  },
  "consumer": {
    "type": 0,
    "firstName": "test",
    "lastName": "test",
    "mobile": "234234324",
    "email": "test@test.com",
    "address": "test",
    "postalCode": "1145",
    "city": "test"
  },
  "originatorType": "private",
  "acceptConditions": true,
  "other": {
    "test": 1
  }
}
```
### Output:
```js
{
  "id": 7707, // Case's Id
  "guid": "69b0e17f-eeb9-4834-b809-60b015054c0d", // Case's GUID
  "integrations": { // All additional data stored here
    "shipping": { // Shipping data
      "packageNumber": "00370726200502318503",
      "shipmentNumber": 0,
      "senderAddress": {
        "name": "testorg",
        "address": "test",
        "postalCode": "1234",
        "city": "test",
        "country": "SE"
      },
      "receiverAddress": {
        "name": "Workshop name",
        "address": "some address",
        "postalCode": "5678",
        "city": "Some city",
        "country": "SE"
      },
      "result": true
    }
  }
}
```

## GET /api/v1/shipping/label
Get shipping label (Base64 encoded PDF file).
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| guid                   | GUID       | Case's GUID                             |

### Output:
*(Shown with wrapper object)*
```js
{
  "data": "dataJVBERi0xLjQNCiXi48/TDQo..."
}
```

## GET /api/v1/case

Get all info about created Case.
### Input:
| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| guid                   | GUID       | Case's GUID                             |

### Output:
```js
{
  "id": 7676,
  "country": "SE",
  "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063",
  "partnerId": 1,
  "workshopId": 2,
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
    "originatorType": "private",
    "pickupDestination": "customer",
    "returnDestination": "customer"
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
    }
  },
  "productType": {
    "id": 1004,
    "name": "Notebook",
    "properties": {
      "requireSerial": true
    }
  },
  "manufacturer": {
    "id": 1006,
    "name": "Acer"
  },
  "serviceLocation": {
    "id": 1,
    "name": "Verkstadsreparation"
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

## POST /api/v1/search-cases

Get list of cases 
### Input (JSON body):
| Name                   | Type        | Description                                     |
| ---------------------- | ----------- | ----------------------------------------------- |
| filters                | Object      | Search query                                    |
| pagination             | Object      | Pagination settings                             |
| pagination.offset      | Int         | Default = 0                                     |
| pagination.limit       | Int         | Default = 10, max = 100                         |

### Example:
```
{
  "filters": { "orderData": { "partnerSpecific": { "caperio": { "sysId": 3112312312 }}}}
}
```

You can also use special keys `$lt` and `$gt` for "<" and ">" conditions
```
{
  "filters": { "createdAt": { "$gt": "2017-01-01" }}
}
```

*Allowed keys for querying:*

id, guid, partnerId, serviceProviderId, manufacturerId, currentStatus, productData.\*, orderData.\*,
serviceTypeId, serviceLocationId, productTypeId, createdAt

All Partner and Service provider specific data is stored in `orderData.partnerSpecific.<urlSlug>`
and `orderData.providerSpecific.<urlSlug>` objects.

Service partners can view only partnerSpecific data with their urlSlug.

Service providers can view all data.

### Output
```
{
  "data": [
    { Case 1 },
    { Case 2 },
    ...
  ],
  "pagination": {
    "offset": 0,
    "count": 194
  }
}   
```

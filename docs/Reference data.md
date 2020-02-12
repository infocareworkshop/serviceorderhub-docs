# Reference data

## GET /api/v3/manufacturers

Get manufacturers (brands).

### Access

Partner

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |

### Output

```
[
  {
    "id": 1006,
    "name": "Acer",
    "externalData": null
  },
  {
    "id": 1012,
    "name": "LG",
    "externalData": null
  }
]
```

## GET /api/v3/service-types

Get service types.

### Access

Partner

### Input:

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |

### Output

```
[
  {
    "id": 1,
    "name": "Garanti",
    "properties": {
      "requirePurchaseDate": true
    },
    "externalData": null
  },
  {
    "id": 2,
    "name": "Försäkringsreparation",
    "properties": {
      "requireInsuranceNumber": true
    },
    "externalData": null
  }
]
```

Field `properties` contains info about required fields

## GET /api/v3/product-types

Get product types.

### Access

Partner

### Input


| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |


### Output

```
[
  {
    "id": 1009,
    "name": "All-in-one desktop",
    "properties": {
      "requireSerial": true,
      "withLocations": true
    },
    "externalData": null
  },
  {
    "id": 1004,
    "name": "Mobile phone",
    "properties": {
      "requireSerial": true,
      "requireImei": true
    },
    "externalData": null
  }
]
```

Field `properties` contains info about required fields.

## GET /api/v3/service-locations

Get service locations.

### Access

Partner

### Input

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   | Booking type               |
| serviceType\*      | Int           | Service type Id            |
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
| productType\*      | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |


### Output

```
[
  {
    "id": 2,
    "name": "På plats reparation",
    "externalData": null
  },
  {
    "id": 1,
    "name": "Verkstadsreparation",
    "externalData": null
  }
]
```

## GET /api/v3/accessory

Get accessory.

### Access

Partner

### Input

| Name            | Type   | Description       |
|-----------------|--------|-------------------|
| productType\*   | Int    | Product type Id   |


### Output

```
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

## GET /api/v3/shipping-methods

Get allowed shipping methods.

### Access

Partner

### Input

| Name                | Type          | Description                |
|---------------------|---------------|----------------------------|
| originatorType\*    | Originator    | Originator type            |
| bookingType\*       | BookingType   | Booking type               |
| serviceType\*       | Int           | Service type Id            |
| manufacturer\*      | Int\|String   | Manufacturer Id or alias   |
| productType\*       | Int\|String   | Product type Id or alias   |
| clientPostalCode    | String        |                            |
| serviceLocation\*   | Int           | Service location Id        |


### Output

```
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

If `requirePickupDate` of the `properties` object is not `false`, the parameter `order.pickupDate` becomes mandatory for case/create and case/validate endpoints!

If `goodsTypeList` of the `properties` object is not `null`, the parameter `order.goodsType` becomes mandatory for case/create and case/validate endpoints!

## GET /api/v3/shipping/pickup-dates

Get allowed pickup dates for this shipping methods.

### Access

Partner

### Input


| Name               | Type     | Description                                 |
|--------------------|----------|---------------------------------------------|
| shippingMethod\*   | Int      | `id` from `shipping-methods` endpoint       |
| postalCode\*       | String   | Postal code (3-10) chars                    |


### Output:

```
[
  "2017-05-23T00:00:00.000Z",
  "2017-05-26T00:00:00.000Z",
  "2017-06-08T00:00:00.000Z",
  "2017-06-09T00:00:00.000Z",
  "2017-06-21T00:00:00.000Z",
  "2017-06-22T00:00:00.000Z"
]
```

## GET /api/v3/shipping/pickup-dates-extended

Get allowed pickup dates with eventual comments for each date. Usually it includes info about pick up time

### Access

Partner

### Input

| Name               | Type     | Description                                 |
|--------------------|----------|---------------------------------------------|
| shippingMethod\*   | Int      | `id` from `shipping-methods` endpoint       |
| postalCode\*       | String   | Postal code (3-10) chars                    |


### Output

```
[
  {
    "date": "2019-02-01",
    "comment": "Mondays/Tuesdays between 12-16, Thursdays/Wednesdays/Fridays between 8-12"
  },
  {
    "date": "2019-02-04",
    "comment": "Mondays/Tuesdays between 12-16, Thursdays/Wednesdays/Fridays between 8-12"
  }
]
```

## GET /api/v3/partner

Get partner by id

### Access

Any

### Input

| Name               | Type     | Description                                 |
|--------------------|----------|---------------------------------------------|
| id\*               | Int      | Id                                          |

### Output

```
{
  "id": 1,
  "name": "Workshop",
  "contactData": {
    "id": 30347,
    "name": null,
    "firstName": null,
    "lastName": null,
    "organizationName": "Workshop",
    "organizationNumber": null,
    "addName": null,
    "address": "Teststreet, 123",
    "postalCode": "123 45",
    "city": "Testcity",
    "countryCode": null,
    "phone": "1234567890",
    "mobile": null,
    "email": "test@example.com",
    "floor": null,
    "entrance": null,
    "doorCode": null,
    "createdAt": "2017-08-01T14:01:24.083Z"
  }
}
```

## GET /api/v3/provider

Get provider by id

### Access

Any

### Input

| Name               | Type     | Description                                 |
|--------------------|----------|---------------------------------------------|
| id\*               | Int      | Id                                          |

### Output

```
{
  "id": 1003,
  "name": "Provider",
  "contactData": {
    "id": 30348,
    "name": null,
    "firstName": "John",
    "lastName": "Smith",
    "organizationName": Provider",
    "organizationNumber": 123456-1234",
    "addName": null,
    "address": "Teststreet, 42",
    "postalCode": "12345",
    "city": "Testcity",
    "countryCode": "SE",
    "phone": "0123456789",
    "mobile": "0123456789",
    "email": "test@example.com",
    "floor": null,
    "entrance": null,
    "doorCode": null,
    "createdAt": "2017-08-01T14:02:57.172Z"
  }
}
```

## GET /api/v3/routing

Find service provider, shipping methods and business rules by case parameters

### Access

Partner

### Input

| Name                | Type          | Description                |
|---------------------|---------------|----------------------------|
| originatorType      | Originator    | Originator type            |
| bookingType         | Originator    | Originator type            |
| serviceType         | Int           | Service type Id            |
| manufacturer        | Int\|String   | Manufacturer Id or alias   |
| productType         | Int\|String   | Product type Id or alias   |
| clientPostalCode    | String        |                            |
| serviceLocation     | Int           | Service location Id        |
| model               | String        | Model name                 |

### Output

```

{
  "servicePartner": {
    "id": 1,
    "name": "Partner"
  },
  "servicePartnerCompetence": {
    // Any specific data extracted from partner's business rules
  },
  "serviceProvider": {
    "contactData": {
      "addName": null,
      "address": "Teststreet 1",
      "city": "Testcity",
      "countryCode": "SE",
      "createdAt": "2017-08-01T14:02:57.172Z",
      "doorCode": null,
      "email": "test@test.com",
      "entrance": null,
      "firstName": "John",
      "floor": null,
      "id": 30348,
      "lastName": "Smith",
      "mobile": "1234567890",
      "name": null,
      "organizationName": "Workshop",
      "organizationNumber": "123456",
      "phone": "1234567890",
      "postalCode": "1234"
    },
    "id": 1,
    "name": "Workshop"
  },
  "serviceProviderCompetence": {
    // Any specific data extracted from service provider's business rules
  },
  "shippingMethods": [
    // All available shipment methods
  ]
}

```

## GET /api/v3/models

Find models

### Access

any

### Input

| Name                | Type          | Description                       |
|---------------------|---------------|-----------------------------------|
| manufacturer        | Int\|String   | Manufacturer Id or alias          |
| productType         | Int\|String   | Product type Id or alias          |
| model\*             | String        | Model name                        |
| limit               | Int           | Number of results (10 by default) |


### Example

`/api/v3/models?manufacturer=acer&model=aspire&limit=1`

### Output

```
[
  {
    "createdAt": "2019-05-21T09:45:04.000Z",
    "id": 845,
    "manufacturer": {
      "externalData": null,
      "id": 1006,
      "name": "Acer"
    },
    "manufacturerId": 1006,
    "name": "aspire",
    "productType": {
      "externalData": null,
      "id": 1004,
      "name": "Notebook",
      "properties": {
        "allowPassword": true,
        "requireSerial": true
      }
    },
    "productTypeId": 1004,
    "tags": [],
    "updatedAt": "2019-05-21T09:45:04.000Z"
  }
]
```

## POST /api/v3/models

Update models

### Access

any

### Input

Array of model objects:

| Name                | Type          | Description                       |
|---------------------|---------------|-----------------------------------|
| manufacturer\*      | Int\|String   | Manufacturer Id or alias          |
| productType\*       | Int\|String   | Product type Id or alias          |
| name\*              | String        | Model name                        |
| id                  | Int           | Id in the model registry          |
| tags                | Array<Int>    | Assigned tags                     |

If you omit "id" key then the model from the request will be matched with all existing models 
and created new record if necessary.

To change name you should always pass "id" key.

### Output

List of modified models
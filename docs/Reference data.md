Title: Reference data

## GET /api/v1/manufacturers

Get manufacturers (brands).

### Input:

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |

### Output:

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

## GET /api/v1/service-types

Get service types.

### Input:

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType        | Int           | Service type Id            |
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |

### Output:

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

## GET /api/v1/product-types

Get product types.

### Input:


| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType\*      | Int           | Service type Id            |
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
| productType        | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |


### Output:

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

## GET /api/v1/service-locations

Get service locations.

### Input:

| Name               | Type          | Description                |
|--------------------|---------------|----------------------------|
| originatorType\*   | Originator    | Originator type            |
| bookingType\*      | BookingType   |                            |
| serviceType\*      | Int           | Service type Id            |
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
| productType\*      | Int\|String   | Product type Id or alias   |
| clientPostalCode   | String        |                            |


### Output:

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

## GET /api/v1/accessory

Get accessory.

### Input:

| Name            | Type   | Description       |
|-----------------|--------|-------------------|
| productType\*   | Int    | Product type Id   |


### Output:

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

## GET /api/v1/shipping-methods

Get allowed shipping methods.

### Input:

| Name                | Type          | Description                |
|---------------------|---------------|----------------------------|
| originatorType\*    | Originator    | Originator type            |
| serviceType\*       | Int           | Service type Id            |
| manufacturer\*      | Int\|String   | Manufacturer Id or alias   |
| productType\*       | Int\|String   | Product type Id or alias   |
| clientPostalCode    | String        |                            |
| serviceLocation\*   | Int           | Service location Id        |


### Output:

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

If `goodsTypeList` `of the `properties` object is not `null`, the parameter `order.goodsType` becomes mandatory for case/create and case/validate endpoints!

## GET /api/v1/shipping/pickup-dates

Get allowed pickup dates for this shipping methods.

### Input:


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

## GET /api/v1/shipping/pickup-dates-extended

Get allowed pickup dates with eventual comments for each date. Usually it includes info about pick up time

### Input:


| Name               | Type     | Description                                 |
|--------------------|----------|---------------------------------------------|
| shippingMethod\*   | Int      | `id` from `shipping-methods` endpoint       |
| postalCode\*       | String   | Postal code (3-10) chars                    |


### Output:

```
{
  "data": [
    {
      "date": "2019-02-01",
      "comment": "Mondays/Tuesdays between 12-16, Thursdays/Wednesdays/Fridays between 8-12"
    },
    {
      "date": "2019-02-04",
      "comment": "Mondays/Tuesdays between 12-16, Thursdays/Wednesdays/Fridays between 8-12"
    }
}
```
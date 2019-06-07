# Booking

## POST /api/v2/case/validate

Validate Case. All parameters are same as in `case/create` endpoint.
In case of validation errors status will be "ok" and debug info will be
passed through `data` field.

**Note** Currently we don't have a possibility to validate shipments and
we can't guarantee that shipment will be created for the Case passed
this validation.

### Access

Partner

### Input

See `/api/v2/case/create`

### Output

When data is correct:

```
{
  "result": true
}
```

When data is incorrect:

```
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

## POST /api/v2/case/create

Create a new Case.

### Access

Partner

### Input

| Name                           | Type          | Description                                              |
|--------------------------------|---------------|----------------------------------------------------------|
| serviceType\*                  | Int           | Id from `service-types`                                  |
| manufacturer\*                 | Int\|String   | Id or alias from `manufacturers`                         |
| productType\*                  | Int\|String   | Id or alias from `product-types`                         |
| shipping<sup>1</sup>           | Int           | Id from `shipping-methods`                               |
| location\*                     | Int           | Id from `service-locations`                              |
| order\*                        | OrderData     | Order data                                               |
| product\*                      | ProductData   | Product data                                             |
| pickupDestination<sup>2</sup>  | String        | `consumer` or `customCompany` or `customPrivate`         |
| returnDestination<sup>2</sup>  | String        | `consumer` or `customCompany` or `customPrivate`         |
| clientPostalCode\*             | String        |                                                          |
| customer\*                     | ContactData   | Info about user who books this order                     |
| consumer<sup>3</sup>           | ContactData   | Info about end user                                      |
| pickupDst<sup>4</sup>          | ContactData   | Where shipment will be picked up or sent from            |
| returnDst<sup>5</sup>          | ContactData   | Where shipment should be delivered after repair          |
| originatorType\*               | Originator    | Originator type                                          |
| bookingType\*                  | BookingType   | Booking type                                             |
| acceptConditions\*             | Boolean       | Terms and condition acceptance. Should be `true`         |
| noPassword                     | Boolean       | Make `product.password` optional                         |

<sup>1</sup> `shipping` can be assigned automatically by the Service Order Hub if
shipping is required according to the business rules, but no shipping
method id was sent. If more than one method is available, the first one by name will be selected.

<sup>2</sup> Enabled when originatorType - `helpdesk`

<sup>3</sup>  Enabled when bookingType ≠ `privateToPrivate`

<sup>4</sup> Enabled when pickupDestination ≠ `consumer`. If pickupDestination is not sent, we use `pickupDestination: "consumer"` by default

<sup>5</sup> Enabled when returnDestination ≠ `consumer`. If returnDestination is not sent, we use `returnDestination: "consumer"` by default

### Example

```
POST /api/v2/case/create?accessToken-my_key HTTP/1.1
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
    "infocareSesamOriginator": "OtherSE",
    "partnerSpecific": {
      "testPartner": {
        "testParam1": "text"
      }
    }
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
    "organizationName": "Test LTD",
    "organizationNumber": "64236482764",
    "mobile": "234234324",
    "email": "test@test.com",
    "address": "test",
    "postalCode": "1234",
    "city": "test"
  },
  "consumer": {
    "type": 0,
    "firstName": "John",
    "lastName": "Doe",
    "mobile": "234234324",
    "email": "test@test.com",
    "address": "test",
    "postalCode": "1145",
    "city": "test"
  },
  "originatorType": "private",
  "bookingType": "companyToPrivate",
  "clientPostalCode": "1145",
  "acceptConditions": true,
  "other": {
    "test": 1
  }
}
```

### Output

```
{
  "id": 7707, // Case's Id
  "guid": "69b0e17f-eeb9-4834-b809-60b015054c0d" // Case's GUID
}
```
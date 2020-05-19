# Booking

## Conditional validation
Data quality is the cornerstone of the serviceorderhub. Thus it's important for service partners to send all data required on each step of service order fulfillment. The set of required data can be different under different circumstances. Consequently, some properties can be either mandatory or non-mandatory in different cases depending on values of the other properties and the settings of your integration.

**List of properties involved in conditional validation**

1. `productData.password` is mandatory only if you book service for `productType` where password protection is common (mobile phone, laptop, PC, etc). Contact support for the complete list of such categories, but usually they can be guessed based on the common sense (no password protection for dish washers, right?). Even if you book such `productType` which is covered by there is a workaround to bypass the valdation. If you send the property `noPassword: true` it will force the validation to accept the order even if `productData.password` is empty or missing.
2. `productData.imei` is mandatory when you book service for mobile phones.
3. `productData.serial` is mandatory for all non-lowcost home electronics products except mobile phones (it's enough to send only `productData.imei` for them, but it's higly recommended to send `productData.serial` too). A complete list of `productType` ids requiring `productData.serial` can be provided upon request, but can be also guessed based on the common sense.
5. `*.firstName` and `*.lastName` are mandatory for [contact data](Data%20types%20and%20structures/#contactdata) objects with `"type": 0` (private persons).
6. Similarly, `*.organizationName` and `*.organizationNumber` are mandatory when the [contact data](Data%20types%20and%20structures/#contactdata) object has `"type: 1` (company).
7. `productData.purchaseDate` and non-empty `attachedFiles` are mandatory if you book a warranty repair (it depends on your `serviceType` settings in the integration).
8. `productData.insuranceNumber` and `productData.insuranceCompany` are mandatory if you book an insurance repair
9. `acceptConditions` is non-mandatory if you send a non-empty `consents` object.
10. `orderData.pickupDate` (a date of desired pickup) is mandatory only when you book shipping by certain carriers along with a service order. Please ask support if it's relevant for your integration.
11. `productData.weight` and / or `productData.volume` are mandatory only when you book shipping by certain carriers along with a service order. Please ask support if it's relevant for your integration.
12. `orderData.goodsType` (type of goods in terminology of the carrier) is mandatory only when you book shipping by certain carriers along with a service order. Please ask support if it's relevant for your integration.
13. `orderData.mail` is mandatory if your integration support package materials ordering and you also send `orderData.emballage: true`

## POST /api/v3/case/validate

Validate Case. Accepts the same input as the `case/create` endpoint.
In case of validation errors the `400` http status will be returned and debug info will be passed through the `error.details` object.

**Note** Currently we don't have a possibility to validate shipments and we can't guarantee that shipping will be successfully booked for a case passing the validation.

### Access

Partner

### Input

See `/api/v3/case/create`

### Output

When data is correct:

```
{
  "data": {
    "result": true
  }
}
```

When data is incorrect:

```
{
  "error": {
    "message": "Fields validation failed",
    "details": {
      "productData.password": [
        "Field is required"
      ],
      "location": [
        "Value is not present in possible options"
      ],
      "consumer.postalCode": [
        "Wrong format",
        "Some other error"
      ],
      "acceptConditions": [
        "Field must be equal to true"
      ]
    }
  }
}
```

**New in V3**

We changed `product` to `productData` and `order` to `orderData` to make naming more consistent.

## POST /api/v3/case/create

Create a new Case.

### Access

Partner

### Input

| Name                           | Type          | Description                                              
|--------------------------------|---------------|------------------------------------------------------------------------------------
| serviceType\*                  | Int           | Id from `service-types`                                  
| manufacturer\*                 | Int\|String   | Id or alias from `manufacturers`                         
| productType\*                  | Int\|String   | Id or alias from `product-types`                         
| shipping<sup>1</sup>           | Int           | Id from `shipping-methods`                               
| location\*                     | Int           | Id from `service-locations`                              
| orderData\*                    | OrderData     | Order data                                               
| productData\*                  | ProductData   | Product data                                             
| pickupDestination<sup>2</sup>  | String        | `consumer` or `customCompany` or `customPrivate`         
| returnDestination<sup>2</sup>  | String        | `consumer` or `customCompany` or `customPrivate`         
| clientPostalCode\*             | String        |                                                          
| customer\*                     | ContactData   | Info about user who books this order                     
| consumer<sup>3</sup>           | ContactData   | Info about end user                                      
| pickupDst<sup>4</sup>          | ContactData   | Where shipment will be picked up or sent from            
| returnDst<sup>5</sup>          | ContactData   | Where shipment should be delivered after repair          
| originatorType\*               | Originator    | Originator type                                          
| bookingType\*                  | BookingType   | Booking type                                             
| acceptConditions\*             | Boolean       | Terms and condition acceptance. Must be `true` if no `orderData.consents` sent
| noPassword                     | Boolean       | Make `productData.password` optional
| trackingId<sup>6</sup>         | String        | Store this requests and mark it with special value

<sup>1</sup> `shipping` can be assigned automatically by the Service Order Hub if shipping is required according to the business rules (your integration setup), but no shipping method id was sent. If more than one method is available, the first one by name (alphabetically) will be selected.

<sup>2</sup> Enabled when originatorType - `helpdesk`

<sup>3</sup> Enabled when bookingType ≠ `privateToPrivate`

<sup>4</sup> Enabled when pickupDestination ≠ `consumer`. If pickupDestination is not sent, we use `pickupDestination: "consumer"` by default

<sup>5</sup> Enabled when returnDestination ≠ `consumer`. If returnDestination is not sent, we use `returnDestination: "consumer"` by default

<sup>6</sup> If presented we will store this request as external request with type "caseCreated" and assign trackingId as meeta field. This is very helpful in debugging and troubleshootiing, because this external request will contain all info about validation errors and all fields, even removed by validation. You can search requests by trackingId using `/requests/find` endpoint.

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
  "orderData": {
    "consents": {
      "ConsentTerms": true,
      "ConsentRepair": true,
      "ConsentOffers": false
    },
    "infocareSesamOriginator": "OtherSE",
    "partnerSpecific": {
      "acme": {
        "acmeUniqueStringProperty": "text",
        "acmeUniqueFloatProperty": 5.1,
        "acmeUniqueBoolProperty": true
      }
    }
  },
  "productData": {
    "model": "iTest 10+",
    "accessory": {
      "1093": true,
      "1094": true,
      "1095": false,
      "1096": false,
      "2501": false
    },
    "otherAccessory": "Accessory(ies) without ID in free text",
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
  "clientPostalCode": "1145"
}
```

### Output

```
{
  "guid": "69b0e17f-eeb9-4834-b809-60b015054c0d" // Case's GUID
}
```

**New in V3**

We changed `product` to `productData` and `order` to `orderData` to make naming more consistent.

We removed `id` field from output to avoid confusion.

## Attach files

You can use [`case/files`](Working%20with%20cases/#post-apiv3casefiles) to upload files after the case is created.

# Data types and structures

There are some special data types used in our API.

**Note** Mandatory parameters are marked with \* sign.

## Date 

Date string in the format `yyyy-MM-ddTHH:mm:ss.SSSZ` (One of the **ISO 8601** standard's variations, that must contain date, time and timezone). Every date will be converted to UTC.

`2012-04-23T18:25:43.511Z`

## Originator 

Describes what kind of user initiates the service order. Must be either
`private`, `registered` or `helpdesk`;

`private` - the case originator is a person or an organization which
doesn’t have any contracts or other ties with the partner; 

`registered` - an organizational unit within the partner; 

`helpdesk` - partner's
centralized helpdesk which gathers support request from all
organizational units and places orders on behalf of them.

The business rules may be different depending on the originator.

## Booking types

Possible values:

`companyToCompany` - when one company makes a transaction with another;

`companyToPrivate` - when a company makes a transaction with a private;
person

`privateToPrivate` - when a private person makes a transaction for
themself or with another private person.

## Dimensions

Contains volume of the package or size of the box.

| Name        | Type        | Description
| ----------- | ----------- | ----------------
| volume      | Number      | m³

OR

| Name        | Type        | Description
| ----------- | ----------- | ----------------
| width       | Number      | cm
| height      | Number      | cm
| depth       | Number      | cm

## OrderData

Contains info about service order.

| Name                                 | Type      | Description
| ------------------------------------ | --------- | ----------------------------------------
| chain                                | String    | Special code of the chain
| refNo                                | String    | External ID, use any string (64 chars)
| goodsType<sup>1</sup>                | String    | Data from `shipping-methods` goodsTypeList
| pickupDate<sup>2</sup>               | String    | Date from `pickup-dates`
| emballage                            | Boolean   | Does customer wants to request packing materials
| mail*<sup>3</sup>                    | String    | Address of where the packaging should be delivered
| partnerSpecific<sup>4</sup>          | Object    | Custom parameters specific for a certain partner
| providerSpecific<sup>5</sup>         | Object    | Same as partnerSpecific but for service provider
| consents\*                           | Object    | Consents. Read more [here](#consents) 
| partnerOrderReference                | String    | Custom reference assigned by partner
| providerOrderReference               | String    | Custom reference assigned by provider
| maxRepairPrice                       | String    | Limit amount for beyond economic repair
| deductible                           | String    | Insurance deductible
| proofOfPurchaseReference             | String    | Proof of purchase reference
| skipCostProposalAndRepairForAnyPrice | Boolean   | Accept any cost for repair without a cost proposal
| contactPerson                        | String    | Contact person for eventual communications regarding the order
| partnerSpecific.*                    | any       | Custom data defined by partner
| providerSpecific.*                   | any       | Custom data defined by provider
| shipmentData                         | ShipmentData | Inbound shipment data
| \*ShipmentData<sup>6</sup>           | ShipmentData | Additional shipment data

<sup>1</sup> `goodsType` is mandatory only if the corresponding shipping method
has `properties.requirePickupDate: true`. Check the API ref for
`/api/v1/shipping-methods` for more details. [Read more](Booking/#conditional-validation) about conditional validation.

<sup>2</sup> `pickupDate` is mandatory only if the corresponding shipping method
has **not** `null` for `properties.goodsTypeList`. Check the API ref for
`/api/v1/shipping-methods` for more details and [read more](Booking/#conditional-validation) about conditional validation.

<sup>3</sup> `mail` is mandatory if `emballage: true`

<sup>4</sup> `partnerSpecific` is an optional object for custom parameters
specific for a certain ingegration with partner. Parameter's names
should be added to the serviceorderhub config before you can use it.
Please contact support for that.

**Example of partnerSpecific**

```
"partnerSpecific": {
    "superStoreScandinavia": {
        "testParam1": 350,
        "anotherTestParam2": "FREETEXT123",
    }
}
```

<sup>5</sup> `providerSpecific` is an optional object for custom parameters
specific for a certain ingegration with service provider. Parameter's
names should be added to the serviceorderhub config before you can use
it. Please contact support for that.

**Example of providerSpecific**

```
"providerSpecific": {
    "nordicService": {
        "anotherTestParam": 1,
        "veryDifferentTestParam": "FREE TEXT 09876554321",
    }
}
```

<sup>6</sup> Cases can have addiional shipment data, for example *exportShipmentData*, *forwardingShipmentData* etc.

## ProductData

Contains info about the product.

| Name                   | Type          | Description
| ---------------------- | ------------- | -----------------------------------
| model\*                | String        | Product's model name (40 chars)
| purchaseDate           | Date          | When the product was purchased
| imei\*\*               | String        | Product's IMEI (20 chars)
| serial\*\*             | String        | Product's Serial number (40 chars)
| insuranceCompany\*\*   | String        | Insurance company name (50 chars)
| insuranceNumber\*\*    | String        | Insurance number
| accessory              | Array         | List of accessory IDs
| otherAccessory         | String        | List of additional accessories in free text
| problemText\*          | String        | Problem description
| solution               | String        | Description of what has been done to solve the problem
| password\*\*           | String        | Product's password (if applicable)
| weight                 | Number        | Product's weight (in kg)
| volume                 | Dimensions    | Width, height, depth
| damage                 | String        | Damage description

\*\* These fields may become mandatory in some conditions. [Read more](Booking/#conditional-validation) about conditional validation.

## ContactData

Contains info about person or company.

| Name                             | Type       | Description
| -------------------------------- | ---------- | -----------------------------------
| type\*                           | Boolean    | 1 - company, 0 - private person
| firstName\*<sup>1</sup>          | String     | Person’s first name (64 chars)
| lastName\*<sup>1</sup>           | String     | Person’s last name (64 chars)
| organizationName\*<sup>2</sup>   | String     | Org Name (64 chars)
| organizationNumber\*<sup>2</sup> | String     | Organization Number (64 chars)
| mobile\*                         | String     | Cellphone (19 chars)
| phone                            | String     | Landline phone (64 chars)
| email\*                          | String     | Email (64 chars)
| address\*                        | String     | Address (64 chars)
| addName                          | String     | Additional name (c/o) (64 chars)
| postalCode\*                     | String     | Postal code (6 chars)
| city\*                           | String     | City (64 chars)
| countryCode                      | String     | Country code (2 capital letters)

<sup>1</sup> Allowed only when user is a private person

<sup>2</sup> Allowed only when user is a company

[Read more](Booking/#conditional-validation) about conditional validation.

## Consents

Contains data regarding user's consents in compliance with [GDPR](https://gdpr.eu). Certain consents can be mandatory, so the validation will fail if they're `false` or missing. Please ask support for a list of mandatory consents for your integration.

| Name                             | Type          | Description                     |
|----------------------------------|---------------|---------------------------------|
| Consent name from the settings\* | Boolean       | Consent name from the settings  |

The set of possible consents may vary, but the most common are these three:

| Name                             | Description
| -------------------------------- | ---------------------------------------------------------------------------
| ConsentTerms\*                   | User accepts terms and conditions of the service
| ConsentRepair\*                  | User allows personal data processing required for the service
| ConsentOffers                    | User allows to get contacted with additional offers related to the service

<sup>*</sup> - mandatory consents

**Example**

```
{
    "ConsentTerms": true,
    "ConsentRepair": true,
    "ConsentOffers": false
}
```

## FileType
Allowed values:
- proofOfPurchase
- proofOfDamage
- transportDamage
- claim
- report
- shipmentLabel
- other (default value)

## ShipmentData

| Name                             | Type          | Description                     |
|----------------------------------|---------------|---------------------------------|
| guid | Guid       | guid |
| shipmentNumber | String | Shipment number |
| packageNumber | String | Package number |
| shippingMethod | Int | Id of the shipping method |

*This object may change unpredictably depending on which shipping service was used. All fields are optional, extra fields are allowed.*

**Example**

```
{
  "guid": "1231d25f-4313-48d4-a48b-************",
  "service": "senderella",
  "serviceId": "1045",
  "packageNumber": "JJFI6027730000017****",
  "senderAddress": {
    "city": "Joensuu",
    "name": "**",
    "email": "***",
    "address": "***",
    "attention": null,
    "postalCode": "00000",
    "countryCode": "FI",
    "phoneNumber": "010 000 0000",
    "mobilePhoneNumber": "010 000 0000"
  },
  "shipmentNumber": "JJFI6027730000017****",
  "receiverAddress": {
    "city": "Vantaa",
    "name": "***",
    "email": "***",
    "address": "***",
    "attention": "",
    "postalCode": "123456",
    "countryCode": "FI",
    "phoneNumber": "010 000 0000",
    "mobilePhoneNumber": "010 000 0000"
  }
}
```

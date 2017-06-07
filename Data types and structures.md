# Data types and structures

There are some special data types used in our API.

**Note** Optional parameters are marked with **?** sign.

### Date

**ISO 8601** compatible string that contains date, time and timezone. Every date will be converted to UTC.

`2012-04-23T18:25:43.511Z`

### Originator

Describes what kind of user performs service order. Must be either `private`, `registered` or `helpdesk`.

### Dimensions

Contains volume of the package or size of the box.

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| volume                 | Number     | m³ ?                                    |

OR

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| width                  | Number     | cm                                      |
| height                 | Number     | cm                                      |
| depth                  | Number     | cm                                      |

### OrderData

Contains info about service order.

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| chain?                 | String     | Special code of the chain               |
| refNo?                 | String     | External ID, use any string (64 chars)  |
| goodsType?             | String     | Data from `shipping-methods` goodsTypeList |
| pickupDate?            | String     | Date from `pickup-dates`                |
| emballage?             | Boolean    | Does customer wants to request emballage|
| mail?                  | String     | ?                         |


### DeviceData

Contains info about the device.

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| model                  | String     | Device's model name (40 chars)          |
| purchaseDate?          | Date       | When the device was purchased           |
| imei?*                 | String     | Device's IMEI (20 chars)                |
| serial?*               | String     | Device's Serial number (40 chars)       |
| insuranceCompany?*     | String     | Insurance company name (50 chars)       |
| insuranceNumber?*      | String     | Insurance number                        |
| accessory?             | Array      | List of accessory Ids                   |
| otherAccessory?        | String     | Names of additional accessory           |
| problemText            | String     | Problem description                     |
| password?              | String     | Device's password if exists             |
| weight?                | Number     | Device's weight (in kg)?                |
| volume?                | Dimensions | Width, height, depth                    |

\* These fields may become required in some conditions. 

### ContactData

Contains info about person or company.

| Name                   | Type       | Description                             |
| ---------------------- | ---------- | --------------------------------------- |
| type                   | Int        | `0` - private, `1` - organization ?     |
| firstName*             | String     | Person's first name (64 chars)          |
| lastName*              | String     | Person's last name (64 chars)           |
| organisationName**     | String     | Org Name (64 chars)                     |
| organisationNumber?**  | String     | VAT Number ? (64 chars)                 |
| mobile                 | String     | Cell phone (19 chars)                   |
| phone?                 | String     | Phone (64 chars)                        |
| email                  | String     | Email (64 chars)                        |
| address                | String     | Address (64 chars)                      |
| addName?               | String     | Additional name (64 chars)              |
| postalCode             | String     | Postal code (6 chars)                   |
| city                   | String     | City (64 chars)                         |

\* Allowed only when `type = 0`

\*\* Allowed only when `type = 1`













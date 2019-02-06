Data types and structures
=========================

There are some special data types used in our API.

**Note** Mandatory parameters are marked with \* sign.

Date
~~~~

**ISO 8601** compatible string that contains date, time and timezone.
Every date will be converted to UTC.

``2012-04-23T18:25:43.511Z``

Originator
~~~~~~~~~~

Describes what kind of user initiates the service order. Must be either
``private``, ``registered`` or ``helpdesk``.

``private`` - the case originator is a person or an organization which
doesn’t have any contracts or other ties with the partner;
``registered`` - an organizational unit within the partner; ``helpdesk``
- partner's centralized helpdesk which gathers support request from all
organizational units and places orders on behalf of them.

The business rules may be different depending on the originator.

Booking types
~~~~~~~~~~~~~

Possible values:

``companyToCompany`` - when one company makes a transaction with another

``companyToPrivate`` - when a company makes a transaction with a private
person

``privateToPrivate`` - when a private person makes a transaction for
themself or with another private person

Dimensions
~~~~~~~~~~

Contains volume of the package or size of the box.

+----------+----------+---------------+
| Name     | Type     | Description   |
+==========+==========+===============+
| volume   | Number   | m³            |
+----------+----------+---------------+

OR

+----------+----------+---------------+
| Name     | Type     | Description   |
+==========+==========+===============+
| width    | Number   | cm            |
+----------+----------+---------------+
| height   | Number   | cm            |
+----------+----------+---------------+
| depth    | Number   | cm            |
+----------+----------+---------------+

OrderData
~~~~~~~~~

Contains info about service order.

+---------------------------+-----------+----------------------------------------------------+
| Name                      | Type      | Description                                        |
+===========================+===========+====================================================+
| chain                     | String    | Special code of the chain                          |
+---------------------------+-----------+----------------------------------------------------+
| refNo                     | String    | External ID, use any string (64 chars)             |
+---------------------------+-----------+----------------------------------------------------+
| goodsType\ :sup:`1`       | String    | Data from ``shipping-methods`` goodsTypeList       |
+---------------------------+-----------+----------------------------------------------------+
| pickupDate\ :sup:`2`      | String    | Date from ``pickup-dates``                         |
+---------------------------+-----------+----------------------------------------------------+
| partnerSpecific\ :sup:`3` | Object    | Custom parameters specific for a certain partner   |
+---------------------------+-----------+----------------------------------------------------+
| providerSpecific\ :sup:`4`| Object    | Same as partnerSpecific but for service provider  |
+---------------------------+-----------+----------------------------------------------------+
| packaging                 | Boolean   | Does customer wants to request packing materials   |
+---------------------------+-----------+----------------------------------------------------+
| mail                      | String    | Used for some specific cases                       |
+---------------------------+-----------+----------------------------------------------------+
| consents                  | Object    | List of consents                                   |
+---------------------------+-----------+----------------------------------------------------+

\ :sup:`1` ``goodsType`` is mandatory only if the corresponding shipping method has ``properties.requirePickupDate: true``. Check the API ref for ``/api/v1/shipping-methods`` for more detailes.

\ :sup:`2` ``pickupDate`` is mandatory only if the corresponding shipping method has **not** ``null`` for ``properties.goodsTypeList``. Check the API ref for ``/api/v1/shipping-methods`` for more detailes.

\ :sup:`3` ``partnerSpecific`` is an optional object for custom parameters specific for a certain ingegration with partner. Parameter's names should be added to the serviceorderhub config before you can use it. Please contact support for that.

**Example of partnerSpecific**

.. code:: js

    "partnerSpecific": {
        "superStoreScandinavia": {
            "testParam1": 350,
            "testParam2": "FREETEXT123",
        }
    }

\ :sup:`4` ``providerSpecific`` is an optional object for custom parameters specific for a certain ingegration with service provider. Parameter's names should be added to the serviceorderhub config before you can use it. Please contact support for that.

**Example of providerSpecific**

.. code:: js

    "providerSpecific": {
        "nordicService": {
            "testParam1": 1,
            "testParam2": "FREETEXT123",
        }
    }

ProductData
~~~~~~~~~~~

Contains info about the product.

+------------------------+--------------+--------------------------------------+
| Name                   | Type         | Description                          |
+========================+==============+======================================+
| model\*                | String       | Product's model name (40 chars)      |
+------------------------+--------------+--------------------------------------+
| purchaseDate           | Date         | When the product was purchased       |
+------------------------+--------------+--------------------------------------+
| imei\*\*               | String       | Product's IMEI (20 chars)            |
+------------------------+--------------+--------------------------------------+
| serial\*\*             | String       | Product's Serial number (40 chars)   |
+------------------------+--------------+--------------------------------------+
| insuranceCompany\*\*   | String       | Insurance company name (50 chars)    |
+------------------------+--------------+--------------------------------------+
| insuranceNumber\*\*    | String       | Insurance number                     |
+------------------------+--------------+--------------------------------------+
| accessory              | Array        | List of accessory Ids                |
+------------------------+--------------+--------------------------------------+
| otherAccessory         | String       | Names of additional accessory        |
+------------------------+--------------+--------------------------------------+
| problemText\*          | String       | Problem description                  |
+------------------------+--------------+--------------------------------------+
| password               | String       | Product's password (if applicable)   |
+------------------------+--------------+--------------------------------------+
| weight                 | Number       | Product's weight (in kg)             |
+------------------------+--------------+--------------------------------------+
| volume                 | Dimensions   | Width, height, depth                 |
+------------------------+--------------+--------------------------------------+

\*\* These fields may become mandatory in some conditions.

ContactData
~~~~~~~~~~~

Contains info about person or company.

+-----------------------+---------+----------------------------------+
| Name                  | Type    | Description                      |
+=======================+=========+==================================+
| type\*                | Boolean | 1 - company, 0 - private person  |
+-----------------------+---------+----------------------------------+
| firstName\*1          | String  | Person’s first name (64 chars)   |
+-----------------------+---------+----------------------------------+
| lastName\*1           | String  | Person’s last name (64 chars)    |
+-----------------------+---------+----------------------------------+
| organizationName\*2   | String  | Org Name (64 chars)              |
+-----------------------+---------+----------------------------------+
| organizationNumber\*2 | String  | Organization Number (64 chars)   |
+-----------------------+---------+----------------------------------+
| mobile\*              | String  | Cellphone (19 chars)             |
+-----------------------+---------+----------------------------------+
| phone                 | String  | Landline phone (64 chars)        |
+-----------------------+---------+----------------------------------+
| email\*               | String  | Email (64 chars)                 |
+-----------------------+---------+----------------------------------+
| address\*             | String  | Address (64 chars)               |
+-----------------------+---------+----------------------------------+
| addName               | String  | Additional name (c/o) (64 chars) |
+-----------------------+---------+----------------------------------+
| postalCode\*          | String  | Postal code (6 chars)            |
+-----------------------+---------+----------------------------------+
| city\*                | String  | City (64 chars)                  |
+-----------------------+---------+----------------------------------+
| countryCode           | String  | Country code (2 capital letters) |
+-----------------------+---------+----------------------------------+

\*1 Allowed only when user is a private person

\*2 Allowed only when user is a company

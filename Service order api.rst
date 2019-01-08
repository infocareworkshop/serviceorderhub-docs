Service order API
=================

**Note** Mandatory parameters are marked with \* sign.

GET /api/v1/manufacturers
-------------------------

Get manufacturers (brands).

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| originatorType\*   | Originator    | Originator type            |
+--------------------+---------------+----------------------------+
| bookingType\*      | BookingType   |                            |
+--------------------+---------------+----------------------------+
| serviceType        | Int           | Service type Id            |
+--------------------+---------------+----------------------------+
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
+--------------------+---------------+----------------------------+
| productType        | Int\|String   | Product type Id or alias   |
+--------------------+---------------+----------------------------+
| clientPostalCode   | String        |                            |
+--------------------+---------------+----------------------------+

Output:
~~~~~~~

.. code:: js

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

GET /api/v1/service-types
-------------------------

Get service types.

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| originatorType\*   | Originator    | Originator type            |
+--------------------+---------------+----------------------------+
| bookingType\*      | BookingType   |                            |
+--------------------+---------------+----------------------------+
| serviceType        | Int           | Service type Id            |
+--------------------+---------------+----------------------------+
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
+--------------------+---------------+----------------------------+
| productType        | Int\|String   | Product type Id or alias   |
+--------------------+---------------+----------------------------+
| clientPostalCode   | String        |                            |
+--------------------+---------------+----------------------------+

Output:
~~~~~~~

.. code:: js

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

Field ``properties`` contains info about required fields

GET /api/v1/product-types
-------------------------

Get product types.

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| originatorType\*   | Originator    | Originator type            |
+--------------------+---------------+----------------------------+
| bookingType\*      | BookingType   |                            |
+--------------------+---------------+----------------------------+
| serviceType\*      | Int           | Service type Id            |
+--------------------+---------------+----------------------------+
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
+--------------------+---------------+----------------------------+
| productType        | Int\|String   | Product type Id or alias   |
+--------------------+---------------+----------------------------+
| clientPostalCode   | String        |                            |
+--------------------+---------------+----------------------------+

Output:
~~~~~~~

.. code:: js

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

Field ``properties`` contains info about required fields.

GET /api/v1/service-locations
-----------------------------

Get service locations.

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| originatorType\*   | Originator    | Originator type            |
+--------------------+---------------+----------------------------+
| bookingType\*      | BookingType   |                            |
+--------------------+---------------+----------------------------+
| serviceType\*      | Int           | Service type Id            |
+--------------------+---------------+----------------------------+
| manufacturer\*     | Int\|String   | Manufacturer Id or alias   |
+--------------------+---------------+----------------------------+
| productType\*      | Int\|String   | Product type Id or alias   |
+--------------------+---------------+----------------------------+
| clientPostalCode   | String        |                            |
+--------------------+---------------+----------------------------+

Output:
~~~~~~~

.. code:: js

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

GET /api/v1/accessory
---------------------

Get accessory.

Input:
~~~~~~

+-----------------+--------+-------------------+
| Name            | Type   | Description       |
+=================+========+===================+
| productType\*   | Int    | Product type Id   |
+-----------------+--------+-------------------+

Output:
~~~~~~~

.. code:: js

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

GET /api/v1/shipping-methods
----------------------------

Get allowed shipping methods.

Input:
~~~~~~

+---------------------+---------------+----------------------------+
| Name                | Type          | Description                |
+=====================+===============+============================+
| originatorType\*    | Originator    | Originator type            |
+---------------------+---------------+----------------------------+
| serviceType\*       | Int           | Service type Id            |
+---------------------+---------------+----------------------------+
| manufacturer\*      | Int\|String   | Manufacturer Id or alias   |
+---------------------+---------------+----------------------------+
| productType\*       | Int\|String   | Product type Id or alias   |
+---------------------+---------------+----------------------------+
| clientPostalCode    | String        |                            |
+---------------------+---------------+----------------------------+
| serviceLocation\*   | Int           | Service location Id        |
+---------------------+---------------+----------------------------+

Output:
~~~~~~~

.. code:: js

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

If ``requirePickupDate`` of the ``properties`` object is not ``false``, the parameter ``order.pickupDate`` becomes mandatory for case/create and case/validate endpoints!

If ``goodsTypeList` `of the ``properties`` object is not ``null``, the parameter ``order.goodsType`` becomes mandatory for case/create and case/validate endpoints!

GET /api/v1/shipping/pickup-dates
---------------------------------

Get allowed pickup dates for this shipping methods.

Input:
~~~~~~

+--------------------+----------+---------------------------------------------+
| Name               | Type     | Description                                 |
+====================+==========+=============================================+
| shippingMethod\*   | Int      | ``id`` from ``shipping-methods`` endpoint   |
+--------------------+----------+---------------------------------------------+
| postalCode\*       | String   | Postal code (3-10) chars                    |
+--------------------+----------+---------------------------------------------+

Output:
~~~~~~~

.. code:: js

    [
      "2017-05-23T00:00:00.000Z",
      "2017-05-26T00:00:00.000Z",
      "2017-06-08T00:00:00.000Z",
      "2017-06-09T00:00:00.000Z",
      "2017-06-21T00:00:00.000Z",
      "2017-06-22T00:00:00.000Z"
    ]

POST /api/v1/case/validate
--------------------------

Validate Case. All parameters are same as in ``case/create`` endpoint.
In case of validation errors status will be "ok" and debug info will be
passed through ``data`` field.

**Note** Currently we don't have a possibility to validate shipments and
we can't guarantee that shipment will be created for the Case passed
this validation.

Output:
~~~~~~~

When data is correct:

.. code:: js

    {
      "result": true
    }

When data is incorrect:

.. code:: js

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

POST /api/v1/case/create
------------------------

Create a new Case.

Input:
~~~~~~

+-----------------------------+---------------+----------------------------------------------------------+
| Name                        | Type          | Description                                              |
+=============================+===============+==========================================================+
| serviceType\*               | Int           | Id from ``service-types``                                |
+-----------------------------+---------------+----------------------------------------------------------+
| manufacturer\*              | Int\|String   | Id or alias from ``manufacturers``                       |
+-----------------------------+---------------+----------------------------------------------------------+
| productType\*               | Int\|String   | Id or alias from ``product-types``                       |
+-----------------------------+---------------+----------------------------------------------------------+
| shipping\ :sup:`1`          | Int           | Id from ``shipping-methods``                             |
+-----------------------------+---------------+----------------------------------------------------------+
| location\*                  | Int           | Id from ``service-locations``                            |
+-----------------------------+---------------+----------------------------------------------------------+
| order\*                     | OrderData     | Order data                                               |
+-----------------------------+---------------+----------------------------------------------------------+
| product\*                   | ProductData   | Product data                                             |
+-----------------------------+---------------+----------------------------------------------------------+
| pickupDestination\ :sup:`2` | String        | ``consumer`` or ``customCompany`` or ``customPrivate``   |
+-----------------------------+---------------+----------------------------------------------------------+
| returnDestination\ :sup:`2` | String        | ``consumer`` or ``customCompany`` or ``customPrivate``   |
+-----------------------------+---------------+----------------------------------------------------------+
| clientPostalCode\*          | String        |                                                          |
+-----------------------------+---------------+----------------------------------------------------------+
| customer\*                  | ContactData   | Info about user who books this order                     |
+-----------------------------+---------------+----------------------------------------------------------+
| consumer\ :sup:`3`          | ContactData   | Info about end user                                      |
+-----------------------------+---------------+----------------------------------------------------------+
| pickupDst\ :sup:`4`         | ContactData   | Where shipment will be picked up or sent from            |
+-----------------------------+---------------+----------------------------------------------------------+
| returnDst\ :sup:`5`         | ContactData   | Where shipment should be delivered after repair          |
+-----------------------------+---------------+----------------------------------------------------------+
| originatorType\*            | Originator    | Originator type                                          |
+-----------------------------+---------------+----------------------------------------------------------+
| bookingType\*               | BookingType   | Booking type                                             |
+-----------------------------+---------------+----------------------------------------------------------+
| acceptConditions\*          | Boolean       | Terms and condition acceptance. Should be ``true``       |
+-----------------------------+---------------+----------------------------------------------------------+
| noPassword                  | Boolean       | Make ``product.password`` optional                       |
+-----------------------------+---------------+----------------------------------------------------------+


\ :sup:`1` ``shipping`` can be assigned automatically by the Service Order Hub if
shipping is required according to the business rules, but no shipping
method id was sent.

\ :sup:`2` Enabled when originatorType = ``helpdesk``

\ :sup:`3` Enabled when bookingType ≠ ``privateToPrivate``

\ :sup:`4` Enabled when pickupDestination ≠ ``consumer``. If pickupDestination is not sent, we use ``pickupDestination: "consumer"`` by default

\ :sup:`5` Enabled when returnDestination ≠ ``consumer``. If returnDestination is not sent, we use ``returnDestination: "consumer"`` by default

Example:
~~~~~~~~

::

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

Output:
~~~~~~~

.. code:: js

    {
      "id": 7707, // Case's Id
      "guid": "69b0e17f-eeb9-4834-b809-60b015054c0d" // Case's GUID
    }

GET /api/v1/shipping/label
--------------------------

Get shipping label (Base64 encoded PDF file).

Input:
~~~~~~

+--------+--------+---------------+
| Name   | Type   | Description   |
+========+========+===============+
| guid   | GUID   | Case's GUID   |
+--------+--------+---------------+

Output:
~~~~~~~

*(Shown with wrapper object)*

.. code:: js

    {
      "data": "dataJVBERi0xLjQNCiXi48/TDQo..."
    }

GET /api/v1/case
----------------

Get all info about a certain Case.

Input:
~~~~~~

+--------+--------+---------------+
| Name   | Type   | Description   |
+========+========+===============+
| guid   | GUID   | Case's GUID   |
+--------+--------+---------------+

Output:
~~~~~~~

.. code:: js

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

POST /api/v1/search-cases
-------------------------

Get list of cases

Input (JSON body):
~~~~~~~~~~~~~~~~~~

+---------------------+----------+---------------------------+
| Name                | Type     | Description               |
+=====================+==========+===========================+
| filters             | Object   | Search query              |
+---------------------+----------+---------------------------+
| pagination          | Object   | Pagination settings       |
+---------------------+----------+---------------------------+
| pagination.offset   | Int      | Default = 0               |
+---------------------+----------+---------------------------+
| pagination.limit    | Int      | Default = 10, max = 100   |
+---------------------+----------+---------------------------+

Example:
~~~~~~~~

::

    {
      "filters": { "orderData": { "partnerSpecific": { "partnerName": { "internalReference": 3112312312 }}}}
    }

You can also use special keys ``$lt`` and ``$gt`` for "<" and ">"
conditions

::

    {
      "filters": { "createdAt": { "$gt": "2017-01-01" }}
    }

*Allowed keys for querying:*

id, guid, partnerId, serviceProviderId, manufacturerId, currentStatus,
productData.\*, orderData.\*, serviceTypeId, serviceLocationId,
productTypeId, createdAt

All Partner and Service provider specific data is stored in
``orderData.partnerSpecific.<urlSlug>`` and
``orderData.providerSpecific.<urlSlug>`` objects.

Service partners can view only partnerSpecific data with their urlSlug.

Service providers can view all data.

Output
~~~~~~

.. code-block:: js

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

POST /api/v1/case/accept
------------------------

Endpoint for the service provider to confirm that the case was accepted
for repair and imported to their production system 

Input (JSON body):
~~~~~~~~~~~~~~~~~~

+----------+----------+-----------------------------------------+
| Name     | Type     | Description                             |
+==========+==========+=========================================+
| guid     | GUID     | Case's GUID                             |
+----------+----------+-----------------------------------------+


Example:
~~~~~~~~

.. code:: js

    {
      "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063"
    }

POST /api/v1/case/cancel
------------------------

Endpoint for the service provider to notify ServiceOrderHub that the
case import was canceled and the provider revokes the acceptance to
handle the service case.

Input (JSON body):
~~~~~~~~~~~~~~~~~~

+----------+----------+-----------------------------------------+
| Name     | Type     | Description                             |
+==========+==========+=========================================+
| guid     | GUID     | Case's GUID                             |
+----------+----------+-----------------------------------------+
| reason   | String   | Why this case was rejected (Optional)   |
+----------+----------+-----------------------------------------+

Example:
~~~~~~~~

.. code:: js

    {
      "guid": "c0167135-2b0f-471b-81d6-c06d91cfb063"
    }

Output:
~~~~~~~

.. code:: js

    {
        "data": {
            "result": true
        }
    }

POST /api/v1/unexported-cases
-----------------------------

Get a list of cases unaccepted by the Service Provider.

The request has the same body structure, parameters and output as `POST
/api/v1/search-cases <#post-apiv1search-cases>`__

POST /api/v1/case/update
------------------------

Modify Case.

Input:
~~~~~~

+---------------+----------+--------------------+
| Name          | Type     | Description        |
+===============+==========+====================+
| guid\*        | GUID     | Guid of the Case   |
+---------------+----------+--------------------+
| orderData     | Object   | Modified fields    |
+---------------+----------+--------------------+
| productData   | Object   | Modified fields    |
+---------------+----------+--------------------+
| customer      | Object   | Modified fields    |
+---------------+----------+--------------------+
| consumer      | Object   | Modified fields    |
+---------------+----------+--------------------+
| pickupDst     | Object   | Modified fields    |
+---------------+----------+--------------------+
| returnDst     | Object   | Modified fields    |
+---------------+----------+--------------------+

Example:
~~~~~~~~

.. code:: js

    {
      "guid": "d2379b74-d9b0-40cd-92e7-d1f48f398f43",
      "orderData": {
        "infocareSesamOriginator": "Microsoft",
        "consents": ["hello", "world"]
      }
    }

Output:
~~~~~~~

.. code:: js

    {
      "result": true
    }


POST /api/v1/case/send-completed-email
--------------------------------------

Resend final email to customers.

Input:
~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| guid\*             | GUID          | Guid of the case           |
+--------------------+---------------+----------------------------+
| email              | String        | Receiver's email           |
+--------------------+---------------+----------------------------+

Example:
~~~~~~~~

.. code:: js

    {
      "guid": "d647067f-20d4-4ac3-891e-a4549e727bee",
      "email": "test@example.com"
    }

Output:
~~~~~~~

.. code:: js

    {
      "data": {
         "result": true // Or string with an information message
      }
    }
    
If ``email`` is empty then fields of the case will be used.

If email cant't be sent then a string with message will be returned in ``result`` field (Sometimes emails are disabled in settings).


GET /api/v1/routing
-------------------

Determine which workshop will be used to process a case with specific parameters. 
This method also returns competence data.

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| originatorType     | Originator    | Originator type            |
+--------------------+---------------+----------------------------+
| bookingType        | BookingType   |                            |
+--------------------+---------------+----------------------------+
| serviceType        | Int           | Service type Id            |
+--------------------+---------------+----------------------------+
| manufacturer       | Int\|String   | Manufacturer Id or alias   |
+--------------------+---------------+----------------------------+
| productType        | Int\|String   | Product type Id or alias   |
+--------------------+---------------+----------------------------+
| clientPostalCode   | String        |                            |
+--------------------+---------------+----------------------------+

All parameters are optional by default, but when provided data is not enough the system will throw an error.

Output:
~~~~~~~

.. code:: js

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

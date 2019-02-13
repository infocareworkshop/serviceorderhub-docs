Cost proposal API
=================

This part of API allows partners to get cost proposals and approve them.
Service providers can create or update cost proposals for cases.

**Cost proposal concept:**

Cost proposals in the Service Order Hub can have two states, either they are approved or not. There is no such state as "rejected". Instead it can be many cost proposals with "id" as a unique key. If the partner doesn't want to approve a certian cost proposal they should contact the provider using the messages endpoints of this API, so the provider may issue another one.

One case may have many cost proposals with different ids and names, e.g. four cost proposals: two cost proposals with name "repair", one with name "return", and one with name "scrap". Cost proposals' names can help to group cost proposals on service partner's. Those names are different depending on service provider. 

In the example above, two cost proposals with the name "repair" could appear when the service provider issued another cost proposal for repair maybe because the previous was not correct or acceptet. It's easy to sort cost proposals with the same name (read "type") by date since they have timestamps. The older one should also have the parameter "active" set to "false". 

The Service Order Hub stores and returns all cost proposals ever created for the case. The service partner can accept only one cost proposal. If the case already has an accepted cost proposal and the partner accepts another, the previous cost proposal loses the flag "accepted". Please note that once a cost proposal is accepted it may be a legal reason for the service provider to invoice the corresponding amount to the service partner even if the partner later changed their mind and asked for another cost proposal. These conditions are a subject of direct negotiations between service partner and service provider.

It's also not neccessary that every case can contain all possible cost proposals. It can contain three, two, ten or only one, e.g. one cost proposal with name "repair".

Details
~~~~~~~

Cost proposals can contain an array with details for storing cost specification. The parameter ``type`` is a free text parameter, but it has some recommended values.

+------------+------------------------------------------+
| type       | Description                              |
+============+==========================================+
| labor      | Costs related to labor                   |
+------------+------------------------------------------+
| shipping   | Costs related to shipping                |
+------------+------------------------------------------+
| sparePart  | Costs related to spare parts             |
+------------+------------------------------------------+
| deductible | Costs related to insurance deductibles   |
+------------+------------------------------------------+

**Note** Mandatory parameters are marked with \* sign.

GET /api/v1/cost-proposals
--------------------------

Get all cost proposals attached to specific *Case*.

Input:
~~~~~~

+----------+-----------+------------------------------------------+
| Name     | Type      | Description                              |
+==========+===========+==========================================+
| guid\*   | Guid      | Guid of the case                         |
+----------+-----------+------------------------------------------+
| active   | Boolean   | Get proposals allowed for approval only  |
+----------+-----------+------------------------------------------+

Example:
~~~~~~~~

::

    /api/v1/cost-proposals?accessToken=key&guid=3ebad3cc-61d3-479a-bd92-510e8774ce5a&active=true

Output:
~~~~~~~

.. code:: js

    [
      {
        "id": 2,
        "externalId": ["123456", "test"],
        "name": "repair",
        "priceNet": "600.00",
        "currency": "SEK",
        "dateValidUntil": null,
        "isActive": true,
        "dateAccepted": null,
        "approverName": null,
        "details": [
          {
            "line": 1,
            "text": "XGHF12 LCD Screen",
            "type": sparePart,
            "priceNet": 500,
            "quantity": 1
          },
          {
            "line": 2,
            "text": "",
            "type": shipping,
            "priceNet": 100,
            "quantity": 1
          }
        ]
      },
      {
        "id": 5,
        "externalId": 213214,
        "name": "Scrap",
        "priceNet": "1860.50",
        "currency": "SEK",
        "dateValidUntil": null,
        "dateAccepted": "2017-07-14T10:14:13.037Z",
        "approverName": "Somebody",
        "details": null,
        "createdAt": "2017-07-12T10:14:13.037Z",
        "updatedAt": "2017-07-14T15:54:09.613Z"
      }
    ]

POST api/v1/cost-proposals/approve
----------------------------------

Approve *Cost Proposal*.

*Note* You can approve only one Cost Proposal for case.

Input:
~~~~~~

+------------------+-----------+----------------------------------------+
| Name             | Type      | Description                            |
+==================+===========+========================================+
| id\*             | Integer   | Id of Cost Proposal (GET parameter)    |
+------------------+-----------+----------------------------------------+
| approverName\*   | String    | Who perfoms this approve (POST body)   |
+------------------+-----------+----------------------------------------+

Example:
~~~~~~~~

::

    /api/v1/cost-proposals/approve?accessToken=key&id=2

    {
     "approverName": "John Smith"
    }

Output:
~~~~~~~

.. code:: js

    {
      data: { /* Cost Proposal */}
    }

POST api/v1/cost-proposals
--------------------------

Add or modify multiple *Cost Proposals*.

*Note* You can approve only one Cost Proposal per case.

Example:
~~~~~~~~

::

    /api/v1/cost-proposals/

    [
      {
        "guid": "3ebad3cc-61d3-479a-bd92-510e8774ce5a", // Case's guid
        "externalId": ["123456", "test"], // Array of strings
        "name": "Scrap", // Type of work, pair externalId and name must be unique for Service provider
        "priceNet": 1860.5, // Price (optional)
        "currency": "SEK", // Currency in ISO 4217 (3 capital letters) (optional)
        "timeStamp": "2017-07-12 13:14:13.037", // date created (optional)
        "isActive": true, // Can this Cost Proposal be approved?
        "dateAccepted": "2017-07-14 13:14:13.037", // when this Cost proposal was accepted (optional)
        "approverName": "John Smith" // Who performed approval (optional)
        "details": [ // Optional array of details (specification)
          {
            "line": 1, // Counter from 1
            "text": "Two-way delivery", // Text description
            "type": shipping, // Type of line
            "priceNet": 100, // Price
            "quantity": 1 // Quantity
          },
          ...
        ]
      }
    ]

Output:
~~~~~~~

.. code:: js

    {
      data: [/* List of created Cost Proposals */]
    }

If the same pair (externalId, name) passes second time, then an update will be performed.

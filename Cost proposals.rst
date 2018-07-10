Cost proposal API
=================

This part of API allows partners to get cost proposals and approve them.
Service providers can create or update cost proposals for cases.

**Cost proposal concept:**

Cost proposals in the Service Order Hub can have two states, either they are approved or not. There is no such state as "rejected". Instead it can be many cost proposals with "id" as a unique key. If the partner doesn't want to approve a certian cost proposal they should contact the provider using the messages endpoints of this API, so the provider may issue another one.

One case can have four cost proposals with different ids and names, e.g. two cost proposals with name "repair", one with name "return", and one with name "scrap". Cost proposals' names can help to group cost proposals on service partner's. Those names are different depending on service provider. 

Two cost proposals with the name "repair" can appear when the service provider issued another cost proposal for repair maybe because the previous was not correct or acceptet. It's easy to sort them by date since they have timestamps. The older one should also have the parameter "active" set to "false". The Service Order Hub stores and returns all cost proposals ever created for the case. The service partner can accept only one cost proposal. If the case already has an accepted cost proposal and the partner accepts another, the previous cost proposal loses the flag "accepted". Please note that once a cost proposal is accepted it may be a legal reason for the service provider to invoice the corresponding amount to the service partner even if the partner later changed their mind and asked for another cost proposal. These conditions are a subject of direct negotiations between service partner and service provider.

It's also not neccessary that every case can contain all possible cost proposals. It can contain only two or even one, e.g. one cost proposal with name "repair". 

--------------------------

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
        "priceNet": "200.00",
        "currency": "SEK",
        "dateValidUntil": null,
        "isActive": true,
        "dateAccepted": null,
        "approverName": null,
        "details": [
          {
            "line": 1,
            "text": "shipping",
            "type": null,
            "priceNet": 100,
            "quantity": 0
          },
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
        "currency": "SEK", // Currency, 3 capital letters (optional)
        "timeStamp": "2017-07-12 13:14:13.037", // date created (optional)
        "isActive": true, // Can we approve this Cost Proposal
        "dateAccepted": "2017-07-14 13:14:13.037", // when this Cost proposal was been accepted (optional)
        "approverName": "John Smith" // Who performed approve (optional)
        "details": [ // Optional array of details
          {
            "line": 1, // Counter from 1
            "text": "shipping", // Type of work (text)
            "type": null, // Type of work (id)
            "priceNet": 100, // Price
            "quantity": 0 // Quantity
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

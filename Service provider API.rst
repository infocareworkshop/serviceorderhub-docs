Service provider API
=================

**Note** Mandatory parameters are marked with \* sign.

POST /api/v1/case/status
------------------------

Add status to case.

Input:
~~~~~~

+--------------------+---------------+----------------------------+
| Name               | Type          | Description                |
+====================+===============+============================+
| guid\*             | GUID          | Guid of the case           |
+--------------------+---------------+----------------------------+
| key\*              | String        | Status key                 |
+--------------------+---------------+----------------------------+

Example:
~~~~~~~~

.. code:: js

  {
    "guid": "a7180ff9-7c6e-4d0e-b756-16b49531382f",
    "key": "final" 
  }
  
Output:
~~~~~~~

.. code:: js

  {
    "data": {
      "id": 98, // Status Id
      "caseId": 7927, // Case Id
      "guid": "a7180ff9-7c6e-4d0e-b756-16b49531382f", // Case guid
      "key": "final",
      "createdAt": "2019-01-22T11:29:38.258Z",
      "updatedAt": "2019-01-22T11:29:38.258Z"
    }
  }

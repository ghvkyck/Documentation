.. _refDelegationManagementEndpoint:

Delegation Management Endpoint
===================

Used to manage delegations by a Delegation Policy Creator. The Delegation Policy Creator can create, update and delete delegations that are stored at an Authorization Registry. In order to do so the Delegation Policy Creator must provide delegation evidence that it is allowed to do so.

This endpoint is modeled based on the abstract service endpoint of a service provider. 

Request (generic)
-----------------

HTTP methods
~~~~~~~~~~~~

* GET
* POST
* PUT
* DELETE

Headers
~~~~~~~

``Authorization``
    | **String**.
    | OAuth 2.0 authorization based on bearer token. MUST contain "Bearer " + access token value. How to retrieve the access token can be found at :ref:`Access Token Endpoint section<refM2MToken>`.

``delegation_evidence``
    | **String**. *Optional*.
    | iSHARE delegation evidence regarding the requested service. The Service Consumer can obtain this evidence from a :ref:`Delegation Endpoint<refDelegationEndpoint>` before requesting a specific service. The evidence should include the following the Resource type ISHARE.DELEGATION and the license ISHARE.9998.  

``service_consumer_assertion``
    | **String**. *Optional*.
    | iSHARE specific optional client assertion. Used when a Service Consumer is requesting a service on behalf of another Service Consumer in a *service broker* pattern. It is used to prove that the *brokering* Service Consumer indeed has had a request from the original Service Consumer.

``Content-Type``
    | **String**.
    | Defines request body content type. MUST be equal to *application/json*.

HTTP GET
--------

URLS
~~~~
``/delegations``
``/delegations/{id}``

Parameters
~~~~~~~~~~

``{id}`` a path parameter to get a specific delegation

RESPONSE HTTP status codes
~~~~~~~~~~~~~~~~~

200 OK
    | When a valid request is sent an OK result should be returned, including the response payload.

400 Bad Request
    | When an access token is valid but request itself is invalid.

401 Unauthorized
    | When ``Authorization`` header is either missing, invalid or token has already expired.

200 OK Example
~~~~~~~~~~~~~~

.. code-block:: json

    [
        {
          "delegation": {
            "id": 12345
            "notBefore": 1541058939,
            "notOnOrAfter": 2147483647,
            "policyIssuer": "EU.EORI.NL000000005",
            "target": {
              "accessSubject": "EU.EORI.NL000000001"
            },
            "policySets": [
              {
                "maxDelegationDepth": 0,
                "target": {
                  "environment": {
                    "licenses": [
                      "ISHARE.0001"
                    ]
                  }
                },
                "policies": [
                  {
                    "target": {
                      "resource": {
                        "type": "ISHARE.DELEGATION",
                        "identifiers": [
                          "1111"
                        ],
                        "attributes": [
                          "AAAA",
                          "AAAA"
                        ]
                      },
                      "environment": {
                        "serviceProviders": [
                          "EU.EORI.NL000000003"
                        ]
                      },
                      "actions": [
                        "ISHARE.READ",
                        "ISHARE.CREATE",
                        "ISHARE.UPDATE",
                        "ISHARE.DELETE"
                      ]
                    },
                    "rules": [
                      {
                        "effect": "Permit"
                      }
                    ]
                  }
                ]
              }
            ]
          }
        }
    ]

NEEDS FURTHER DOCUMENTATION

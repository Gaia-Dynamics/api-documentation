# API

## Create Company
**Description**: Creates a new company with details including name, type, address, identifiers, and contacts. \
**Method**: POST \
**Endpoint**: `/company`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "company": {
    "name": "Acme",
    "is_customer": true,
    "is_manufacturer": false,
    "address_line1": "2nd Avenue, 4000 - Brooklin",
    "address_line2": "Building 4th floor, 1st room",
    "address_zipcode": "0888283",
    "address_city": "New York",
    "address_country": "`country.uuid`",
    "address_state": "`state.uuid`",
    "identifiers": [
      {
        "type": "EIN",
        "value": "234234234"
      },
      {
        "type": "SSN",
        "value": "345345345"
      },
      {
        "type": "CBP_ASSIGNED_NUMBER",
        "value": "432432432"
      }
    ],
    "contacts": [
      {
        "name": "Emil Stephan",
        "is_principal": true,
        "email": "emil@gaiadynamis.ai",
        "phone": "+1234567",
        "job_title": "CEO"
      },
      {
        "name": "Jhony Brabo",
        "is_principal": false,
        "email": "jhony@gaiadynamis.ai",
        "phone": "+1555557",
        "job_title": "Founder"
      }
    ]
  }
}
```

**Responses**:

- **Description**: Company created successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "c2f...",
      "name": "Acme",
      "is_customer": true,
      "is_manufacturer": false,
      "address_line1": "2nd Avenue, 4000 - Brooklin",
      "address_line2": "Building 4th floor, 1st room",
      "address_zipcode": "0888283",
      "address_city": "New York",
      "created_at": "2025-06-03T15:00:00Z",
      "updated_at": "2025-06-03T15:00:00Z",
      "address_country": {
        "uuid": "`country.uuid`",
        "name": "United States",
        "iso_code": "US"
      },
      "address_state": {
        "uuid": "`state.uuid`",
        "name": "Nova York",
        "iso_code": "NY"
      },
      "identifiers": [
        {
          "uuid": "`identifier.uuid`",
          "type": "EIN",
          "value": "234234234",
          "created_at": "2025-06-03T15:00:00Z",
          "updated_at": "2025-06-03T15:00:00Z"
        },
        {
          "uuid": "`identifier.uuid`",
          "type": "SSN",
          "value": "345345345",
          "created_at": "2025-06-03T15:00:00Z",
          "updated_at": "2025-06-03T15:00:00Z"
        },
        {
          "uuid": "`identifier.uuid`",
          "type": "CBP_ASSIGNED_NUMBER",
          "value": "432432432",
          "created_at": "2025-06-03T15:00:00Z",
          "updated_at": "2025-06-03T15:00:00Z"
        }
      ],
      "contacts": [
        {
          "uuid": "`contact.uuid`",
          "is_principal": true,
          "name": "Emil Stephan",
          "email": "emil@gaiadynamis.ai",
          "phone": "+1234567",
          "job_title": "CEO",
          "created_at": "2025-06-03T15:00:00Z",
          "updated_at": "2025-06-03T15:00:00Z"
        },
        {
          "uuid": "`contact.uuid`",
          "is_principal": false,
          "name": "Jhony Brabo",
          "email": "jhony@gaiadynamis.ai",
          "phone": "+1555557",
          "job_title": "Founder",
          "created_at": "2025-06-03T15:00:00Z",
          "updated_at": "2025-06-03T15:00:00Z"
        }
      ]
    }
  }
  ```

- **Description**: Identifier already exists in the system \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 4,
      "error": "Identifier Already Taken",
      "detail": "Identifier EIN::123123123 already taken",
      "status_code": 400
    }
  }
  ```

- **Description**: Same identifier type provided more than once \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 5,
      "error": "Identifier Type Duplication",
      "detail": "Identifier of type EIN is duplicated",
      "status_code": 400
    }
  }
  ```

- **Description**: Duplicate email used in contact list \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 7,
      "error": "Contact Email Duplication",
      "detail": "Contact email emil2@gaiadynamis.ai is duplicated",
      "status_code": 400
    }
  }
  ```

- **Description**: Email already registered to another contact \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 6,
      "error": "Contact Already Taken",
      "detail": "Contact email::emil@gaiadynamis.ai already taken",
      "status_code": 400
    }
  }
  ```

- **Description**: More than one principal contact provided \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 8,
      "error": "Contact is principal Duplication",
      "detail": "Contact is principal Duplication",
      "status_code": 400
    }
  }
  ```

- **Description**: Company must be at least one type (customer, manufacturer, etc.) \
  **HTTP Code**: 400

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 9,
      "error": "Missing Company Type",
      "detail": "Company must to be at least of one type (customer, manufacturer, consignee, record_importer)",
      "status_code": 400
    }
  }
  ```

- **Description**: Company created with minimal required fields \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "`company.uuid`",
      "name": "Acme",
      "is_customer": false,
      "is_manufacturer": true,
      "address_line1": null,
      "address_line2": null,
      "address_zipcode": null,
      "address_city": null,
      "created_at": "2025-06-03T15:00:00Z",
      "updated_at": "2025-06-03T15:00:00Z",
      "address_country": null,
      "address_state": null,
      "identifiers": [],
      "contacts": []
    }
  }
  ```

## View Company
**Description**: Retrieves a company's detailed information by its UUID. \
**Method**: GET \
**Endpoint**: `/company/${company.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Company details returned successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${company.uuid}",
      "name": "Acme",
      "is_customer": true,
      "is_manufacturer": false,
      "address_line1": "2nd Avenue, 3444 - Brooklin",
      "address_line2": null,
      "address_zipcode": "0888032",
      "address_city": "New York",
      "address_country": {
        "uuid": "${country.uuid}",
        "name": "United States",
        "iso_code": "US"
      },
      "address_state": {
        "uuid": "${state.uuid}",
        "name": "Nova York",
        "iso_code": "NY"
      },
      "identifiers": [
        {
          "uuid": "${identifier.uuid}",
          "type": "EIN",
          "value": "123123123",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        },
        {
          "uuid": "${identifier.uuid}",
          "type": "CBP_ASSIGNED_NUMBER",
          "value": "321321321",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ],
      "contacts": [
        {
          "uuid": "${contact.uuid}",
          "is_principal": true,
          "name": "Stev Jobs",
          "email": "stev.jobs@acme.com",
          "phone": "+1999935323",
          "job_title": "Founder",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ],
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z"
    }
  }
  ```

- **Description**: The company was not found for the given UUID \
  **HTTP Code**: 404

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 2,
      "error": "Not Found",
      "detail": "Company not found with uuid: ${company.uuid}",
      "status_code": 404
    }
  }
  ```


## Update Company
**Description**: Updates an existing company's information using its UUID. \
**Method**: PUT \
**Endpoint**: `/company/${company.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "company": {
    "name": "Acme2",
    "is_customer": true,
    "is_manufacturer": false,
    "address_line1": "2nd Avenue, 4000 - Brooklin",
    "address_line2": "Building 4th floor, 1st room",
    "address_zipcode": "0888283",
    "address_city": "New York",
    "address_country": "`country.uuid`",
    "address_state": "`state.uuid`"
  }
}
```

**Responses**:

- **Description**: Company updated successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${company.uuid}",
      "name": "Acme2",
      "is_customer": true,
      "is_manufacturer": false,
      "address_line1": "2nd Avenue, 4000 - Brooklin",
      "address_line2": "Building 4th floor, 1st room",
      "address_zipcode": "0888283",
      "address_city": "New York",
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-06-01T00:00:00Z",
      "address_country": {
        "uuid": "`country.uuid`",
        "name": "United States",
        "iso_code": "US"
      },
      "address_state": {
        "uuid": "`state.uuid`",
        "name": "Nova York",
        "iso_code": "NY"
      },
      "identifiers": [
        {
          "uuid": "`identifier.uuid`",
          "type": "EIN",
          "value": "123123123",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        },
        {
          "uuid": "`identifier.uuid`",
          "type": "CBP_ASSIGNED_NUMBER",
          "value": "321321321",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ],
      "contacts": [
        {
          "uuid": "`contact.uuid`",
          "is_principal": true,
          "name": "Stev Jobs",
          "email": "stev.jobs@acme.com",
          "phone": "+1999935323",
          "job_title": "Founder",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ]
    }
  }
  ```
## Search Companies
**Description**: Performs a paginated search for companies with optional filters and sorting criteria. \
**Method**: POST \
**Endpoint**: `/company/search`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "search": {
    "page": 1,
    "per_page": 15,
    "where": [],
    "order_by": []
  }
}
```

**Responses**:

- **Description**: List of companies returned successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": [
      {
        "uuid": "${company.uuid}",
        "name": "Acme",
        "is_customer": true,
        "is_manufacturer": false,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "address_country": {
          "uuid": "${country.uuid}",
          "name": "United States",
          "iso_code": "US"
        },
        "address_state": {
          "uuid": "${state.uuid}",
          "name": "Nova York",
          "iso_code": "NY"
        },
        "identifiers": [
          {
            "uuid": "${identifier.uuid}",
            "type": "EIN",
            "value": "123123123",
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z"
          },
          {
            "uuid": "${identifier.uuid}",
            "type": "CBP_ASSIGNED_NUMBER",
            "value": "321321321",
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z"
          }
        ],
        "principal_contact": {
          "uuid": "${contact.uuid}",
          "is_principal": true,
          "name": "Stev Jobs",
          "email": "stev.jobs@acme.com",
          "phone": "+1999935323",
          "job_title": "Founder",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        },
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      }
    ],
    "meta": {
      "page": 1,
      "per_page": 15,
      "total_rows": 1,
      "total_pages": 1
    }
  }
  ```
## Delete Company
**Description**: Deletes a company identified by its UUID. \
**Method**: DELETE \
**Endpoint**: `/company/${company.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Company deleted successfully \
  **HTTP Code**: 204

- **Description**: Company not found for the given UUID \
  **HTTP Code**: 404

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 2,
      "error": "Not Found",
      "detail": "Company not found with uuid: ${company.uuid}",
      "status_code": 404
    }
  }
  ```
## Add Contact to Company
**Description**: Adds a new contact to an existing company identified by its UUID. \
**Method**: POST \
**Endpoint**: `/company/${company.uuid}/contact`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "contact": {
    "name": "Emil Stephan",
    "is_principal": true,
    "email": "emil@gaiadynamis.ai",
    "phone": "+1234567",
    "job_title": "CEO"
  }
}
```

**Responses**:

- **Description**: Contact added successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${contact.uuid}",
      "is_principal": true,
      "name": "Emil Stephan",
      "email": "emil@gaiadynamis.ai",
      "phone": "+1234567",
      "job_title": "CEO",
      "created_at": "2025-06-03T15:00:00Z",
      "updated_at": "2025-06-03T15:00:00Z"
    }
  }
  ```



## Remove Contact from Company
**Description**: Removes a contact from a company using the company's and contact's UUIDs. \
**Method**: DELETE \
**Endpoint**: `/company/${company.uuid}/contact/${contact.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Contact removed successfully \
  **HTTP Code**: 204



## Set Contact as Principal
**Description**: Sets a specific contact as the principal contact for a company using their respective UUIDs. \
**Method**: PUT \
**Endpoint**: `/company/${company.uuid}/contact/${contact.uuid}/set-principal`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Contact successfully set as principal \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${contact.uuid}",
      "is_principal": true,
      "name": "Stev Jobs",
      "email": "stev.jobs@acme.com",
      "phone": "+1999935323",
      "job_title": "Founder",
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z"
    }
  }
  ```

## Add Identifier to Company
**Description**: Adds a new identifier to a company using the company's UUID. \
**Method**: POST \
**Endpoint**: `/company/${company.uuid}/identifier`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "identifier": {
    "type": "EIN",
    "value": "234234234"
  }
}
```

**Responses**:

- **Description**: Identifier added successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${identifier.uuid}",
      "type": "EIN",
      "value": "234234234",
      "created_at": "2025-06-03T15:00:00Z",
      "updated_at": "2025-06-03T15:00:00Z"
    }
  }
  ```

## Remove Identifier from Company
**Description**: Removes a specific identifier from a company using the company's and identifier's UUIDs. \
**Method**: DELETE \
**Endpoint**: `/company/${company.uuid}/identifier/${identifier.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Identifier removed successfully \
  **HTTP Code**: 204


## Set Contact as Principal
**Description**: Sets a contact as the principal for a company using the UUIDs of both the company and the contact. \
**Method**: PUT \
**Endpoint**: `/company/${company.uuid}/contact/${contact.uuid}/set-principal`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Contact successfully marked as principal \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${contact.uuid}",
      "is_principal": true,
      "name": "Stev Jobs",
      "email": "stev.jobs@acme.com",
      "phone": "+1999935323",
      "job_title": "Founder",
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z"
    }
  }
  ```

## Search Countries
**Description**: Performs a paginated search for countries with optional filters and sorting. \
**Method**: POST \
**Endpoint**: `/geo/country/search`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "search": {
    "page": 1,
    "per_page": 15,
    "where": [
      {
        "kind": "LR_LIKE",
        "field": "name",
        "query": "State"
      }
    ],
    "order_by": [
      {
        "field": "iso_code",
        "direction": "ASC"
      }
    ]
  }
}
```

**Responses**:

- **Description**: List of countries returned successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": [
      {
        "uuid": "${country.uuid}",
        "name": "United States",
        "iso_code": "US"
      }
    ],
    "meta": {
      "page": 1,
      "per_page": 15,
      "total_rows": 1,
      "total_pages": 1
    }
  }
  ```
## Search States
**Description**: Performs a paginated search for states with filtering and sorting options. Supports filtering by country attributes and state name. \
**Method**: POST \
**Endpoint**: `/geo/state/search`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "search": {
    "page": 1,
    "per_page": 15,
    "where": [
      {
        "kind": "EQUAL",
        "field": "country.iso_code",
        "query": "US",
        "or_": [
          {
            "kind": "LR_LIKE",
            "field": "name",
            "query": "York"
          },
          {
            "kind": "IS_NOT_NULL",
            "field": "name"
          }
        ]
      },
      {
        "kind": "IS_NOT_NULL",
        "field": "country.name"
      }
    ],
    "order_by": [
      {
        "field": "iso_code",
        "direction": "ASC"
      }
    ]
  }
}
```

**Responses**:

- **Description**: States matching filters returned successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": [
      {
        "uuid": "${state.uuid}",
        "name": "Nova York",
        "iso_code": "NY"
      }
    ],
    "meta": {
      "page": 1,
      "per_page": 15,
      "total_rows": 1,
      "total_pages": 1
    }
  }
  ```

## Create Product
**Description**: Creates a new product with associated customer, manufacturer, origin country, and optional tariff codes. \
**Method**: POST \
**Endpoint**: `/product`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "product": {
    "name": "T-shirt",
    "description": "Man's T-shirt G2 coton blue",
    "origin_country": "`country.uuid`",
    "customer": "`customer.uuid`"
  }
}
```

**Responses**:

- **Description**: Product created with basic details \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${product.uuid}",
      "name": "T-shirt",
      "description": "Man's T-shirt G2 coton blue",
      "reference_number": null,
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z",
      "origin_country": {
        "uuid": "${country.uuid}",
        "name": "United States",
        "iso_code": "US"
      },
      "customer": {
        "uuid": "${customer.uuid}",
        "name": "Acme",
        "is_customer": true,
        "is_manufacturer": false,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      },
      "manufacturer": null,
      "tariff_codes": [],
      "attachments": []
    }
  }
  ```

- **Description**: Product created with full details including reference number, manufacturer, and tariff codes \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${product.uuid}",
      "name": "T-shirt",
      "reference_number": "T01MGZ",
      "description": "Man's T-shirt G coton blue",
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z",
      "origin_country": {
        "uuid": "${country.uuid}",
        "name": "United States",
        "iso_code": "US"
      },
      "customer": {
        "uuid": "${customer.uuid}",
        "name": "Acme",
        "is_customer": true,
        "is_manufacturer": false,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      },
      "manufacturer": {
        "uuid": "${manufacturer.uuid}",
        "name": "Acme2",
        "is_customer": false,
        "is_manufacturer": true,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      },
      "tariff_codes": [
        {
          "uuid": "${tariff_code.uuid}",
          "type": "HTS",
          "code": "6105.10.0010",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ],
      "attachments": []
    }
  }
  ```
## View Product
**Description**: Retrieves the detailed information of a product by its UUID. \
**Method**: GET \
**Endpoint**: `/product/${product.uuid}`

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Responses**:

- **Description**: Product details returned successfully \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "${product.uuid}",
      "name": "T-shirt",
      "reference_number": "T01MGZ",
      "description": "Man's T-shirt G coton blue",
      "created_at": "2099-01-01T00:00:00Z",
      "updated_at": "2099-01-01T00:00:00Z",
      "origin_country": {
        "uuid": "${country.uuid}",
        "name": "United States",
        "iso_code": "US"
      },
      "customer": {
        "uuid": "${customer.uuid}",
        "name": "Acme",
        "is_customer": true,
        "is_manufacturer": false,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      },
      "manufacturer": {
        "uuid": "${manufacturer.uuid}",
        "name": "Acme",
        "is_customer": true,
        "is_manufacturer": false,
        "address_line1": "2nd Avenue, 3444 - Brooklin",
        "address_line2": null,
        "address_zipcode": "0888032",
        "address_city": "New York",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z"
      },
      "tariff_codes": [
        {
          "uuid": "${tariff_code.uuid}",
          "type": "HTS",
          "code": "6105.10.0010",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ],
      "attachments": [
        {
          "uuid": "${attachment.uuid}",
          "type": "other",
          "name": "image.png",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        }
      ]
    }
  }
  ```
## Search Product
**Description**: Search for products using filters like pagination and field values. \
**Method**: POST \
**Endpoint**: /product/search \

**QueryString**:
- arrival_date: Arrival date filter (e.g. 2025-01-01)
- include: Fields to include in the response (e.g. tariff_codes.detail,tariff_codes.configuration)

**Request Headers**:
- Content-Type: application/json
- Authorization: token

**Request Body Example**:

```json
{
  "search" : {
    "page": 1,
    "per_page": 15,
    "where": [],
    "order_by": []
  }
}
```

**Responses**:

- **Description**: Product search result with pagination and product data \
  **HTTP Code**: 200

  **Example Response Body**:
  ```json
  {
    "data": [
      {
        "uuid": "3bb27fd5-8c2c-4a0c-9ba1-6f912c278301",
        "name": "T-shirt",
        "reference_number": "T01MGZ",
        "description": "Man's T-shirt G coton blue",
        "created_at": "2099-01-01T00:00:00Z",
        "updated_at": "2099-01-01T00:00:00Z",
        "origin_country": {
          "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
          "name": "United States",
          "iso_code": "US"
        },
        "customer": {
          "uuid": "a7740ec5-4521-4649-a214-e69bca68f8f9",
          "name": "Acme",
          "is_customer": true,
          "is_manufacturer": false,
          "address_line1": "2nd Avenue, 3444 - Brooklin",
          "address_line2": null,
          "address_zipcode": "0888032",
          "address_city": "New York",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        },
        "manufacturer": {
          "uuid": "a7740ec5-4521-4649-a214-e69bca68f8f9",
          "name": "Acme",
          "is_customer": true,
          "is_manufacturer": false,
          "address_line1": "2nd Avenue, 3444 - Brooklin",
          "address_line2": null,
          "address_zipcode": "0888032",
          "address_city": "New York",
          "created_at": "2099-01-01T00:00:00Z",
          "updated_at": "2099-01-01T00:00:00Z"
        },
        "tariff_codes": [
          {
            "uuid": "04cbf588-af16-4b27-be3c-ad4cabbf8766",
            "type": "HTS",
            "code": "6105.10.0010",
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z"
          }
        ]
      }
    ],
    "meta": {
      "page": "<is_int>",
      "per_page": "<is_int>",
      "total_rows": "<is_int>",
      "total_pages": "<is_int>"
    }
  }
  ```
## Delete Product
**Description**: Delete a specific product by UUID. \
**Method**: DELETE \
**Endpoint**: /product/`${product.uuid}` \

**Request Headers**:
- Authorization: token

**Responses**:

- **Description**: Product deleted successfully \
  **HTTP Code**: 204

  **Example Response Body**:
  _No content returned._

## Get Product Classification Questions  
**Description**: Retrieve classification guidance and disambiguation questions for a given product based on its name and description.  
**Method**: POST  
**Endpoint**: `/product/classification/questions`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "input": {
    "name": "T-shirt",
    "description": "Man's T-shirt of 100% coton"
  }
}
```

**Responses**:

- **Description**: Classification questions and suggested tariff data returned successfully  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "suggested_description": "Men's T-shirt made of 100% cotton with zipper closure",
      "best_guess": {
        "6109": 0.75,
        "6205": 0.85
      },
      "disambiguation": {
        "Is the T-shirt knitted or crocheted?": [
          "Yes, it is knitted or crocheted",
          "No, it is woven"
        ],
        "What type of neckline does the T-shirt have?": [
          "Crew/round neck",
          "V-neck",
          "Other neckline"
        ],
        "What is the sleeve length?": [
          "Short sleeves",
          "Long sleeves"
        ]
      }
    }
  }
  ```

## Classify Product Tariff Code  
**Description**: Classifies a product based on detailed information, returning a tariff code, reasoning, confidence level, and applicable rulings.  
**Method**: POST  
**Endpoint**: `/product/classification/tariff-code`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "input": {
    "name": "T-shirt",
    "description": "Man's T-shirt of 100% coton",
    "improved_description": "Men's T-shirt made of 100% cotton with a zipper.",
    "initial_chat_result": {
      "suggested_description": "Men's T-shirt made of 100% cotton with zipper closure",
      "best_guess": {
        "6109": 0.75,
        "6205": 0.85
      },
      "disambiguation": {
        "Is the T-shirt knitted or crocheted?": [
          "Yes, it is knitted or crocheted",
          "No, it is woven (not knitted or crocheted)"
        ],
        "Where is the zipper located?": [
          "Full front zipper opening",
          "Partial front zipper opening",
          "Zipper at neck/collar only",
          "Other location"
        ],
        "What type of sleeves does it have?": [
          "Short sleeves",
          "Long sleeves",
          "Sleeveless"
        ],
        "What type of neckline does it have?": [
          "Crew/round neck",
          "V-neck",
          "Other neckline"
        ]
      }
    },
    "answers": {
      "Is the T-shirt knitted or crocheted?": "Yes, it is knitted or crocheted",
      "Where is the zipper located?": "Full front zipper opening",
      "What type of sleeves does it have?": "Short sleeves",
      "What type of neckline does it have?": "Crew/round neck"
    }
  }
}
```

**Responses**:

- **Description**: Tariff code classification result with rationale and supporting rulings  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "reasoning": [
        "The product is a men's knitted T-shirt made of 100% cotton with a full front zipper opening and short sleeves with crew/round neck.",
        "The presence of a full front zipper opening is a key distinguishing feature that makes this different from a standard T-shirt.",
        "While heading 6109 covers T-shirts, the full front zipper opening is not typical of garments in this heading, which generally have no fastenings.",
        "The garment more closely resembles sweaters, pullovers or similar articles with front openings in heading 6110.",
        "The knitted construction places it in Chapter 61 rather than Chapter 62 (which includes heading 6205)."
      ],
      "suggested_description": "Men's knitted T-shirt made of 100% cotton with full front zipper opening, short sleeves, and crew/round neckline",
      "best_guess_code": "6110.20.20.69",
      "confidence": "MEDIUM",
      "rulings": [
        {
          "ruling_number": "952530",
          "url": "https://rulings.cbp.gov/ruling/952530",
          "subject": "Women's knit garment; long, close fitting sleeves; classifiable as similar to a T-shirt; heading 6109, HTSUSA",
          "ruling_content": "facts : the pullover portion of style 1031757 is described as a t - shirt styled pullover, size medium, constructed of jersey fabric, with a rounded close - fitting neckline and narrow long sleeves. [...]",
          "hts_numbers": [
            "6109.10.00.70"
          ]
        },
        {
          "ruling_number": "954560",
          "url": "https://rulings.cbp.gov/ruling/954560",
          "subject": "Classification of women's upper body garments which do not reach the waist; proper heading is 6114, HTSUSA, as an other garment",
          "ruling_content": "facts : styles 5358 and 5458 are knit short sleeve tops constructed from 95 percent cotton and 5 percent spandex fabric. [...]",
          "hts_numbers": [
            "6114.20.00.10"
          ]
        },
        {
          "ruling_number": "m86915",
          "url": "https://rulings.cbp.gov/ruling/m86915",
          "subject": "The tariff classification of five women's garments from Guatemala.Dear Mr. Scharpf:",
          "ruling_content": "style 30701 is a woman ' s t - shirt constructed of lightweight 100 % cotton knit fabric. [...]",
          "hts_numbers": [
            "6106.10.00.10",
            "6109.10.00.40",
            "6109.10.00.70",
            "6110.20.20.79"
          ]
        }
      ],
      "reflection_result": null,
      "alternatives": [
        {
          "code": "6109.10.00.12",
          "condition": "If the zipper is considered a decorative element rather than a functional opening feature",
          "tariff_detail": {
            "description_heading": [
              "T-shirts, singlets, tank tops and similar garments, knitted or crocheted:",
              "Of cotton",
              "Men's or boys':",
              "Other T-shirts:",
              "Men's (338)"
            ],
            "description": "Men's (338)",
            "units": [
              "doz.",
              "kg"
            ],
            "rate_of_duty": {
              "general": "16.5%",
              "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
              "other": "90%"
            }
          }
        },
        {
          "code": "6114.20.00.10",
          "condition": "If considered a specialized knitted garment not covered by more specific headings",
          "tariff_detail": {
            "description_heading": [
              "Other garments, knitted or crocheted:",
              "Of cotton",
              "Tops:",
              "Women's or girls' (339)"
            ],
            "description": "Women's or girls' (339)",
            "units": [
              "doz.",
              "kg"
            ],
            "rate_of_duty": {
              "general": "10.8%",
              "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
              "other": "90%"
            }
          }
        }
      ],
      "tariff_detail": {
        "description_heading": [
          "Sweaters, pullovers, sweatshirts, waistcoats (vests) and similar articles, knitted or crocheted:",
          "Of cotton:",
          "Other",
          "Other:",
          "Other:",
          "Men's or boys':",
          "Other (338)"
        ],
        "description": "Other (338)",
        "units": [
          "doz.",
          "kg"
        ],
        "rate_of_duty": {
          "general": "16.5%",
          "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
          "other": "50%"
        }
      }
    }
  }
  ```

## Classify Product Tariff Code **autonomous-mode** 
**Description**: Classifies a product based on detailed information, returning a tariff code, reasoning, confidence level, and applicable rulings.  
**Method**: POST  
**Endpoint**: `/product/classification/tariff-code/autonomous`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "input": {
    "name": "T-shirt",
    "description": "Man's T-shirt of 100% coton"
  }
}
```

**Responses**:

- **Description**: Tariff code classification result with rationale and supporting rulings  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "reasoning": [
        "The product is a men's knitted T-shirt made of 100% cotton with a full front zipper opening and short sleeves with crew/round neck.",
        "The presence of a full front zipper opening is a key distinguishing feature that makes this different from a standard T-shirt.",
        "While heading 6109 covers T-shirts, the full front zipper opening is not typical of garments in this heading, which generally have no fastenings.",
        "The garment more closely resembles sweaters, pullovers or similar articles with front openings in heading 6110.",
        "The knitted construction places it in Chapter 61 rather than Chapter 62 (which includes heading 6205)."
      ],
      "suggested_description": "Men's knitted T-shirt made of 100% cotton with full front zipper opening, short sleeves, and crew/round neckline",
      "best_guess_code": "6110.20.20.69",
      "confidence": "MEDIUM",
      "rulings": [
        {
          "ruling_number": "952530",
          "url": "https://rulings.cbp.gov/ruling/952530",
          "subject": "Women's knit garment; long, close fitting sleeves; classifiable as similar to a T-shirt; heading 6109, HTSUSA",
          "ruling_content": "facts : the pullover portion of style 1031757 is described as a t - shirt styled pullover, size medium, constructed of jersey fabric, with a rounded close - fitting neckline and narrow long sleeves. [...]",
          "hts_numbers": [
            "6109.10.00.70"
          ]
        },
        {
          "ruling_number": "954560",
          "url": "https://rulings.cbp.gov/ruling/954560",
          "subject": "Classification of women's upper body garments which do not reach the waist; proper heading is 6114, HTSUSA, as an other garment",
          "ruling_content": "facts : styles 5358 and 5458 are knit short sleeve tops constructed from 95 percent cotton and 5 percent spandex fabric. [...]",
          "hts_numbers": [
            "6114.20.00.10"
          ]
        },
        {
          "ruling_number": "m86915",
          "url": "https://rulings.cbp.gov/ruling/m86915",
          "subject": "The tariff classification of five women's garments from Guatemala.Dear Mr. Scharpf:",
          "ruling_content": "style 30701 is a woman ' s t - shirt constructed of lightweight 100 % cotton knit fabric. [...]",
          "hts_numbers": [
            "6106.10.00.10",
            "6109.10.00.40",
            "6109.10.00.70",
            "6110.20.20.79"
          ]
        }
      ],
      "reflection_result": null,
      "alternatives": [
        {
          "code": "6109.10.00.12",
          "condition": "If the zipper is considered a decorative element rather than a functional opening feature",
          "tariff_detail": {
            "description_heading": [
              "T-shirts, singlets, tank tops and similar garments, knitted or crocheted:",
              "Of cotton",
              "Men's or boys':",
              "Other T-shirts:",
              "Men's (338)"
            ],
            "description": "Men's (338)",
            "units": [
              "doz.",
              "kg"
            ],
            "rate_of_duty": {
              "general": "16.5%",
              "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
              "other": "90%"
            }
          }
        },
        {
          "code": "6114.20.00.10",
          "condition": "If considered a specialized knitted garment not covered by more specific headings",
          "tariff_detail": {
            "description_heading": [
              "Other garments, knitted or crocheted:",
              "Of cotton",
              "Tops:",
              "Women's or girls' (339)"
            ],
            "description": "Women's or girls' (339)",
            "units": [
              "doz.",
              "kg"
            ],
            "rate_of_duty": {
              "general": "10.8%",
              "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
              "other": "90%"
            }
          }
        }
      ],
      "tariff_detail": {
        "description_heading": [
          "Sweaters, pullovers, sweatshirts, waistcoats (vests) and similar articles, knitted or crocheted:",
          "Of cotton:",
          "Other",
          "Other:",
          "Other:",
          "Men's or boys':",
          "Other (338)"
        ],
        "description": "Other (338)",
        "units": [
          "doz.",
          "kg"
        ],
        "rate_of_duty": {
          "general": "16.5%",
          "special": "Free (AU,BH,CL,CO,IL,JO,KR,MA,OM,P,PA,PE,S,SG)",
          "other": "50%"
        }
      }
    }
  }
  ```

## Upload Product Bulk File  
**Description**: Upload a CSV file containing product data in bulk for processing.  
**Method**: POST  
**Endpoint**: `/product/bulk/upload`  

**Request Headers**:
- Authorization: token  

**Request Body Example** (multipart/form-data):
```json
[file: product-bulk-upload.csv]
```

**Responses**:

- **Description**: File uploaded and processed successfully  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "bulk_process_uuid": "4c3b2e1a-719d-4cc7-baf6-f92b375fdd3e",
      "total_registers": 1,
      "total_processed": 1,
      "total_unprocessed": 0
    }
  }
  ```

- **Description**: File uploaded but contains validation errors  
  **HTTP Code**: 400  

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 1,
      "error": "Invalid Input",
      "detail": "Uploaded file contain errors",
      "status_code": 400
    },
    "meta": {
      "validation_errors": [
        {
          "error_type": "missing_required_column",
          "column_name": "hts"
        },
        {
          "error_type": "empty_row",
          "row_number": 1
        },
        {
          "error_type": "duplicate_row",
          "row_number": 4
        }
      ]
    }
  }
  ```
## Optimize Product Description  
**Description**: Enhances and standardizes a product description using AI-based optimization.  
**Method**: POST  
**Endpoint**: `/product/optimize-description`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "product": {
    "description": "Man's T-shirt G2 coton blue"
  }
}
```

**Responses**:

- **Description**: Description optimized successfully  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "optimized_description": "Men's T-shirt, of cotton, colored (blue)."
    }
  }
  ```

- **Description**: Optimization could not generate a better result (response is empty)  
  **HTTP Code**: 422  

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 17,
      "error": "Unprocessable Entity",
      "detail": "Can not process the request",
      "status_code": 422
    },
    "meta": {
      "reason": "response_empty_after_processing"
    }
  }
  ```

- **Description**: Optimization output is the same as input, thus rejected  
  **HTTP Code**: 422  

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": 17,
      "error": "Unprocessable Entity",
      "detail": "Can not process the request",
      "status_code": 422
    },
    "meta": {
      "reason": "output_identical_to_input"
    }
  }
  ```

## Remove Product Tariff Code  
**Description**: Removes an existing tariff code from a specific product.  
**Method**: DELETE  
**Endpoint**: `/product/${product.uuid}/tariff-code/${tariff_code.uuid}`  

**Parameters**:
- `product.uuid`: UUID of the product (e.g. `3bb27fd5-8c2c-4a0c-9ba1-6f912c278301`)  
- `tariff_code.uuid`: UUID of the tariff code to remove (e.g. `04cbf588-af16-4b27-be3c-ad4cabbf8766`)  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
_N/A (DELETE request with no body)_

**Responses**:

- **Description**: Tariff code successfully removed  
  **HTTP Code**: 204  

---

## Create Product Tariff Code  
**Description**: Adds a new tariff code to a specific product.  
**Method**: POST  
**Endpoint**: `/product/${product.uuid}/tariff-code`  

**Parameters**:
- `product.uuid`: UUID of the product (e.g. `3bb27fd5-8c2c-4a0c-9ba1-6f912c278301`)  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "tariff_code": {
    "type": "HTS",
    "code": "6105.10.0010"
  }
}
```

**Responses**:

- **Description**: Tariff code successfully created  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "df8e8c0d-32f4-4ed6-b9f0-8c028de62f7e",
      "type": "HTS",
      "code": "6105.10.0010",
      "created_at": "2025-06-03T14:12:30.481Z",
      "updated_at": "2025-06-03T14:12:30.481Z"
    }
  }
  ```


## Upload Product Attachment  
**Description**: Uploads an attachment file (e.g., image or document) and associates it with a specific product.  
**Method**: POST  
**Endpoint**: `/product/${product.uuid}/attachment/other`  

**Parameters**:
- `product.uuid`: UUID of the product to which the attachment will be linked (e.g. `3bb27fd5-8c2c-4a0c-9ba1-6f912c278301`)  

**Request Headers**:
- Authorization: token  

**Request Body Example** (multipart/form-data):
```json
[file: image.png]
```

**Responses**:

- **Description**: File uploaded successfully and attached to the product  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "uuid": "5b3ef90e-fd3a-44d6-a05d-2cd5fa982017",
      "type": "other",
      "name": "image.png",
      "created_at": "2025-06-03T14:17:20.202Z",
      "updated_at": "2025-06-03T14:17:20.202Z"
    }
  }
  ```

## Delete Product Attachment  
**Description**: Deletes a specific attachment associated with a product.  
**Method**: DELETE  
**Endpoint**: `/product/${product.uuid}/attachment/${attachment.uuid}`  

**Parameters**:
- `product.uuid`: UUID of the product (e.g. `3bb27fd5-8c2c-4a0c-9ba1-6f912c278301`)  
- `attachment.uuid`: UUID of the attachment to be deleted (e.g. `7abaafad-2b20-4a65-81a9-1c7c27d0f705`)  

**Request Headers**:
- Authorization: token  

**Request Body Example**:
_N/A (DELETE request with no body)_

**Responses**:

- **Description**: Attachment successfully deleted  
  **HTTP Code**: 204  


## Search Products by Tariff Conditions  
**Description**: Searches for products and materializes tariff data based on specific filter conditions.  
**Method**: POST  
**Endpoint**: `/product/tariff/materialize`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "condition": {
    "where": [
      {
        "kind": "EQUAL",
        "field": "bulk_process_uuid",
        "query": "df4d7658-9e91-4a35-b519-2921552633ed"
      }
    ]
  }
}
```

**Responses**:

- **Description**: Product tariff data successfully materialized and retrieved  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "total_registers": 1,
      "total_processed": 1,
      "total_unprocessed": 0
    }
  }
  ```
## View Tariff Detail  
**Description**: Retrieves detailed tariff information for a specific HTS code in a given country, including base and additional tariffs, scenarios, and optional flags.  
**Method**: GET  
**Endpoint**: `/product/tariff-detail/hts/${code}/${country}?include=pga_flags,remedy_flags`  

**Parameters**:
- `code`: HTS code to retrieve detail for (e.g. `9105.99.10.00`)  
- `country`: Target country code (e.g. `US`)  

**QueryString**:
- `include`: Comma-separated related data to include (e.g. `pga_flags,remedy_flags`)  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
_N/A (GET request with query parameters)_

**Responses**:

- **Description**: Tariff detail successfully retrieved  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "code": "9105.99.10.00",
      "description": "Standard marine chronometers having spring-detent escapements",
      "description_heading": [
        "Other clocks:",
        "Other:",
        "Other:",
        "Standard marine chronometers having spring-detent escapements"
      ],
      "units": [
        "No."
      ],
      "created_at": "2025-06-03T14:25:00Z",
      "tariff_base": [
        {
          "description": "Tariff 1",
          "rules": [
            {
              "kind": "each",
              "value": "4.50",
              "unit": "$",
              "by": "1",
              "on": null
            },
            {
              "kind": "total",
              "value": "65",
              "unit": "%",
              "by": "1",
              "on": null
            },
            {
              "kind": "per unit",
              "value": "25",
              "unit": "¢",
              "by": "jwel",
              "on": null
            }
          ]
        },
        {
          "description": "Tariff 2",
          "rules": [
            {
              "kind": "total",
              "value": "0",
              "unit": "%",
              "by": null,
              "on": null
            }
          ]
        }
      ],
      "tariff_additional": [
        {
          "description": "Tariff 3",
          "rules": [
            {
              "kind": "each",
              "value": "4.50",
              "unit": "$",
              "by": "1",
              "on": null
            },
            {
              "kind": "total",
              "value": "65",
              "unit": "%",
              "by": "1",
              "on": null
            },
            {
              "kind": "per unit",
              "value": "25",
              "unit": "¢",
              "by": "jwel",
              "on": null
            }
          ]
        }
      ],
      "tariff_scenario": [
        {
          "uuid": "59923d4c-7965-42b2-8114-d067fdf60ddb",
          "affected_country": "US",
          "tariff_code_pattern": "9105.99.10",
          "type": 0,
          "type_name": "Percentage",
          "value": 25.0,
          "tariff": {
            "uuid": "091de288-51a2-4756-8dd1-11de8a9e4df0",
            "title": "First Tariff Scenario",
            "description": "First Tariff Scenario description",
            "source": "USA 083 Federal Rule",
            "source_url": "https://www.site.com/#123",
            "category": 0,
            "category_name": "Official",
            "is_approved": true,
            "is_additional": false,
            "is_upcoming": true,
            "is_rumored": false,
            "initial_effective_date": "2099-01-01T00:00:00Z"
          }
        }
      ],
      "pga_flags": [],
      "remedy_flags": {
        "possible_add_required_indicator": false,
        "possible_cvd_duty_required_indicator": false
      }
    }
  }
  ```


## Update Tariff Configuration  
**Description**: Updates the tariff configuration for a specific product's tariff code, including base, additional, upcoming, and rumored tariffs.  
**Method**: PUT  
**Endpoint**: `/product/${product.uuid}/tariff-code/${tariff_code.uuid}/configuration`  

**Parameters**:
- `product.uuid`: UUID of the product (e.g. `3bb27fd5-8c2c-4a0c-9ba1-6f912c278301`)  
- `tariff_code.uuid`: UUID of the tariff code to configure (e.g. `04cbf588-af16-4b27-be3c-ad4cabbf8766`)  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "configuration": {
    "base_tariff": "0",
    "additional_tariffs": "0,1",
    "upcoming_tariffs": "514ef222-eb9c-4730-ba47-b27545c5cf29",
    "rumored_tariffs": "b483f029-74b0-40ce-af25-ea51e94aa11c,0f2bd1cf-8011-44b9-a897-1237b5122b50"
  }
}
```

**Responses**:

- **Description**: Tariff configuration updated successfully  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  {
    "data": {
      "base_tariff": "0",
      "additional_tariffs": "0,1",
      "upcoming_tariffs": "514ef222-eb9c-4730-ba47-b27545c5cf29",
      "rumored_tariffs": "b483f029-74b0-40ce-af25-ea51e94aa11c,0f2bd1cf-8011-44b9-a897-1237b5122b50"
    }
  }
  ```

## Download Product CSV  
**Description**: Downloads a CSV file containing product data and tariff information filtered by specified conditions.  
**Method**: POST  
**Endpoint**: `/product/csv-download`  

**Request Headers**:
- Content-Type: application/json  
- Authorization: token  

**Request Body Example**:
```json
{
  "condition": {
    "where": [
      {
        "kind": "EQUAL",
        "field": "bulk_process_uuid",
        "query": "df4d7658-9e91-4a35-b519-2921552633ed"
      }
    ]
  }
}
```

**Responses**:

- **Description**: CSV generated and returned successfully  
  **HTTP Code**: 200  

  **Example Response Body**:
  ```json
  product_name;product_description;hts_code;origin_country;base_tariff_rate;additional_tariff;upcoming_tariff;rumored_tariff;total_effective_tariff;antidumping_duty;countervailing_duty;pga_flags
  "T-shirt";"Man's T-shirt G coton blue";"6105.10.0010";"United States";"94.5 USD";"0 ";"0 ";"0 ";"N/A";"FALSE";"FALSE";"PGA FLAG CODE 1"
  ```

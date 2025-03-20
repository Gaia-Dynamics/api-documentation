# API

## Edit Account
  **Description**: Allows an authenticated user to edit account details \
  **Method**: PUT \
  **Endpoint**: `/account`

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Request Body Example**:

  ```json
  {
      "account": {
          "company_name": "Gaia Dynamics Inc."
      }
  }
  ```

  **Responses**:

  - **Description**: Successfully updated account details \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":{
            "uuid":"2e138e20-7642-4615-992b-8653b44f6430",
            "type":0,
            "company_name": "Gaia Dynamics Inc.",
            "available_credits": 0
        }
    }
    ```

  - **Description**: Successfully updated account billing information \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":{
            "uuid":"2e138e20-7642-4615-992b-8653b44f6430",
            "type":0,
            "company_name": "Gaia Dynamics Inc.",
            "available_credits": 100,
            "billing_information": {
                "uuid": "%.*%",
                "first_name": "John",
                "last_name": "Doe",
                "email": "john.doe@example.com",
                "phone": "+15555555555",
                "line1": "Main Street, 123",
                "line2": "Apt 1",
                "line3": "",
                "city": "New York",
                "country":
                {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                    "name": "United States",
                    "iso_code": "US"
                },
                "state":
                {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                    "name": "New York",
                    "iso_code": "NY"
                }
            }
        }
    }
    ```

  - **Description**: Successfully updated account type \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":
        {
            "uuid": "08f67e41-c61e-4c71-ad4e-971129d4e4d2",
            "type": 1,
            "company_name": "Acme Corp.",
            "available_credits": 100,
            "billing_information": null
        }
    }
    ```

  - **Description**: Unauthorized attempt to edit account type \
    **HTTP Code**: 403 \
    **Example Response Body**:
    ```json
    {
        "data":
        {
            "code": 12,
            "error": "Forbidden",
            "detail": "Account type cannot be changed",
            "status_code": 403
        }
    }
    ```

## View Account
  **Description**: Allows an authenticated user to retrieve their account information \
  **Method**: GET \
  **Endpoint**: `/account`

  **Request Headers**:
  -   Content-Type: application/json
  -   Authorization: token

  **Request Body Example**:
  *(No request body required)*

  **Responses**:

  - **Description**: Successfully retrieved account information \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":
        {
            "uuid": "2e138e20-7642-4615-992b-8653b44f6430",
            "type": 0,
            "company_name": "Acme Corp.",
            "billing_information": null,
            "available_credits": 100
        }
    }
    ```

## Create/Add User (invite user)
  **Description**: Allows an authenticated user to create a new user \
  **Method**: POST \
  **Endpoint**: `/account/user`

  **Request Headers**:
  -   Content-Type: application/json
  -   Authorization: token

  **Request Body Example**:

  ```json
  {
      "user": {
          "email": "rachel.doe@example.com",
          "role": 1
      }
  }
  ```

  **Responses**:

  - **Description**: Successfully created a new user \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":{
            "uuid":"%.*%",
            "first_name":Rachel,
            "last_name":Doe,
            "email":"rachel.doe@example.com",
            "job_title": null,
            "last_login_at": null,
            "account":{
                "uuid":"2e138e20-7642-4615-992b-8653b44f6430",
                "type":0,
                "company_name": "Acme Corp.",
                "available_credits": 100
            }
        }
    }
    ```

## Resend Invite
  **Description**: Resend an invitation as an unauthenticated user \
  **Method**: GET \
  **Endpoint**: `/account/user/resend-invite/${user.uuid}`

  **Parameters**:
  - user.uuid: The unique identifier of the user for whom the invitation is being resent (i.e 87080b37-89a6-4503-882e-70f1410a8fb4)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Invitation successfully resent \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Search Users
  **Description**: Allows an authenticated user to search for users \
  **Method**: POST \
  **Endpoint**: `/account/user/search`

  **Request Headers**:
  -   Content-Type: application/json
  -   Authorization: token

  **Request Body Example**:

  ```json
  {
    "search" : {
      "page": 1,
      "per_page": 10,
      "where": [{
        "kind": "IS_NULL",
        "field": "deactivated_at"
      }],
      "order_by": []
    }
  }
  ```

  **Responses**:

  - **Description**: Successfully retrieved user search results \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":
        [
            {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "first_name": "Rachel",
                "last_name": "Doe",
                "job_title": "Analyst",
                "email": "rachel.doe@example.com",
                "email_verified": false,
                "role": 0,
                "role_name": "ROLE_ADMIN",
                "last_login_at": "%[0-9]{4,4}-[0-9]{2,2}-[0-9]{2,2}T[0-9]{2,2}:[0-9]{2,2}:[0-9]{2,2}Z%",
                "created_at": "2099-01-01T00:00:00Z",
                "updated_at": "2099-01-01T00:00:00Z",
                "deactivated_at": null,
                "account":
                {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6430",
                    "type": 0,
                    "company_name": "Acme Corp.",
                    "available_credits": 100
                }
            },
            {
                "uuid": "87080b37-89a6-4503-882e-70f1410a8fb4",
                "first_name": "John",
                "last_name": "Doe",
                "job_title": "Executive",
                "email": "john.doe@example.com",
                "email_verified": false,
                "role": 1,
                "role_name": "ROLE_MEMBER",
                "last_login_at": null,
                "created_at": "2099-01-01T00:00:00Z",
                "updated_at": "2099-01-01T00:00:00Z",
                "deactivated_at": null,
                "account":
                {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6430",
                    "type": 0,
                    "company_name": "Acme Corp.",
                    "available_credits": 0
                }
            }
        ],
        "meta":
        {
            "page": 1,
            "per_page": 10,
            "total_rows": 2,
            "total_pages": 1
        }
    }
    ```

## Edit User
  **Description**: Edit user information as an authenticated user \
  **Method**: PUT \
  **Endpoint**: `/account/user/${user.uuid}`

  **Parameters**:
  - user.uuid: The unique identifier of the user to be edited (i.e 87080b37-89a6-4503-882e-70f1410a8fb4)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Request Body Example**:

  ```json
  {
      "user": {
          "first_name": "John",
          "last_name": "Doe",
          "job_title": "Executive",
          "password": "SecretPassword",
          "role": 0
      }
  }
  ```

## Deactivate User
  **Description**: Deactivate a user as an authenticated user \
  **Method**: DELETE \
  **Endpoint**: `/account/user/${user.uuid}`

  **Parameters**:
  - user.uuid: The unique identifier of the user to be deactivated (i.e 87080b37-89a6-4503-882e-70f1410a8fb4)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: User successfully deactivated \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Forgot Password
  **Description**: Allows an unauthenticated user to request a password reset email \
  **Method**: POST \
  **Endpoint**: `/account/forgot-password`

  **Request Headers**:
  -   Content-Type: application/json

  **Request Body Example**:

  ```json
  {
      "forgot_password": {
          "email": "john.doe@example.com"
      }
  }
  ```

  **Responses**:

  - **Description**: Password reset request successfully processed \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No response body expected)*

## Get User
  **Description**: Allows an authenticated user to retrieve their user information \
  **Method**: GET \
  **Endpoint**: `/account/user`

  **Request Headers**:
  -   Content-Type: application/json
  -   Authorization: token

  **Request Body Example**:
  *(No request body required)*

  **Responses**:

  - **Description**: Successfully retrieved user information \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data":{
            "uuid":"2e138e20-7642-4615-992b-8653b44f6431",
            "first_name":"John",
            "last_name":"Doe",
            "job_title": "Executive",
            "email":"john.doe@example.com",
            "email_verified": false,
            "last_login_at": "%[0-9]{4,4}-[0-9]{2,2}-[0-9]{2,2}T[0-9]{2,2}:[0-9]{2,2}:[0-9]{2,2}Z%",
            "account":{
                "uuid":"2e138e20-7642-4615-992b-8653b44f6430",
                "type":0,
                "company_name": "Acme Corp.",
                "available_credits": 0
            }
        }
    }
    ```

## Create Company
  **Description**: Create a company as an authenticated user \
  **Method**: POST \
  **Endpoint**: `/company` \

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
          "address_line2": "4th floor, Room 1",
          "address_zipcode": "0888283",
          "address_city": "New York",
          "address_country": "2e138e20-7642-4615-992b-8653b44f6431",
          "address_state": "2e138e20-7642-4615-992b-8653b44f6431",
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
                  "name": "John Doe",
                  "is_principal": true,
                  "email": "john.doe@example.com",
                  "phone": "+15555555555",
                  "job_title": "Executive"
              },
              {
                  "name": "Rachel Doe",
                  "is_principal": false,
                  "email": "rachel.doe@example.com",
                  "phone": "+15555555555",
                  "job_title": "Analyst"
              }
          ]
      }
  }
  ```

  **Responses**:

  - **Description**: Company successfully created \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "generated-uuid",
            "name": "Acme",
            "is_customer": true,
            "is_manufacturer": false,
            "address_line1": "2nd Avenue, 4000 - Brooklin",
            "address_line2": "4th floor, Room 1",
            "address_zipcode": "0888283",
            "address_city": "New York",
            "created_at": "timestamp",
            "updated_at": "timestamp",
            "address_country": {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "United States",
                "iso_code": "US"
            },
            "address_state": {
                "uuid": "generated-uuid",
                "name": "New York",
                "iso_code": "NY"
            },
            "identifiers": [
                {
                    "uuid": "generated-uuid",
                    "type": "EIN",
                    "value": "234234234",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                },
                {
                    "uuid": "generated-uuid",
                    "type": "SSN",
                    "value": "345345345",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                },
                {
                    "uuid": "generated-uuid",
                    "type": "CBP_ASSIGNED_NUMBER",
                    "value": "432432432",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                }
            ],
            "contacts": [
                {
                    "uuid": "generated-uuid",
                    "is_principal": true,
                    "name": "John Doe",
                    "email": "john.doe@example.com",
                    "phone": "+15555555555",
                    "job_title": "Executive",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                },
                {
                    "uuid": "generated-uuid",
                    "is_principal": false,
                    "name": "Rachel Doe",
                    "email": "rachel.doe@example.com",
                    "phone": "+15555555555",
                    "job_title": "Analyst",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                }
            ]
        }
    }
    ```

  - **Description**: Identifier already taken \
    **HTTP Code**: 400 \
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

  - **Description**: Identifier type duplication \
    **HTTP Code**: 400 \
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

  - **Description**: Contact email duplication \
    **HTTP Code**: 400 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 7,
            "error": "Contact Email Duplication",
            "detail": "Contact email rachel.doe@example.com is duplicated",
            "status_code": 400
        }
    }
    ```

  - **Description**: Contact already taken \
    **HTTP Code**: 400 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 6,
            "error": "Contact Already Taken",
            "detail": "Contact email::rachel.doe@example.com already taken",
            "status_code": 400
        }
    }
    ```

  - **Description**: Main Contact duplication \
    **HTTP Code**: 400 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 8,
            "error": "Main Contact Duplication",
            "detail": "Main Contact Duplication",
            "status_code": 400
        }
    }
    ```

  - **Description**: Missing company type \
    **HTTP Code**: 400 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 9,
            "error": "Missing Company Type",
            "detail": "Company must have at least one type (customer, manufacturer, consignee, record_importer)",
            "status_code": 400
        }
    }
    ```

## Delete Company
  **Description**: Delete a company as an authenticated user \
  **Method**: DELETE \
  **Endpoint**: `/company/${company.uuid}`

  **Parameters**:
  - company.uuid: The unique identifier of the company to be deleted (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Company successfully deleted \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

  - **Description**: Company not found \
    **HTTP Code**: 404 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 2,
            "error": "Not Found",
            "detail": "Company not found with uuid: 08b4e62b-c974-4196-9f23-11216a653816",
            "status_code": 404
        }
    }
    ```

## Update Company
  **Description**: Update a company's information as an authenticated user \
  **Method**: PUT \
  **Endpoint**: `/company/${company.uuid}`

  **Parameters**:
  - company.uuid: The unique identifier of the company to be updated (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)

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
          "address_line2": "4th floor, Room 1",
          "address_zipcode": "0888283",
          "address_city": "New York",
          "address_country": "2e138e20-7642-4615-992b-8653b44f6431",
          "address_state": "2e138e20-7642-4615-992b-8653b44f6431"
      }
  }
  ```

  **Responses**:

  - **Description**: Company successfully updated \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "a7740ec5-4521-4649-a214-e69bca68f8f9",
            "name": "Acme",
            "is_customer": true,
            "is_manufacturer": false,
            "address_line1": "2nd Avenue, 4000 - Brooklin",
            "address_line2": "4th floor, Room 1",
            "address_zipcode": "0888283",
            "address_city": "New York",
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z",
            "address_country": {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "United States",
                "iso_code": "US"
            },
            "address_state": {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "New York",
                "iso_code": "NY"
            },
            "identifiers": [
                {
                    "uuid": "d18c4ac5-851d-461b-bcad-92ab0de5c13e",
                    "type": "EIN",
                    "value": "123123123",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                },
                {
                    "uuid": "03d9bb88-75ca-4f64-b920-e8b365d3a040",
                    "type": "CBP_ASSIGNED_NUMBER",
                    "value": "321321321",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ],
            "contacts": [
                {
                    "uuid": "ac6e6bac-7381-4252-8712-b492feeec419",
                    "is_principal": true,
                    "name": "John Doe",
                    "email": "john.doe@example.com",
                    "phone": "+15555555555",
                    "job_title": "Executive",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ]
        }
    }
    ```

## View Company
  **Description**: Retrieve a company's details as an authenticated user \
  **Method**: GET \
  **Endpoint**: `/company/${company.uuid}`

  **Parameters**:
  - company.uuid: The unique identifier of the company to be retrieved (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Company details successfully retrieved \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "a7740ec5-4521-4649-a214-e69bca68f8f9",
            "name": "Acme",
            "is_customer": true,
            "is_manufacturer": false,
            "address_line1": "2nd Avenue, 3444 - Brooklin",
            "address_line2": null,
            "address_zipcode": "0888032",
            "address_city": "New York",
            "address_country": {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "United States",
                "iso_code": "US"
            },
            "address_state": {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "New York",
                "iso_code": "NY"
            },
            "identifiers": [
                {
                    "uuid": "d18c4ac5-851d-461b-bcad-92ab0de5c13e",
                    "type": "EIN",
                    "value": "123123123",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                },
                {
                    "uuid": "03d9bb88-75ca-4f64-b920-e8b365d3a040",
                    "type": "CBP_ASSIGNED_NUMBER",
                    "value": "321321321",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ],
            "contacts": [
                {
                    "uuid": "ac6e6bac-7381-4252-8712-b492feeec419",
                    "is_principal": true,
                    "name": "John Doe",
                    "email": "john.doe@example.com",
                    "phone": "+15555555555",
                    "job_title": "Executive",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ],
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z"
        }
    }
    ```

  - **Description**: Company not found \
    **HTTP Code**: 404 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "code": 2,
            "error": "Not Found",
            "detail": "Company not found with uuid: 08b4e62b-c974-4196-9f23-11216a653816",
            "status_code": 404
        }
    }
    ```

## Search Company
  **Description**: Search for companies as an authenticated user \
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

  - **Description**: List of companies retrieved successfully \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "a7740ec5-4521-4649-a214-e69bca68f8f9",
                "name": "Acme",
                "is_customer": true,
                "is_manufacturer": false,
                "address_line1": "2nd Avenue, 3444 - Brooklin",
                "address_line2": null,
                "address_zipcode": "0888032",
                "address_city": "New York",
                "address_country": {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                    "name": "United States",
                    "iso_code": "US"
                },
                "address_state": {
                    "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                    "name": "New York",
                    "iso_code": "NY"
                },
                "identifiers": [
                    {
                        "uuid": "d18c4ac5-851d-461b-bcad-92ab0de5c13e",
                        "type": "EIN",
                        "value": "123123123",
                        "created_at": "timestamp",
                        "updated_at": "timestamp"
                    },
                    {
                        "uuid": "03d9bb88-75ca-4f64-b920-e8b365d3a040",
                        "type": "CBP_ASSIGNED_NUMBER",
                        "value": "321321321",
                        "created_at": "timestamp",
                        "updated_at": "timestamp"
                    }
                ],
                "principal_contact": {
                    "uuid": "ac6e6bac-7381-4252-8712-b492feeec419",
                    "is_principal": true,
                    "name": "John Doe",
                    "email": "john.doe@example.com",
                    "phone": "+15555555555",
                    "job_title": "Executive",
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
            "total_rows": 100,
            "total_pages": 7
        }
    }
    ```

## Add Contact
  **Description**: Add a contact to a company as an authenticated user \
  **Method**: POST \
  **Endpoint**: `/company/${company.uuid}/contact`

  **Parameters**:
  - company.uuid: The unique identifier of the company to which the contact will be added (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Request Body Example**:

  ```json
  {
      "contact": {
          "name": "Rachel Doe",
          "is_principal": true,
          "email": "rachel.doe@example.com",
          "phone": "+15555555555",
          "job_title": "Analyst"
      }
  }
  ```

  **Responses**:

  - **Description**: Contact successfully added \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "generated-uuid",
            "is_principal": true,
            "name": "Rachel Doe",
            "email": "rachel.doe@example.com",
            "phone": "+15555555555",
            "job_title": "Analyst",
            "created_at": "timestamp",
            "updated_at": "timestamp"
        }
    }
    ```

## Remove Contact
  **Description**: Remove a contact from a company as an authenticated user \
  **Method**: DELETE \
  **Endpoint**: `/company/${company.uuid}/contact/${contact.uuid}`

  **Parameters**:
  - company.uuid: The unique identifier of the company from which the contact will be removed (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)
  - contact.uuid: The unique identifier of the contact to be removed (i.e ac6e6bac-7381-4252-8712-b492feeec419)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Contact successfully removed \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Set Contact as Principal
  **Description**: Set a contact as the principal contact for a company as an authenticated user \
  **Method**: PUT \
  **Endpoint**: `/company/${company.uuid}/contact/${contact.uuid}/set-principal`

  **Parameters**:
  - company.uuid: The unique identifier of the company where the contact is being updated (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)
  - contact.uuid: The unique identifier of the contact to be set as principal (i.e ac6e6bac-7381-4252-8712-b492feeec419)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Contact successfully set as principal \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "ac6e6bac-7381-4252-8712-b492feeec419",
            "is_principal": true,
            "name": "Rachel Doe",
            "email": "rachel.doe@example.com",
            "phone": "+15555555555",
            "job_title": "Analyst",
            "created_at": "2099-01-01T00:00:00Z",
            "updated_at": "2099-01-01T00:00:00Z"
        }
    }
    ```

## Add Identifier
  **Description**: Add an identifier to a company as an authenticated user \
  **Method**: POST \
  **Endpoint**: `/company/${company.uuid}/identifier`

  **Parameters**:
  - company.uuid: The unique identifier of the company to which the identifier will be added (i.e 18c17a19-1051-47e5-ba86-7109797305f9)

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

  - **Description**: Identifier successfully added \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "generated-uuid",
            "type": "EIN",
            "value": "234234234",
            "created_at": "timestamp",
            "updated_at": "timestamp"
        }
    }
    ```

## Remove Identifier
  **Description**: Remove an identifier from a company as an authenticated user \
  **Method**: DELETE \
  **Endpoint**: `/company/${company.uuid}/identifier/${identifier.uuid}`

  **Parameters**:
  - company.uuid: The unique identifier of the company from which the identifier will be removed (i.e a7740ec5-4521-4649-a214-e69bca68f8f9)
  - identifier.uuid: The unique identifier of the identifier to be removed (i.e d18c4ac5-851d-461b-bcad-92ab0de5c13e)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Identifier successfully removed \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Search Country
  **Description**: Search for countries as an authenticated user \
  **Method**: POST \
  **Endpoint**: `/geo/country/search` \

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

  - **Description**: List of countries retrieved successfully \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
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

## Search State
  **Description**: Search for states as an authenticated user \
  **Method**: POST \
  **Endpoint**: `/geo/state/search` \

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
                  "query": "US"
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

  - **Description**: List of states retrieved successfully \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "New York",
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

  **Request Body Example (Search by Name)**:

  ```json
  {
      "search": {
          "page": 1,
          "per_page": 15,
          "where": [
              {
                  "kind": "LR_LIKE",
                  "field": "name",
                  "query": "York"
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

  - **Description**: List of states retrieved successfully (search by name) \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "2e138e20-7642-4615-992b-8653b44f6431",
                "name": "New York",
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
  **Description**: Create a product as an authenticated user \
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
          "reference_number": "T01MGZ",
          "description": "Men's T-shirt, 100% coton, blue",
          "origin_country": "2e138e20-7642-4615-992b-8653b44f6431",
          "customer": "a7740ec5-4521-4649-a214-e69bca68f8f9",
          "manufacturer": "0d2c4713-9ba0-4c5f-ae21-cd3685e6ace7",
          "tariff_codes": [
              {
                  "type": "HTS",
                  "code": "6105.10.0010"
              }
          ]
      }
  }
  ```

  **Responses**:

  - **Description**: Product successfully created \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "generated-uuid",
            "name": "T-shirt",
            "reference_number": "T01MGZ",
            "description": "Men's T-shirt, 100% coton, blue",
            "created_at": "timestamp",
            "updated_at": "timestamp",
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
                "uuid": "0d2c4713-9ba0-4c5f-ae21-cd3685e6ace7",
                "name": "Acme",
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
                    "uuid": "generated-uuid",
                    "type": "HTS",
                    "code": "6105.10.0010",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                }
            ],
            "attachments": []
        }
    }
    ```

## Delete Product
  **Description**: Delete a product as an authenticated user \
  **Method**: DELETE \
  **Endpoint**: `/product/${product.uuid}`

  **Parameters**:
  - product.uuid: The unique identifier of the product to be deleted (i.e 3bb27fd5-8c2c-4a0c-9ba1-6f912c278301)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Product successfully deleted \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Update Product
  **Description**: Update a product as an authenticated user \
  **Method**: PUT \
  **Endpoint**: `/product/${product.uuid}`

  **Parameters**:
  - product.uuid: The unique identifier of the product to be updated (i.e 3bb27fd5-8c2c-4a0c-9ba1-6f912c278301)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Request Body Example**:

  ```json
  {
      "product": {
          "name": "T-shirt",
          "reference_number": "T01MGZs",
          "description": "Men's T-shirt, 100% coton, blue",
          "origin_country": "2e138e20-7642-4615-992b-8653b44f6431",
          "customer": "a7740ec5-4521-4649-a214-e69bca68f8f9",
          "manufacturer": "0d2c4713-9ba0-4c5f-ae21-cd3685e6ace7"
      }
  }
  ```

  **Responses**:

  - **Description**: Product successfully updated \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "3bb27fd5-8c2c-4a0c-9ba1-6f912c278301",
            "name": "T-shirt",
            "reference_number": "T01MGZs",
            "description": "Men's T-shirt, 100% coton, blue",
            "created_at": "timestamp",
            "updated_at": "timestamp",
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
                "uuid": "0d2c4713-9ba0-4c5f-ae21-cd3685e6ace7",
                "name": "Acme",
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
                    "uuid": "generated-uuid",
                    "type": "HTS",
                    "code": "6105.10.0010",
                    "created_at": "timestamp",
                    "updated_at": "timestamp"
                }
            ],
            "attachments": [
                {
                    "uuid": "7abaafad-2b20-4a65-81a9-1c7c27d0f705",
                    "type": "other",
                    "name": "image.png",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ]
        }
    }
    ```

## View Product
  **Description**: Retrieve product details as an authenticated user \
  **Method**: GET \
  **Endpoint**: `/product/${product.uuid}`

  **Parameters**:
  - product.uuid: The unique identifier of the product to be retrieved (i.e 3bb27fd5-8c2c-4a0c-9ba1-6f912c278301)

  **Request Headers**:
  - Content-Type: application/json
  - Authorization: token

  **Responses**:

  - **Description**: Product details successfully retrieved \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "3bb27fd5-8c2c-4a0c-9ba1-6f912c278301",
            "name": "T-shirt",
            "reference_number": "T01MGZ",
            "description": "Men's T-shirt, 100% coton, blue",
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
            ],
            "attachments": [
                {
                    "uuid": "7abaafad-2b20-4a65-81a9-1c7c27d0f705",
                    "type": "other",
                    "name": "image.png",
                    "created_at": "2099-01-01T00:00:00Z",
                    "updated_at": "2099-01-01T00:00:00Z"
                }
            ]
        }
    }
    ```

## View Tariff Detail
  **Description**: Retrieve tariff detail information for a specific HTS code \
  **Method**: GET \
  **Endpoint**: `/product/tariff-detail/hts/${tariff-detail.code}/US?include=pga_flags,remedy_flags`

  **Parameters**:
  - `tariff-detail.code`: The HTS code to retrieve tariff details for (i.e. `9105.99.10.00`)

  **QueryString**:
  - `include`: Specifies additional data to be included in the response (e.g. `pga_flags,remedy_flags`)

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Responses**:

  - **Description**: Successful response with tariff detail information \
    **HTTP Code**: 200 \
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
            "units": ["No."],
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
                    "uuid": "${tariff-scenario.uuid}",
                    "affected_country": "US",
                    "tariff_code_pattern": "9105.99.10",
                    "type": 0,
                    "type_name": "Percentage",
                    "value": 25.0,
                    "tariff": {
                        "uuid": "${tariff.uuid}",
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
            "pga_flags": null,
            "remedy_flags": {
                "possible_add_required_indicator": false,
                "possible_cvd_duty_required_indicator": false
            }
        }
    }
    ```

## Update Tariff Configuration
  **Description**: Update the tariff configuration for a specific tariff code within a product \
  **Method**: PUT \
  **Endpoint**: `/product/${product.uuid}/tariff-code/${tariff-code.uuid}/configuration`

  **Parameters**:
  - `product.uuid`: The UUID of the product whose tariff configuration is being updated
  - `tariff-code.uuid`: The UUID of the tariff code within the product

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
      "configuration": {
          "base_tariff": "0",
          "additional_tariffs": "0,1",
          "upcoming_tariffs": "${tariff.upcoming_tariff}",
          "rumored_tariffs": "${tariff.rumored_tariff},${tariff.rumored_tariff_2}"
      }
  }
  ```

  **Responses**:

  - **Description**: Successful update of the tariff configuration \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "base_tariff": "0",
            "additional_tariffs": "0,1",
            "upcoming_tariffs": "${tariff.upcoming_tariff}",
            "rumored_tariffs": "${tariff.rumored_tariff},${tariff.rumored_tariff_2}"
        }
    }
    ```

## Search Product
  **Description**: Search for products based on criteria and filters \
  **Method**: POST \
  **Endpoint**: `/product/search`

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

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

  - **Description**: Successful product search response \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "${product.uuid}",
                "name": "T-shirt",
                "reference_number": "T01MGZ",
                "description": "Men's T-shirt, 100% coton, blue",
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
                        "uuid": "${tariff-code.uuid}",
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

## Search Product with Field Inclusion
  **Description**: Search for products with additional requested fields \
  **Method**: POST \
  **Endpoint**: `/product/search?arrival_date=2025-01-01&include=tariff_codes.detail,tariff_codes.configuration`

  **QueryString**:
  - `arrival_date`: set arrival date (e.g., `2025-01-01`)
  - `include`: Additional fields to include in the response (e.g., `tariff_codes.detail,tariff_codes.configuration`)

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
    "search": {
      "page": 1,
      "per_page": 1,
      "where": [{
        "kind": "EQUAL",
        "field": "bulk_process_uuid",
        "query": "${bulk_process.uuid}"
      }],
      "order_by": []
    }
  }
  ```

  **Responses**:

  - **Description**: Successful search with included fields \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "${product.uuid}",
                "name": "T-shirt",
                "reference_number": "T01MGZ",
                "description": "Men's T-shirt, 100% coton, blue",
                "created_at": "2099-01-01T00:00:00Z",
                "updated_at": "2099-01-01T00:00:00Z",
                "origin_country": {
                    "uuid": "${country.uuid}",
                    "name": "United States",
                    "iso_code": "US"
                },
                "tariff_codes": [
                    {
                        "uuid": "${tariff-code.uuid}",
                        "type": "HTS",
                        "code": "6105.10.0010",
                        "detail": {
                            "code": "6105.10.0010",
                            "description": "Standard marine chronometers having spring-detent escapements",
                            "description_heading": [
                                "Other clocks:",
                                "Other:",
                                "Other:",
                                "Standard marine chronometers having spring-detent escapements"
                            ],
                            "units": ["No."],
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
                                        }
                                    ]
                                }
                            ]
                        },
                        "configuration": {
                            "base_tariff": "0",
                            "additional_tariffs": null,
                            "upcoming_tariffs": null,
                            "rumored_tariffs": null
                        }
                    }
                ]
            }
        ],
        "meta": {
            "page": 1,
            "per_page": 1,
            "total_rows": 1,
            "total_pages": 1
        }
    }
    ```

## Search Product with Flags
  **Description**: Search for products with PGA and remedy flags included \
  **Method**: POST \
  **Endpoint**: `/product/search?arrival_date=2025-01-01&include=tariff_codes.pga_flags,tariff_codes.remedy_flags`

  **QueryString**:
  - `arrival_date`: set arrival date (e.g., `2025-01-01`)
  - `include`: Additional fields to include in the response (e.g., `tariff_codes.pga_flags,tariff_codes.remedy_flags`)

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
    "search": {
      "page": 1,
      "per_page": 1,
      "where": [{
        "kind": "EQUAL",
        "field": "bulk_process_uuid",
        "query": "${bulk_process.uuid}"
      }],
      "order_by": []
    }
  }
  ```

  **Responses**:

  - **Description**: Successful search with included flags \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": [
            {
                "uuid": "${product.uuid}",
                "name": "T-shirt",
                "reference_number": "T01MGZ",
                "description": "Men's T-shirt, 100% coton, blue",
                "created_at": "2099-01-01T00:00:00Z",
                "updated_at": "2099-01-01T00:00:00Z",
                "origin_country": {
                    "uuid": "${country.uuid}",
                    "name": "United States",
                    "iso_code": "US"
                },
                "tariff_codes": [
                    {
                        "uuid": "${tariff-code.uuid}",
                        "type": "HTS",
                        "code": "6105.10.0010",
                        "pga_flags": [
                            {
                                "pga_name": "PGA NAME 1",
                                "pga_flag_code": "PGA FLAG CODE 1",
                                "pga_flag_description": "PGA FLAG DESCRIPTION 1"
                            }
                        ],
                        "remedy_flags": {
                            "possible_add_required_indicator": false,
                            "possible_cvd_duty_required_indicator": false
                        }
                    }
                ]
            }
        ],
        "meta": {
            "page": 1,
            "per_page": 1,
            "total_rows": 1,
            "total_pages": 1
        }
    }
    ```

## Upload an Attachment
  **Description**: Upload a file attachment for a specific product \
  **Method**: POST \
  **Endpoint**: `/product/${product.uuid}/attachment/other`

  **Parameters**:
  - `product.uuid`: The UUID of the product for which the attachment is being uploaded

  **Request Headers**:
  - `Authorization`: `token`

  **Request Body Example**:

  The request body contains a multipart file upload with the following field:

  - `file`: The file to be uploaded (e.g., `image.png`)

  **Responses**:

  - **Description**: Successful file upload \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "uuid": "${attachment.uuid}",
            "type": "other",
            "name": "image.png",
            "created_at": "${attachment.created_at}",
            "updated_at": "${attachment.updated_at}"
        }
    }
    ```

## Delete an Attachment
  **Description**: Delete an attachment from a specific product \
  **Method**: DELETE \
  **Endpoint**: `/product/${product.uuid}/attachment/${attachment.uuid}`

  **Parameters**:
  - `product.uuid`: The UUID of the product from which the attachment is being deleted
  - `attachment.uuid`: The UUID of the attachment to be deleted

  **Request Headers**:
  - `Authorization`: `token`

  **Responses**:

  - **Description**: Attachment successfully deleted \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Download an Attachment
  **Description**: Download an attachment from a specific product \
  **Method**: GET \
  **Endpoint**: `/product/${product.uuid}/attachment/${attachment.uuid}?authorization=token`

  **Parameters**:
  - `product.uuid`: The UUID of the product from which the attachment is being downloaded
  - `attachment.uuid`: The UUID of the attachment to be downloaded

  **QueryString**:
  - `authorization`: Authorization token for the request (e.g., `token`)

  **Responses**:

  - **Description**: Successful attachment download \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```text
    123
    ```

## Remove a Tariff Code
  **Description**: Remove a tariff code from a specific product \
  **Method**: DELETE \
  **Endpoint**: `/product/${product.uuid}/tariff-code/${tariff-code.uuid}`

  **Parameters**:
  - `product.uuid`: The UUID of the product from which the tariff code is being removed
  - `tariff-code.uuid`: The UUID of the tariff code to be removed
  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Responses**:

  - **Description**: Tariff code successfully removed \
    **HTTP Code**: 204 \
    **Example Response Body**:
    *(No content)*

## Create a Tariff Code
  **Description**: Create a new tariff code for a specific product \
  **Method**: POST \
  **Endpoint**: `/product/${product.uuid}/tariff-code`

  **Parameters**:
  - `product.uuid`: The UUID of the product to which the tariff code is being added

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

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

  - **Description**: Tariff code successfully created \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
      "data": {
        "uuid": "${tariff-code.uuid}",
        "type": "HTS",
        "code": "6105.10.0010",
        "created_at": "${tariff-code.created_at}",
        "updated_at": "${tariff-code.updated_at}"
      }
    }
    ```

## Upload a Bulk Product File
  **Description**: Upload a bulk product file for processing \
  **Method**: POST \
  **Endpoint**: `/product/bulk/upload`

  **Request Headers**:
  - `Authorization`: `token`
  **Request Body Example**:

  The request body contains a multipart file upload with the following field:

  - `file`: The CSV file to be uploaded (e.g., `product-bulk-upload.csv`)

  **Responses**:

  - **Description**: Successful file upload for bulk processing \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "data": {
            "bulk_process_uuid": "${bulk-process.uuid}"
        }
    }
    ```

## Download CSV Report
  **Description**: Download a CSV report for a specific bulk process \
  **Method**: GET \
  **Endpoint**: `/product/csv-download/${bulk-process.uuid}`

  **Parameters**:
  - `bulk-process.uuid`: The UUID of the bulk process for which the CSV report is being downloaded

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Responses**:

  - **Description**: Successful CSV file download \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```csv
    product_name;product_description;hts_code;origin_country;base_tariff_rate;additional_tariff;upcoming_tariff;rumored_tariff;total_effective_tariff;antidumping_duty;countervailing_duty;pga_flags
    "T-shirt";"Man's T-shirt G coton blue";"6105.10.0010";"United States";"94.5 USD";"0 ";"0 ";"0 ";"N/A";"FALSE";"FALSE";"PGA FLAG CODE 1"
    ```

## Product AI Chat Initialization
  **Description**: Initialize an AI-powered chat for product classification and suggestions \
  **Method**: POST \
  **Endpoint**: `/product/ai-chat/init`

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
    "chat": {
      "name": "T-shirt",
      "description": "Men's T-shirt, 100% coton, blue",
      "origin_country": "China",
      "additional_message": "It has zipper"
    }
  }
  ```

  **Responses**:

  - **Description**: Successful AI chat initialization with classification suggestions \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```json
    {
        "suggested_desc": "Men's T-shirt made of 100% cotton with a zipper.",
        "best_guess": {
            "61": 0.85,
            "62": 0.1,
            "50": 0.05
        },
        "disambiguation": {
            "Is the T-shirt knitted or crocheted?": [
                "Knitted",
                "Not knitted"
            ],
            "What is the weight of the fabric?": [
                "Lightweight",
                "Medium weight",
                "Heavyweight"
            ],
            "Is the zipper functional or decorative?": [
                "Functional",
                "Decorative"
            ],
            "What is the size range of the T-shirt?": [
                "Small",
                "Medium",
                "Large",
                "Extra Large"
            ]
        }
    }
    ```

## Product AI Chat Refinement
  **Description**: Refine an AI-powered product classification by providing additional details \
  **Method**: POST \
  **Endpoint**: `/product/ai-chat/refine`

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
    "chat": {
      "improved_description": "Men's T-shirt made of 100% cotton with zipper.",
      "initial_chat_result": {
          "suggested_desc": "Men's T-shirt made of 100% cotton with zipper.",
          "best_guess": {
              "61": 0.85,
              "62": 0.1,
              "50": 0.05
          },
          "disambiguation": {
              "Is the T-shirt knitted or crocheted?": [
                  "Knitted",
                  "Not knitted"
              ],
              "What is the weight of the fabric?": [
                  "Lightweight",
                  "Medium weight",
                  "Heavyweight"
              ],
              "Is the zipper functional or decorative?": [
                  "Functional",
                  "Decorative"
              ],
              "What is the size range of the T-shirt?": [
                  "Small",
                  "Medium",
                  "Large",
                  "Extra Large"
              ]
          }
      },
      "answers": {
        "Is the T-shirt knitted or crocheted?": "Knitted",
        "What is the weight of the fabric?": "Lightweight",
        "Is the zipper functional or decorative?": "Functional",
        "What is the size range of the T-shirt?": "Small"
      },
      "continuation_text": []
    }
  }
  ```

  **Responses**:

  - **Description**: AI-suggested HTS classification refinement \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```text
    ## Recommended HTS Code
    - **Recommended HTS Code**: <mark>6109.10.00.11</mark>
    - **Tariff-ready Official Description**: <mark>T-shirts, singlets, tank tops and similar garments, knitted or crocheted: - Of cotton - Men's or boys': - Thermal undershirts</mark>

    ## Analysis of Options
    - **Knitted or Crocheted**: The T-shirt is confirmed to be knitted.
    - **Material**: Made of 100% cotton.
    - **Zipper**: The zipper is functional, which may influence classification.
    - **Weight**: Described as lightweight.
    - **Size Range**: Small, indicating it is a men's shirt.

    ### Potential HTS Codes Considered
    1. **Men's or boys' shirts, knitted or crocheted: - Of cotton - Men's**: <mark>6105.10.00.10</mark>
    2. **Men's or boys' shirts, knitted or crocheted: - Of cotton - Boys'**: <mark>6105.10.00.30</mark>
    3. **T-shirts, singlets, tank tops and similar garments, knitted or crocheted: - Of cotton - Men's or boys'**: <mark>6109.10.00.11</mark> (Recommended)
    4. **Men's or boys' shirts: - Of cotton: - Dress shirts**: <mark>6205.20.20.16</mark> (Not applicable due to T-shirt classification)

    ## Conclusion
    The most reliable classification for the described product is under HTS code <mark>6109.10.00.11</mark>, as it accurately reflects the characteristics of a knitted cotton T-shirt with a functional zipper.
    ```

## Product AI Chat Refinement with User Preference
  **Description**: Refine AI-powered classification with user-selected HTS code \
  **Method**: POST \
  **Endpoint**: `/product/ai-chat/refine`

  **Request Headers**:
  - `Content-Type`: `application/json`
  - `Authorization`: `token`

  **Request Body Example**:

  ```json
  {
    "chat": {
      "improved_description": "Men's T-shirt made of 100% cotton with zipper.",
      "initial_chat_result": {
          "suggested_desc": "Men's T-shirt made of 100% cotton with zipper.",
          "best_guess": {
              "61": 0.85,
              "62": 0.1,
              "50": 0.05
          },
          "disambiguation": {
              "Is the T-shirt knitted or crocheted?": [
                  "Knitted",
                  "Not knitted"
              ],
              "What is the weight of the fabric?": [
                  "Lightweight",
                  "Medium weight",
                  "Heavyweight"
              ],
              "Is the zipper functional or decorative?": [
                  "Functional",
                  "Decorative"
              ],
              "What is the size range of the T-shirt?": [
                  "Small",
                  "Medium",
                  "Large",
                  "Extra Large"
              ]
          }
      },
      "answers": {
        "Is the T-shirt knitted or crocheted?": "Knitted",
        "What is the weight of the fabric?": "Lightweight",
        "Is the zipper functional or decorative?": "Functional",
        "What is the size range of the T-shirt?": "Small"
      },
      "continuation_text": [
        "I want to use the HTS 6105.10.00.10"
      ]
    }
  }
  ```

  **Responses**:

  - **Description**: AI classification refined with user-selected HTS code \
    **HTTP Code**: 200 \
    **Example Response Body**:
    ```text
    ### Recommended HTS Code
    - **HTS Code**: <mark>6105.10.00.10</mark>
    - **Tariff-ready Official Description**: <mark>Men's or boys' shirts, knitted or crocheted: - Of cotton - Men's</mark>

    ### Justification
    - The product is a men's T-shirt made of 100% cotton.
    - It is confirmed to be knitted.
    - The presence of a functional zipper does not disqualify it from this classification, as it is still categorized under men's shirts.

    ### Conclusion
    The HTS code <mark>6105.10.00.10</mark> is appropriate for your product based on the provided details.
    ```

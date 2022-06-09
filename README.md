# cf-api
circular.fashion's API for creating, reading, updating and deleting Products.

Please contact us at <?> to set up your account be API enabled.
Once the API for your account is enabled, you can use the instructions below to handle Products.

# Overview

* [API Root](#api-root)
* [Authentication](#authentication)
* [Request Types](#request-types)
* [Response Types](#response-types)
* [List of Endpoints](#list-of-endpoints)

# API Root
All endpoints to the API start with a URL like:
```
https://app.circular.fashion/circularity_id/api/v<api_version>/data/v<data_version>
```

Currently, the only API available is version 0, with the data version 3 so the start of the API uris will be:
```
https://app.circular.fashion/circularity_id/api/v0/data/v3
```

# Authentication and API key management
The API key model is specific for the circularity id APIs and directly linked to a company. 

API keys cannot be deleted as such but are rather revoked. 

The keys are specific for circularity id. This allows for separation of concerns when other APIs will be published.

## Obtain an API key
Obtain an API key at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).
Alternatively, you can use the API to create an API key:
```
POST /users/api/v1/api-key/
```
This key can only be viewed **once**.

The API key is generated for the company of the requesting user and returned in a JSON with status code 200:
`{"generated_key": <key>}` On authentication failure a `403` error is returned.

If a key already exists for the company, a new one will be created. The old one will be revoked and can no longer be used.

> Please note that the API key is on a **company basis**, not on a user basis.

## Use the API key
When sending a request to any endpoint, the `HTTP header` must contain `Authorization: Api-Key <key>`:
```
Content-Type: application/json
Authorization: Api-Key <key>
```

## Revoke API key
```
DELETE /users/api/v1/api-key/
```
### Response
If an API key exists, it will be revoked. If no API key exists, this information will not be disclosed to the user.

It returns a JSON with status `200`: `{"success": true}`

On authentication failure a `403` is returned.

## Verify an API Key
```
GET /users/api/v1/api-key/test/
```
### Response
If the key is valid, a JSON object is received:
```
{"success": true}
```
Otherwise, the JSON object will look like:
```
{"success": false}
```

It can also return a response code `403` in case of error.

# Request Types
## Create
```
POST /products
```
### Payload
Refer to [product_payload_definition.json](product_payload_definition.json) and [product_payload_sample.json](product_payload_sample.json) for the payload format.

- `base64_image_file` needs to be replaced by a base64 encoding, smaller than 5MB, of an image file ("jpeg", "jpg" or "png").
- `base64_pdf_file` needs to be replaced by a base64 encoding, smaller than 5MB, of a file ("pdf", "jpeg", "jpg or "png").

### Response
If successful, the API will send an HTTP response `201`.

The response contains a generated `product_id`. You can use this `product_id` to view or delete the product.
```
{
    "product_id": 1
}
```
## List
```
GET /products
```
### Response
The response is an array of all products owned by the user's company. See [product_list_sample.json](product_list_sample.json).
## View
```
GET /products/<product_id>
```
### Response
The response is the product with `product_id`.
## Delete
```
DELETE /products/<product_id>
```
### Response
If successful, the API will send an HTTP response `204`.

# Response Types

# List of Endpoints
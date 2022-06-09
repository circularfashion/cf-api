# cf-api
circular.fashion's API for creating Products. Please contact us at <?> to set up your account be API enabled.
Once the API for your account is enabled, you can use the instructions below to create Products.

# Overview

* [API Root](#-api-root)
* [Authentication](#-authentication)
* [Request Types](#-request-types)
* [Response Types](#-response-types)
* [List of Endpoints](#list-of-endpoints)

# API Root
```
https://app.circular.fashion/circularity-id/api/0/data/3/
```
# Authentication
The API key model is specific for the circularity id APIs and directly linked to a company. 

API keys cannot be deleted as such but are rather revoked. 

The keys are specific for circularity id. This allows for separation of concerns when other APIs will be published.

## Obtain an API key
Obtain an API key at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).
Alternatively, use the API to create an API key: 
```
POST /users/api/v1/api-key/
```
This key can only be viewed once.
The API key is generated for the company of the requesting user and returned in a JSON with status code 200:
`{"generated_key": <key>}` On authentication failure a 403 is returned.



If already a key exists for the company, a new one will be created. The old one will be revoked and can no longer be used. 

Please note that the API key is on a **company basis**, not on a user basis.

## Revoke API key
```

```

## Verify an API Key
```
POST /users/api/v1/api-key/test/
```
### Payload
```
None
```
### Response
If the key is valid, you will receive an JSON object:
```
{"success": true}
```
Otherwise, you will receive an JSON object:
```
{"success": false}
```

or an response code 403
## Using the access token
When sending a messages to any endpoint, the `header` must contain `Authorization: Api-Key <key>`:
```
Content-Type: application/json
Authorization: Api-Key <key>
```
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
# cf-api
circular.fashion's API for creating Products. Please contact us at <?> to set up your account be API enabled.
Once the API for your account is enabled, you can use the instructions below to create Products.

# API Root
```
https://app.circular.fashion/api
```
# Authentication
Obtain an API key at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).

The API key is on a company basis, not on a user basis.

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
# Endpoints
## Create
```
POST /products
```
### Payload
Refer to [product_payload_definition.json](product_payload_definition.json) and [product_payload_sample.json](product_payload_sample.json) for the payload format.
### Response
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

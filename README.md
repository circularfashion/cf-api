# cf-api
circular.fashion's API for creating, reading, updating and deleting Products.

Please contact us at info@circular.fashion to set up your account be API enabled.
Once the API for your account is enabled, you can use the instructions below to handle Products.

You can also contact us at develop@circular.fashion if you have any questions or feedback regarding the API.

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

> **Disclaimer**: This first version is not made to stay and may be deprecated at some point.

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
This key can only be viewed **once**. _We will not save your API key in your profile._

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

This is a C.R.U.D (Create Retrieve Update Delete) API.
All those operations are available for each endpoint.

## Endpoint

> Quick reminder, all endpoints start with the [API Root](#api-root).

For each data type, there are two different endpoints, a generic and a specific one.

The generic one is for listing all the data instances or creating a new instance.
It is formatted as :
```
 /<data_type>
```
Examples:
* `/products` for the products.
* `/products/4/variations` for variations in the product with id **4**

The specific one, with the instance id, is for getting the data instance, updating and deleting it.
It is formatted as:
```
 /<data_type>/<data_id>
```
Examples:
* `/products/4` to get the product with id **4**.
* `/products/4/variations/3` for the variation with id **3**, in the product with id **4**

## Create a new element
Generic endpoint, `POST` method.
```
POST /<data_type>
```
### Payload
A valid json with the data fields.

You can see an example for products in [product_payload_sample.json](product_payload_sample.json).  
See [product_payload_definition.json](product_payload_definition.json) for the payload format.

- `base64_image_file` needs to be replaced by a base64 encoding, smaller than 5MB, of an image file ("jpeg", "jpg" or "png").
- `base64_pdf_file` needs to be replaced by a base64 encoding, smaller than 5MB, of a file ("pdf", "jpeg", "jpg or "png").

> NB: It is possible to convert a file to base64 using many tools like websites, the python `base64` library, ...

### Response
If successful, the API will send an HTTP response `201`.

The response contains the generated `id`. You can use this `id` to view, delete or update the data.
```
{
    "id": 1
}
```
For some endpoints it will also contain all the fields for the data that was created


If not successful, it will usually return a `400` HTTP code, and the response will contain information on the error that occured.

## List
Generic endpoint, `GET` method.
```
GET /<data_type>
```
### Response
The response is an array of all data instance of `data_type` owned by the user's company.

See [product_list_sample.json](product_list_sample.json) for an example on what could be returned

## Get a specific element
Specific endpoint, `GET` method.
```
GET /<data_type>/<data_id>
```
### Response
If successful, `200` code and the corresponding data for the given id is returned.
If not, returns a `404` error code.

## Delete
Specific endpoint, `DELETE` method.
```
DELETE /<data_type>/<data_id>
```
### Response
If successful, the API will send an HTTP response `204`.

##  Update
Specific endpoint, `DELETE` method.
```
PATCH /<data_type>/<data_id>
```
### Payload
JSON with the data fields that we want to update
For instance, to update the product name
```
{
    "name": "New name"
}
```

> It is **not** possible to edit a child data through its parent endpoint. The direct endpoint for this needs to be used (eg, to update a variation name, go to `/products/<product_id>/variations/<variation_id>`).
> An exception is made on colours.

### Response
If successful, `200` code and the updated data is returned.
If not, should return a `400` error with information on what happened.

# List of Endpoints

Here is a full list of all endpoints available.

Methods on these endpoints can be seen in [Request Types](#request-types).

> As a reminder, all endpoints start with [API Root](#api-root)

* `/products`
* `/products/<product_id>`

* `/products/<product_id>/variations`
* `/products/<product_id>/variations/<variation_id>`

* `/products/<product_id>/variations/<variation_id>/product_images`
* `/products/<product_id>/variations/<variation_id>/product_images/<product_image_id>`

* `/products/<product_id>/variations/<variation_id>/care_guide_images`
* `/products/<product_id>/variations/<variation_id>/care_guide_images/<care_guide_image_id>`

* `/products/<product_id>/variations/<variation_id>/services`
* `/products/<product_id>/variations/<variation_id>/services/<service_id>`

* `/products/<product_id>/variations/<variation_id>/circular_design_strategies`
* `/products/<product_id>/variations/<variation_id>/circular_design_strategies/<circular_design_strategy_id>`

* `/products/<product_id>/variations/<variation_id>/certifications`
* `/products/<product_id>/variations/<variation_id>/certifications/<certification_id>`

* `/products/<product_id>/variations/<variation_id>/product_steps`
* `/products/<product_id>/variations/<variation_id>/product_steps/<product_step_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/social_environmental_certifications`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/social_environmental_certifications/<social_environmental_certification_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/material_safety_data_sheets`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/material_safety_data_sheets/<material_safety_data_sheets_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/chemical_compliances`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/chemical_compliances/<chemical_compliances_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/biodegradability_certifications`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/biodegradability_certifications/<biodegradability_certification_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/steps`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/steps/<step_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/components`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/components/<component_id>`

* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/components/<component_id>/steps`
* `/products/<product_id>/variations/<variation_id>/assemblies/<assembly_id>/materials/<material_id>/components/<component_id>/steps/<step_id>`

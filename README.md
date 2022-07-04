# cf-api
circular.fashion's API for creating, reading, updating and deleting Products.

Please contact us at info@circular.fashion to set up your account to have the API enabled.
Once the API for your account is enabled, you can use the instructions below to handle Products.

You can also contact us at develop@circular.fashion if you have any questions or feedback regarding the API.

# Overview

* [API Root](#api-root)
* [Authentication and API Key Management](#authentication-and-api-key-management)
* [Request Types](#request-types)
* [List of Endpoints](#list-of-endpoints)

# API Root
All endpoints to the API start with a URL like:
```
https://app.circular.fashion/circularity_id/api/v<api_version>/data/v<data_version>
```

Currently, the only API available is version 0, with the data version 3 so the start of the API uris look like this:
```
https://app.circular.fashion/circularity_id/api/v0/data/v3
```

> **Disclaimer**: This first version is not made to stay and may be deprecated at some point in time.

# Authentication and API key management
The API key model is specific for the circularity.id APIs and directly linked to a company. 

API keys cannot be deleted as such but can rather be revoked. 

The keys are specific for circularity id. This allows for separation of concerns when other APIs will be published.

Creating and deleting an API Key can be done at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).

Alternatively, it can also be created using an API. The root for this API is :
```
https://app.circular.fashion/users/api/v<user-api-version>/api-key
```
It uses the same authentication method as the other usual requests.

## Obtain an API key
When logged in, obtain an API key at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).

If facing a `403 - Forbidden` error, contact us at info@circular.fashion to ensure the API is enabled for your account.

Alternatively, you can use the API to create an API key, eg:
```
POST https://app.circular.fashion/users/api/v1/api-key/
```

This key can only be viewed **once**. _The API key is not saved in your profile._

The API key is generated for the company of the requesting user and returned in a JSON with status code 200:
`{"generated_key": <key>}`. On authentication failure a `403` error is returned.

If a key already exists for the company, a new one will be created. The old one will be revoked and can no longer be used.

> Please note that the API key is on a **company basis**, not on a user basis.

## Revoke API key
When logged in, delete the API key at [https://app.circular.fashion/users/api-key](https://app.circular.fashion/users/api-key).

Alternatively, you can use the API to delete an API key, eg:
```
DELETE https://app.circular.fashion/users/api/v1/api-key/
```
### Response
If an API key exists, it will be revoked. If no API key exists, this information will not be disclosed to the user.

It returns a JSON with status `200`: `{"success": true}`

On authentication failure a `403` is returned.

## Use the API key
When sending a request to any endpoint, the `HTTP header` must contain `Authorization: Api-Key <key>`:
```
Content-Type: application/json
Authorization: Api-Key <key>
```

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

- `base64_image_file`: replace by a base64 encoded image file, smaller than 5MB ("jpeg", "jpg" or "png").
- `base64_pdf_file`: replace by a base64 encoded file, smaller than 5MB ("pdf", "jpeg", "jpg or "png").

> Tip: It is possible to convert a file to base64 using many tools like websites, the python `base64` library, ...

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
The response is an array of all data instances of `data_type` owned by the user's company.

See [product_list_sample.json](product_list_sample.json) for an example on what could be returned

## Get a specific element
Specific endpoint, `GET` method.
```
GET /<data_type>/<data_id>
```
### Response
If successful, `200` code and the corresponding data for the given id is returned.
Otherwise, a `404` error code is returned.

## Delete
Specific endpoint, `DELETE` method.
```
DELETE /<data_type>/<data_id>
```
### Response
If successful, the API will send an HTTP response `204`.

##  Update
Specific endpoint, `PATCH` method.
```
PATCH /<data_type>/<data_id>
```
### Payload
JSON with the data fields we want to update.
For instance, updating the product name:
```
{
    "name": "New name"
}
```

> It is **not** possible to edit a child data through its parent endpoint. The direct endpoint for this needs to be used (e.g., to update a variation name, use `/products/<product_id>/variations/<variation_id>`).
> 
> An exception is made on colours, as we're not really creating or updating existing colours but assigning existing colours to elements.

### Response
If successful, `200` code and the updated data is returned.
Else, a `400` error with information on what happened is returned.

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

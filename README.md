# cf-api
circular.fashion's API for creating Products. Please contact us at <?> to set up your account be API enabled.
Once the API for your account is enabled, you can use the instructions below to create Products.

# Authentication
Get an authentication token using JWT authentication. The token will last 60 minutes, at which point it will need to be
refreshed or a new one generated.

## Create an access token
```
POST https://app.circular.fashion/api/jwt/create
```
### Payload
`username` and `password` of a user in the account with API access enabled.
```
{
    "username": <your_username>,
    "password": <your_password>
}
```
### Response
You will receive 2 tokens:

- `access` token - Used to access API endpoints. Lasts 60 minutes.
- `refresh` token - Used to refresh the `access` token. Lasts 1 week. After that, you must create a new
`access` token using `username` and `password` login credentials.

```
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY0NTk2MjM0MSwianRpIjoiODA2YWUwNzUwN2Y4NGExN2IxMTMxYjRlMjJjZGU4ZmEiLCJ1c2VyX2lkIjoyMn0.qzmE4lL1TAvt2WdcigHdgoAJtMWQCQgcPERfODl5x6E",
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjQ1MzYxMTQxLCJqdGkiOiJjYWIzMjI0N2MxZDA0N2M1OWMzMDdiMzU1ZTdiM2I3MCIsInVzZXJfaWQiOjIyfQ.d0md3S3yUgpbAoW5_XVaiBGBnnlXG2OdwvXkgXzqGco"
}
```

## Refresh an access token
```
POST https://app.circular.fashion/api/jwt/refresh
```
### Payload
Provide the `refresh` token received when creating the `access` token.
```
{
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTY0NTk2MjM0MSwianRpIjoiODA2YWUwNzUwN2Y4NGExN2IxMTMxYjRlMjJjZGU4ZmEiLCJ1c2VyX2lkIjoyMn0.qzmE4lL1TAvt2WdcigHdgoAJtMWQCQgcPERfODl5x6E",
}
```
### Response
The response will contain a new `access` token.
```
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjQ1MzYyMDQwLCJqdGkiOiJlMmQzNTFiNmUzYzU0NmI2ODk5ZmM5YWUyZTFiZTFhZiIsInVzZXJfaWQiOjIyfQ.lfPywjR0h4V2ZCkbxffwVvV-uvScvoU31x9IQybutn0"
}
```

## Verify an access token
```
POST https://app.circular.fashion/api/jwt/verify
```
### Payload
```
{
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjQ1MzYxMTQxLCJqdGkiOiJjYWIzMjI0N2MxZDA0N2M1OWMzMDdiMzU1ZTdiM2I3MCIsInVzZXJfaWQiOjIyfQ.d0md3S3yUgpbAoW5_XVaiBGBnnlXG2OdwvXkgXzqGco",
}
```
### Response
If the token is valid, you will receive an empty JSON object:
```
{}
```
Otherwise, you will receive:
```
{
    "detail": "Token is invalid or expired",
    "code": "token_not_valid"
}
```
## Using the access token
When sending a messages to any endpoint, the `header` must contain the `access` token prefixed with the string "JWT":
```
Content-Type: application/json
Authorization: JWT eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjQ1MzYyMDQwLCJqdGkiOiJlMmQzNTFiNmUzYzU0NmI2ODk5ZmM5YWUyZTFiZTFhZiIsInVzZXJfaWQiOjIyfQ.lfPywjR0h4V2ZCkbxffwVvV-uvScvoU31x9IQybutn0
```
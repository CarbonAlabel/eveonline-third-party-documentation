# BETA - JWT Token validation

>## **Please note**: This is documentation for a prerelease feature and is subject to change

Clients should validate the tokens on the client side. There are many libraries for most programming languages available and listed on [JWT.IO](https://jwt.io). Auth0 also has a good and detailed introduction to [JWT token validation](https://auth0.com/docs/api-auth/tutorials/verify-access-token).

Listed below are the most important things to validate when using the JWT tokens issued by the EVE Online SSO. To explain, here is the payload from a typical SSO JWT Token.

```json
{
  "scp": [
    "esi-skills.read_skills.v1",
    "esi-skills.read_skillqueue.v1"
  ],
  "jti": "998e12c7-3241-43c5-8355-2c48822e0a1b",
  "kid": "JWT-Signature-Key",
  "sub": "CHARACTER:EVE:123123",
  "azp": "my3rdpartyclientid",
  "name": "Some Bloke",
  "owner": "8PmzCeTKb4VFUDrHLc/AeZXDSWM=",
  "exp": 1534412504,
  "iss": "login.eveonline.com"
}
```

1. ### Validate the JWT signature
    The [SSO metadata endpoint](https://login.eveonline.com/.well-known/oauth-authorization-server) contains a description of the supported operations for the SSO. That endpoint lists the supported endpoints as well as provides a link to the SSO JWT key set which is currently located at <https://login.eveonline.com/oauth/jwks>. You will need to load that key set using whatever JWT library available. Currently the SSO uses the RS-256 signature method, but will also support ES-256 in the near future.

1. ### Validate the issuer
    The issuer is always the hostname of the SSO instance you are using (mos tlikely login.eveonline.com). Make sure it matches the `iss` claim in the JWT token payload.

1. ### Validate the expiry date
    The `exp` claim contains the expiry date of the token as a UNIX timestamp. You can use that to know when to refresh the token and to make sure that the token is valid.
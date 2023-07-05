# Some Notes of Learning OAuth 2.0 and OpenID connect


___

## OAuth 2.0 : 
To provide third party applications access to restricted resource, 
OAuth introduces an authorization layer that can be controlled by the resource owner and hosted by the resource sever. 
The client gets an access token --a string denoting specific scope, lifetime, and other access attribute.
The client can use the access token to access the protected resources hosted by resource server.
### OAuth four roles:
#### 1. resource owner:  
An entity capable of allowing access to a protected resource.
#### 2. resource server:  
The server hosting the protected resources, can accept and respond to protected resource requests using access token.
#### 3. client:  
An application making protected resources requests for the resource owner and with its authorization.

#### 4. authorization server:  
The server issuing access tokens to the client after successfully authenticating the resource owner and obtaining authorization. 
It has 3 key components: 
* Authentication: handles the verification of the user credentials when the user requests information from a server.
To make sure only authenticated users can get access to the data.
* Authorization: is responsible for verifying that user is allowed to access the information requested and then issued the access token. 
* Token Management: manage the lifecycle, issuance, revocation of access tokens.

#### How it works ?  
Granting authorization ======> Token request ======> Accessing protected resources  
**Granting types**:  
Authorization code : users authenticate themselves on the authorization server, then receive authorization code to exchange for access token.  
Client Credentials : when client needs access to protected resource, client will authenticate with client ID to get access token.
### OAuth 2.0 authorization code flow

![code flow](OAuth2.0_png/code_flow.png "code flow")


### Problem with OAuth 2.0 for authentication:
* No standard way to get users' information
* Every implementation is a bit different 
* No common set of scopes


### OpenID Connect (OIDC) :
Is built on OAuth and used as an additional identity layer, OIDC allows servers to authenticate users and obtain their basic information.
Compared to OAuth 2.0, OpenID Connect adds  **ID token**, **Userinfo endpoint for getting more user information**, **Standard set of scopes**, **Standardized implementation**.  
OpenID Token is JWT (JSON Web Token), it includes Header, Payload, Signature that separated by ".".  
Header and Payload can be decoded (using Base64URL to encode), signature is created based on Based64 encoded header and payload along with other algorithm, 
it can be a proof that header or payload wasn't modified by others.  
#### Example: 
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlRpbmciLCJpYXQiOjE1MTYyMzkwMjJ9.SzAiB9uToBkg1O46Uq33xgFTOaReJW8icOBA8mmM3K0  


#### Header:   
**eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9**.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlRpbmciLCJpYXQiOjE1MTYyMzkwMjJ9.SzAiB9uToBkg1O46Uq33xgFTOaReJW8icOBA8mmM3K0  

```python
      #Header                           decode     algorithm and token type 
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9   ========>  {
                                                    "alg": "HS256",
                                                    "typ": "JWT"
                                                   }
```
#### Payload:   
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.**eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlRpbmciLCJpYXQiOjE1MTYyMzkwMjJ9**.SzAiB9uToBkg1O46Uq33xgFTOaReJW8icOBA8mmM3K0  


```python
      #Payload                                                        decode     data 
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlRpbmciLCJpYXQiOjE1MTYyMzkwMjJ9 ========> {
                                                                                 "sub": "1234567890",
                                                                                 "name": "Ting",
                                                                                 "iat": 1516239022
                                                                                }
```

#### Signature:   
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlRpbmciLCJpYXQiOjE1MTYyMzkwMjJ9.**SzAiB9uToBkg1O46Uq33xgFTOaReJW8icOBA8mmM3K0**  

```python
      #Signature                              decode    verify signature 
SzAiB9uToBkg1O46Uq33xgFTOaReJW8icOBA8mmM3K0  ========> HMACSHA256(
                                                                   base64UrlEncode(header) + "." +
                                                                   base64UrlEncode(payload),
                                                                   your-256-bit-secret
                                                                    
                                                                  )
```

### OpenID connect authorization code flow

![OIDC code flow](OAuth2.0_png/OIDC_code_flow.png "OIDC code flow")





### OAuth and OpenID Connect: 
OAuth 2.0 -> **Authorization** :  
* Allow access to your API  
* Getting access to user data in other systems  

OpenID Connect -> **Authentication** :  
* User logging  
* Making your accounts available in other systems


### reference:  
[1. The OAuth 2.0 Authorization Framework ]( https://datatracker.ietf.org/doc/html/rfc6749#section-1 "title")  
[2. OAuth 2.0 and OpenID Connect (in plain English) ](https://www.youtube.com/watch?v=996OiexHze0 "title")  
[3. What is an Authorization Server](https://www.descope.com/learn/post/authorization-server "title")
 

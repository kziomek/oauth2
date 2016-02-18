# OAuth2

This sample exemplify OAuth2 Authorization Server implemented with Spring Boot 1.3.2.
The sample presents `authorization code`, `implicity`, `password` and `refresh_token` grand types to produce access_token.

## Run

    mvn spring-boot:run
    
To prevent any possible caching etc. I used 'New Incognito Window' in Chrome for every single test.
 

## OAUTH2 Endpoints

### Authorization endpoint
The authorization endpoint is used by the authorization code grant type and implicit grant type flows. 
The response_type value MUST be one of "code" for requesting an authorization code or "token" for requesting an access token (implicit grant)

### Token endpoint
The token endpoint is used by the Client to obtain an access token by presenting its authorization grant or refresh token.
The token endpoint is used with every authorization grant except for the implicit grant type (since an access token is issued directly).

## Authorization flow types

### `authorization_code` grant type

Resource Owner (user) uses link below to produce authorization code for Client. 
Authorization Server authenticate resource owner with provided user and password and asks for authorization to Client.

1. Client invokes authorize endpoint

    [http://localhost:9999/uaa/oauth/authorize?response_type=code&client_id=acme&redirect_uri=http://example.com]()
    
2. User provides 'user' and 'password' to autenticate in Authorization Server
3. User authorize Client to get access to resources
4. Authorization server redirect code to Client. 
5. Client invokes token endpoint to get access token and response token:

    ```
    curl acme:acmesecret@localhost:9999/uaa/oauth/token -d grant_type=authorization_code -d client_id=acme  -d redirect_uri=http://example.com -d code=pWUjoP
    ```
    
6. Authorization Server redirects to Client with access_token and refresh_token:

    ```
    {"access_token":"c31701d7-1374-4948-8aa3-90ea29eb8335","token_type":"bearer","refresh_token":"489021c1-80b7-4292-915f-58b54944ec74","expires_in":43199,"scope":"openid"}
    ```
    
7. Client can refresh access_token with refresh token. To get fresh tokens Client invokes:

    ```
    curl acme:acmesecret@localhost:9999/uaa/oauth/token -d grant_type=refresh_token -d refresh_token=489021c1-80b7-4292-915f-58b54944ec74
    ```
8. Authorization Server responses with new access_token. Refresh token remains the same.
    ```
    {"access_token":"85111676-b9f9-4d85-b37a-e18e72b6c067","token_type":"bearer","refresh_token":"489021c1-80b7-4292-915f-58b54944ec74","expires_in":43199,"scope":"openid"}
    ```
    

### `implicit` grant type 
authorized-grant-type=implicit:
Implicit flow is simplified method to get access token.

1. Client invokes authorize endpoint

    [http://localhost:9999/uaa/oauth/authorize?response_type=token&client_id=acme&redirect_uri=http://example.com]()

2. User provides 'user' and 'password' to autenticate in Authorization server
3. User authorize Client to get access to resources
4. Authorization Server redirects to Client with access_token:

    [http://example.com/#access_token=85111676-b9f9-4d85-b37a-e18e72b6c067&token_type=bearer&expires_in=43101&scope=openid]()



### `password` grant type
Password flow doesn't user authorization endpoint. It is used with trusted Client.
1. Client simly use user's 'user' and 'password' to retrieve access token and refresh_token

    ```
    curl http://acme:acmesecret@localhost:9999/uaa/oauth/token -d grant_type=password -d username=user -d password=password
    ```
    
2. Authorization Server responses with access_token and refresh_token
    
    ```
    {"access_token":"85111676-b9f9-4d85-b37a-e18e72b6c067","token_type":"bearer","refresh_token":"489021c1-80b7-4292-915f-58b54944ec74","expires_in":43058,"scope":"openid"}
    ```
    
3. Client can refresh access_token with refresh token. To get fresh tokens Client invokes:
    
    ```
    curl acme:acmesecret@localhost:9999/uaa/oauth/token -d grant_type=refresh_token -d refresh_token=489021c1-80b7-4292-915f-58b54944ec74
    ```
4. Authorization Server responses with new access_token.

    ```
    {"access_token":"ca7f58c4-2ccf-4ffa-89e5-9ff7a0d15284","token_type":"bearer","refresh_token":"489021c1-80b7-4292-915f-58b54944ec74","expires_in":43199,"scope":"openid"}
    ```
    
## Library
[https://spring.io/guides/tutorials/spring-security-and-angular-js/]()

[https://tools.ietf.org/html/rfc6749]()
    
    
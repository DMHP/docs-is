# Enable Authentication for a Mobile Application

<**TODO : add following information to the guide with proper wording**>
When using the client-side applications to work with access tokens, they can not securely store the client secrets. Hence PKCE-enhanced Authorization Code Flow is used for the login flow to mitigate some security concerns.
This page guides you through enabling login using the [Authorization Code](../../../concepts/authorization/authorization-code-grant) grant type with [PKCE](insertlink) for client-side applications
(Mobile Applications, Single Page Applications) that uses OpenID Connect. 

---

This guide assumes you have your own application. If you wish to try out this flow with a sample application, click the button below. 

<a class="samplebtn_a" href="../../../quick-starts/webapp-oidc-sample" rel="nofollow noopener">Try it with the sample</a>

----

{!fragments/register-a-service-provider.md!}

----

{!fragments/oauth-app-config-basic.md!}

----


## Enable PKCE

When configuring **OAuth/OpenID Connect Configuration** for the service provider, select **PKCE Mandatory** in order to enable PKCE. 

![enable-pkce](../../assets/img/guides/enable-pkce.png)

----

## Configure the client application

Make the following requests via your application to connect your application to WSO2 IS. 
<**TODO check the request tabs. The font colors are not consistent**>

1. Obtain the `authorization_code` by sending an authorization request to the authorization endpoint. 

    !!! tip
        You can use [an online tool](https://tonyxu-io.github.io/pkce-generator/) to generate PKCE code challenges to include the `code_challenge` and `code_challenge_method` parameters. 

    ```tab="Request Format"
    https://<host>/oauth2/authorize?scope=openid&response_type=code
    &redirect_uri=<redirect_uri>
    &client_id=<oauth_client_key>
    &code_challenge=<PKCE_code_challenge>
    &code_challenge_method=<PKCE_code_challenge_method>
    ```

    ```tab="Sample Request"
    https://localhost:9443/oauth2/authorize?scope=openid&response_type=code
    &redirect_uri=https://localhost/callback
    &client_id=YYVdAL3lLcmrubZ2IkflCAuLwk0a
    &code_challenge=5vEtIy2T-G65yXHc8g5zcJDQXICBzZMrtERq0zhx7hM
    &code_challenge_method=S256
    ```
    
2. Obtain the access token by sending a token request to the token endpoint using the `authorization_code` recieved in step 1, and the `<oauth_client_key>` and `<oauth_client_secret>` obtained when configuring the service provider.


    ```tab="Request Format"
    curl -i -X POST -u <oauth_client_key>:<client_secret> -k -d 
    'grant_type=authorization_code&redirect_uri=<redirect_uri>
    &code=<authorization_code>&code_verifier=<PKCE_code_verifier>' 
    https://localhost:9443/oauth2/token
    ```

    ```tab="Sample Request"
    curl -i -X POST -u YYVdAL3lLcmrubZ2IkflCAuLwk0a:azd39swy3Krt59fLjewYuD_EylIa -k -d 
    'grant_type=authorization_code
    &redirect_uri=https://localhost/callback&code=d827ec7e-1b8e-3d81-a4c0-2f7ff67ce844
    &code_verifier=aYr1jbrAHhZDC5WBi8wQPdraATAvvKy93S22rkPDkkRTHzzaAMVOJ5MHgRPgoKf8xDBJPE08'
    https://localhost:9443/oauth2/token
    ```

3. Validate the ID token. For the token request, you will receive a response containing the access token, scope, and ID token. The ID token contains basic user information. To check what is encoded within the ID token, you can use a tool such as <https://devtoolzone.com/decoder/jwt>.

----
<**TODO include all the related topics here**>

# OAuth2 Demo Using Spring Security 5

## Setup Facebook SSO

* Login to facebook developers console: https://developers.facebook.com/
* Setup your developer account (option on top right after login) which ask for OTP on mobile please make sure you have setup your mobile number on facebook account
* Once account setup go to My Apps -> Create App
* mention app name and click on next
* select use case "Authenticate and request data from users with Facebook Login" and click on next
* select I don't want to connect a business portfolio yet and click on next
* click next on the next screen
* click Go to Dashboard
* on dashbard you can see your app.
* click on app and go to settings -> basic
* copy your App ID and App Secret to use it later in spring security application
* add app domain (eg. localhost)
* go to Use cases from left menu and click on Customize 
*  add permissions public_profile if already not added and all optionals that you will configure in your yml file under scope
* For this application add permissions for email as well.
* Add your client id and secret in application.yml (copied from facebook developer console client id -> App ID, client secret -> App Secret)
* now run your application and hit the url http://localhost:8080/hello
* Enter facebook login credentials
* Select Continue as <your name>
* You will be redirected to /hello page.

## Extra points in Spring Security

* Bydefault is is code grant in response_type variable value does go as code.
* Spring Security also supply a parameter with name state which it will expect in response along with code that is to protect it from crosssite request forgery attack.

Facebook SSO code grant flow

```mermaid
sequenceDiagram
    participant Browser
    participant UI
    participant FacebookSSO
    participant ApplicationAPI
    Browser->>API: Try to access welcome page <br/> http://localhost:8080/hello
    API->>FacebookSSO: Spring security Redirect to FacebookSSO URL <br/> https://www.facebook.com/v24.0/dialog/oauth?<br/>client_id={app-id}=<br/>&redirect_uri={redirect-uri}<br/>&state={state-param} 
    FacebookSSO->>FacebookSSO: Ask for facebook login
    FacebookSSO->>API: redirects on application <br/>https://www.domain.com/login?<br/>state="{st=state123abc,ds=123456789}"
    API->>FacebookSSO: API call FacebookSSO to get access token <br/> GET https://graph.facebook.com/v24.0/oauth/access_token?<br/>client_id={app-id}<br/>&redirect_uri={redirect-uri}<br/>&client_secret={app-secret}<br/>&code={code-parameter}
    FacebookSSO->>API: return access token <br/> {"access_token": {access-token}, <br/> "token_type": {type},<br/>"expires_in":  {seconds-til-expiration}}
    API->>FacebookSSO: API call to get user info <br/> GET https://graph.facebook.com/me?<br/>fields=id,name,email<br/>&access_token={access-token}
    API->>UI: forward request to UI setting access token <br/> either on chrome local <br/> store or in http session
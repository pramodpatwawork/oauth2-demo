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
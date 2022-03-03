# Signicat Documentation
## Normalization Scripts
1.) Under the  “Native Consoles” tab open “Access Management” <br />
2.) Select the Realm > Scripts > +New Script <br />
3.) Name the Script(i.e “Signicat Profile Normalization”) > Script Type > Social Identity Profile Transformation <br />
4.) Set language to “Groovy” <br />
5.) Paste the following code snippet into the Script field: <br /> 
                  
        import static org.forgerock.json.JsonValue.field
        import static org.forgerock.json.JsonValue.json
        import static org.forgerock.json.JsonValue.object

        return json(object(
        field("id", rawProfile.sub),
        field("displayName", rawProfile.name),
        field("givenName", rawProfile.given_name),
        field("familyName", rawProfile.family_name ),
        field("fr-attr-str1", rawProfile.signicat.national_id),
        field("email", rawProfile.email),
        field("username", rawProfile.email)))

6.) Create another script titled "Normalized Profile to Managed User". This script will convert a normalized social profile into a managed user. Select "Social Identity Provider Profile Transformation" for  Script Type and "Groovy" for language. Copy & paste the following code snippet into the Script field: 

      import static org.forgerock.json.JsonValue.field
      import static org.forgerock.json.JsonValue.json
      import static org.forgerock.json.JsonValue.object

      import org.forgerock.json.JsonValue

      JsonValue managedUser = json(object())

      if (normalizedProfile.username.isNotNull()) managedUser.put("userName", normalizedProfile.username)
      if (normalizedProfile.familyName.isNotNull()) managedUser.put("sn", normalizedProfile.familyName)
      if (normalizedProfile.givenName.isNotNull()) managedUser.put("givenName", normalizedProfile.givenName)
      if (normalizedProfile.email.isNotNull()) managedUser.put("mail", normalizedProfile.email)
      if (normalizedProfile.postalAddress.isNotNull()) managedUser.put("postalAddress", normalizedProfile.postalAddress)
      if (normalizedProfile.addressLocality.isNotNull()) managedUser.put("city", normalizedProfile.addressLocality)
      if (normalizedProfile.addressRegion.isNotNull()) managedUser.put("stateProvince", normalizedProfile.addressRegion)
      if (normalizedProfile.postalCode.isNotNull()) managedUser.put("postalCode", normalizedProfile.postalCode)
      if (normalizedProfile.country.isNotNull()) managedUser.put("country", normalizedProfile.country)
      if (normalizedProfile.phone.isNotNull()) managedUser.put("telephoneNumber", normalizedProfile.phone)

      return managedUser



## Configure idPs
Services > Social Identity Provider Service > Secondary Configurations > Add a Secondary Configuration > Select “Client configuration for provider that implement   
Open ID Connect specifications” Fill out the fields within “New oidcConfig configuration” according to the following. If not indicated, leave the field blank. 
                             
### 1.) Norwegian BankID | Market Coverage: Norway
- **Fill out the following fields:**<br />
**Auth ID Key:** sub <br />
**Client ID:** TBD(to be inserted per customer once customer has signed contract) <br />
**Authentication Endpoint URL:** https://eu01.signicat.com/oidc/authorize <br />
**Access Token Endpoint URL:** https://eu01.signicat.com/oidc/token <br />
**User Profile Service URL:** https://eu01.signicat.com/oidc/userinfo <br />
**Redirect URL:** TBD(to be inserted per customer once customer has signed contract) <br />
**Scope Delimiter:** Put a single space (“ “) <br />
**OAuth Scopes:** openid, profile, signicat.national_id make sure that these items are not space nor comma separated, but added as separate items. <br />
**ACR Values:** urn:signicat:oidc:method:nbid <br />
**Well Known Endpoint:** https://eu01.signicat.com/oidc/.well-known/openid-configuration <br />
**Issuer:** https://eu01.signicat.com/oidc  <br />
**JWKS URI Endpoint:** https://eu01.signicat.com/oidc/jwks.json <br />

- **UI Config Properties:** <br /> Create the following keys and corresponding values:<br />
**buttonImage:** TBD <br />
**buttonDisplayName:** NorwegianBankID <br />
**buttonCustomStyleHolder:** color: #6d6d6d; background-color: #eee;  border-color: #ccc; <br />
![](images/pic1.png)
**Transform Script:** Select the Normalization script created in Step 1 <br />
Click “Create” <br /> 

- **Once configuration is created, additional fields will be added. Fill out the following:**<br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />


### 2.) Norwegian BankID Mobile | Market Coverage: Norway
- **Fill out the following fields:**<br />
Auth ID Key: sub <br /> Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL:https://eu01.signicat.com/oidc/authorize <br /> 
Access Token Endpoint URL: https://eu01.signicat.com/oidc/token <br /> 
User Profile Service URL: https://eu01.signicat.com/oidc/userinfo <br /> 
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes: openid, profile, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values: urn:signicat:oidc:method:nbid-mobil <br />
Well Known Endpoint: https://eu01.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer: https://eu01.signicat.com/oidc <br /> 
JWKS URI Endpoint: https://eu01.signicat.com/oidc/jwks.json <br /> 
- **UI Config Properties:** <br />
Create following keys and corresponding values: <br />
buttonImage >  <br />
buttonDisplayName > Norwegian BankID Mobile <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: #eee;  border-color: #ccc; <br />
Transform Script: Select the script created in Step 1 <br />
Click “Create” <br /> 

- **Once configuration is created, additional fields will be added. Fill out the following:** <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract)<br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />

### 3.) iDIN | Market Coverage: The Netherlands 
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL: https://eu01.signicat.com/oidc/authorize <br />
Access Token Endpoint URL: https://eu01.signicat.com/oidc/token <br /> 
User Profile Service URL: https://eu01.signicat.com/oidc/userinfo <br /> 
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br /> 
OAuth Scopes: openid, profile, signicat.idin, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values: urn:signicat:oidc:method:idin-ident <br />
Well Known Endpoint: https://eu01.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer: https://eu01.signicat.com/oidc <br /> 
JWKS URI Endpoint: https://eu01.signicat.com/oidc/jwks.json <br /> 
UI Config Properties: <br />
- Create following keys and corresponding values: <br />
buttonImage > TBD <br />
buttonDisplayName > iDIN <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the Normalization script created in Step 1 <br />
Click “Create” <br /> 

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />

### 4.) Swedish BankID | Market Coverage: Sweden 
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL: https://eu01.signicat.com/oidc/authorize <br /> 
Access Token Endpoint URL: https://eu01.signicat.com/oidc/token <br /> 
User Profile Service URL: https://eu01.signicat.com/oidc/userinfo <br /> 
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:openid, profile, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values:urn:signicat:oidc:method:sbid <br />
Well Known Endpoint: https://eu01.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer: https://eu01.signicat.com/oidc <br /> 
JWKS URI Endpoint: https://eu01.signicat.com/oidc/jwks.json <br /> 
- UI Config Properties: <br />
Create following keys and corresponding values: <br />
buttonImage > TBD <br />
buttonDisplayName > Swedish BankID <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the Normalization script created in Step 1 <br />
Click “Create” <br /> 

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled  <br />
User Info Response Format: JSON <br />
	
### 5.) MitID | Market Coverage: Denmark
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL:https://id.signicat.com/oidc/authorize <br /> 
Access Token Endpoint URL:https://id.signicat.com/oidc/token <br /> 
User Profile Service URL:https://id.signicat.com/oidc/userinfo <br />
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:openid, profile, mitid, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values: urn:signicat:oidc:method:mitid-cpr <br />
Well Known Endpoint: https://id.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer: https://id.signicat.com/oidc <br /> 
JWKS URI Endpoint:https://id.signicat.com/oidc/jwks.json <br /> 
- UI Config Properties: <br />
      Create following keys and corresponding values:
            buttonImage > TBD <br />
            buttonDisplayName > MitID <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: #eee;  border-color: #ccc; <br />
Transform Script: Select the Normalization script created in Step 1 <br />
Click “Create” <br /> 

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract)  <br />
Client Authentication Method: CLIENT_SECRET_BASIC  <br />
PKCE Method: S256  <br />
Enable Native Nonce: Enabled  <br />
User Info Response Format: JSON  <br />

### 6.) NemID | Market Coverage: Denmark
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL:https://id.signicat.com/oidc/authorize <br /> 
Access Token Endpoint URL:https://id.signicat.com/oidc/token <br /> 
User Profile Service URL: https://id.signicat.com/oidc/userinfo <br />
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:openid, profile, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values:urn:signicat:oidc:method:nemid <br />
Well Known Endpoint: https://id.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer:https://id.signicat.com/oidc <br /> 
JWKS URI Endpoint:https://id.signicat.com/oidc/jwks.json <br /> 
- UI Config Properties: <br />
Create following keys and corresponding values: <br />
buttonImage > TBD <br />
buttonDisplayName > NemID <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the Normalization script created in Step 1 <br />
Click “Create” <br /> 

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />

### 7.) FTN | Market Coverage: Finland
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL:https://id.signicat.com/oidc/authorize <br />
Access Token Endpoint URL:https://id.signicat.com/oidc/token <br />
User Profile Service URL: https://id.signicat.com/oidc/userinfo <br />
Redirect URL: TBD(to be inserted per customer once customer has signed contract)  <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:openid, profile, signicat.national_id make sure that these items are not space or comma separated, but added as separate items. <br />
ACR Values:urn:signicat:oidc:method:  <br />
Well Known Endpoint: https://id.signicat.com/oidc/.well-known/openid-configuration  <br />
Issuer:https://id.signicat.com/oidc  <br />
JWKS URI Endpoint:https://id.signicat.com/oidc/jwks.json <br />
- UI Config Properties: <br />
Create following keys and corresponding values:
buttonImage  <br />
buttonDisplayName > FTN <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the Normalization script created in Step 1 <br />
Click “Create”  <br />

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />

### 8.) itsme | Market Coverage: Belgium, The Netherlands
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL: https://eu01.signicat.com/oidc/authorize <br /> 
Access Token Endpoint URL: https://eu01.signicat.com/oidc/token <br /> 
User Profile Service URL: https://eu01.signicat.com/oidc/userinfo <br /> 
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:openid, profile, signicat.itsme, signicat.national_id, address make sure that these items are not space or comma separated, but added as separate items. <br /> 
ACR Values:urn:signicat:oidc:method:itsme-register <br />
Well Known Endpoint: https://eu01.signicat.com/oidc/.well-known/openid-configuration <br />
Issuer: https://eu01.signicat.com/oidc <br /> 
JWKS URI Endpoint: https://eu01.signicat.com/oidc/jwks.json <br /> 
- UI Config Properties: <br />
Create following keys and corresponding values: <br />
buttonImage > TBD <br />
buttonDisplayName > itsme <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the script created in Step 1 <br />
Click “Create” <br /> 

- Once configuration is created, additional fields will be added. Fill out the following: <br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />
	
### 9.) eHerkenning | Market Coverage: The Netherlands
- **Fill out the following fields:**<br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL: <br />
Access Token Endpoint URL: <br />
User Profile Service URL: <br />
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:idp_scoping:simulator make sure that these items are not space or comma separated, but added as separate items. <br />
ACR Values:N/A (scope controlled) <br />
Well Known Endpoint: TBD(to be inserted per customer once customer has signed contract) <br />
Issuer: <br />
JWKS URI Endpoint:<br />

- UI Config Properties:<br />
Create following keys and corresponding values:<br />
buttonImage > TBD<br />
buttonDisplayName > EHerkenning
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc;<br />
Transform Script: Select the script created in Step 1<br />
Click “Create” <br />

- Once configuration is created, additional fields will be added. Fill out the following:<br /> 
Client Secret: TBD(to be inserted per customer once customer has signed contract)
Client Authentication Method: CLIENT_SECRET_BASIC
PKCE Method: S256
Enable Native Nonce: Enabled
User Info Response Format: JSON

### 10.) DigID | Market Coverage: The Netherlands 
- **Fill out the following fields:** <br />
Auth ID Key: sub <br />
Client ID: TBD(to be inserted per customer once customer has signed contract) <br />
Authentication Endpoint URL: <br />
Access Token Endpoint URL: <br />
User Profile Service URL: <br />
Redirect URL: TBD(to be inserted per customer once customer has signed contract) <br />
Scope Delimiter: Put a single space (“ “) <br />
OAuth Scopes:idp_scoping:simulator make sure that these items are not space or comma separated, but added as separate items.  <br />
ACR Values: N/A (scope controlled)  <br />
Well Known Endpoint: TBD(to be inserted per customer once customer has signed contract) <br />
Issuer: <br />
JWKS URI Endpoint: <br />

- UI Config Properties: <br />
Create following keys and corresponding values:  <br />
buttonImage > TBD <br />
buttonDisplayName > DigID <br />
buttonCustomStyleHolder > color: #6d6d6d; background-color: 
#eee;  border-color: #ccc; <br />
Transform Script: Select the script created in Step 1 <br />
Click “Create”  <br />

- Once configuration is created, additional fields will be added. Fill out the following:  <br />
Client Secret: TBD(to be inserted per customer once customer has signed contract) <br />
Client Authentication Method: CLIENT_SECRET_BASIC <br />
PKCE Method: S256 <br />
Enable Native Nonce: Enabled <br />
User Info Response Format: JSON <br />


# Authentication Tree
In order to configure the Authentication tree, go to “Journeys” within the Platform. 

### Recreate the Authentication Tree below: <br />
![](images/pic2.png)
Configure each node’s settings <br />

### Page Node: <br />
Platform Password > Password Attribute > password <br />
Platform Username > Username Attribute > userName <br />
Select Identity Provider > Select “Include Local Authentication” <br />
Password Attribute > password <br /> 
Identity Attribute > mail <br />

### Social Provider Handler Node: <br />
Transformation Script > Normalized Profile to Managed User <br />
Username Attribute > userName <br />
Client Type > BROWSER <br />

### Required Attributes Present: <br /> 
Identity Resource > Must match identity resource of the current tree. <br />

### Page Node: <br /> 
Attribute Collector > Attribute to Collect > givenName, sn, mail (make sure that these items are not space or comma separated, but added as separate items). <br /> 
Select “All Attributes Required” and “Validate Input” <br /> 
Identity Attribute > userName  

# Testing
For testing this flow, paste the Preview URL into an address string in an incognito window. If you have an Entry account, you will be redirected back after you have logged in.
All users in your Workspace should be able to log in to your ForgeRock instance.
Once configured the idPs should look like: <br />
<img src="./images/pic3.png" width="340" height="400" />


## idP Specific Test User Information/Instructions:

### 1.) Norwegian BankID
Test user guide: https://developer.signicat.com/enterprise/identity-methods/norwegian-bankid.html#test-information

In the above overview, there are some static test users which can be used. There is also a webpage (https://ra-preprod.bankidnorge.no/#/search/endUser) where one can create additional test users, by going to "test number generator", press "generate number", and then enter a name and a BankID friendly name (can be whatever), and lastly "order". Test users created here all have user ID equal to the generated test number, one time code equal to "otp", and personal password equal to "qwer1234". On the authorize URL at Signicat, one simply enters the generated national ID number ("user ID"), then the one time code ("otp") and lastly the personal password ("qwer1234") when prompted for it.

### 2.) Norwegian BankID Mobile
Contact support@signicat.com to order (physical) test SIM cards. These will be shipped per mail (usually takes 1-2 weeks from they are ordered until they arrive at the destination). Once the SIM card is received, it can be installed in a mobile device. Each SIM card has a Norwegian phone number, a date of birth (user identifier) and a 4-digit PIN code attached to it (these credentials are on a printed letter in the SIM card shipment). In order to perform the identification, simply enter the test phone number and the test date of birth in the mobile BankID dialog (on the authorize URL to Signicat), and then enter a 4-digit PIN code in the dialog that automatically will pop up on the mobile device (no app installation required).

### 3.) iDIN
In the eID view (on the authorize URL), select bank "Rabobank iDIN issuer simulatie", which will then emulate a static test response and proceed to redirect URL with an authorization response code. 
	
### 4.) Swedish BankID

1. Download the Swedish BankID app: https://play.google.com/store/apps/details?id=com.bankid.bus&hl=sv&gl=US  or https://apps.apple.com/se/app/bankid-s%C3%A4kerhetsapp/id433151512 
2. Create a valid, yet fake, Swedish national ID number. For instance personnummer.nu can give you such a number based on an input date of birth in the format YY-MM-DD: https://www.personnummer.nu/
3. Map the created national ID number created in the previous step, to the required BankID format. The national ID number generated in the previous step is on the format YYMMDD-XXXX, whereas the required format by BankID is YYYYMMDDXXXX.
4. Go to https://demo.bankid.com/, “Generate code” to order personal code, get a code per email, and then log into demo.bankid.com using that code
5. Once logged into demo.bankid.com, click “issue BankID for test”, enter the (mapped) national ID number you created and mapped under steps 2) and 3) above, and then also follow their configuration guide there for how to configure the app for testing. This requires opening the app and scanning an activation QR code.
 
On the authorize URL at Signicat, select "mobile", then enter the created national ID number in the required format (YYYYMMDDXXXX), and open the mobile BankID app. In the app, you will be prompted for either the selected PIN code (as per activation guide above) or for a biometric authentication on the mobile device (if activated in the app).

### 5.) MitID
Test users can be created and used from this page: 

https://pp.mitid.dk/test-tool/frontend/#/create-identity 

A further explanation of how to use this tool follows below:

In order to create a new test user, one can fill out custom data, or have some data autofilled. We recommend using the autofill function: click the button "autofill" without altering any of the filled-in options on the page, and wait a few seconds (it hangs a bit before the data are filled out in the form). After a few seconds, a username should appear in the "identity claim" field in the form, and some other data should also have been filled further down in the form. Next, click the "create identity" button next to the "autofill" button. This is what persists the newly filled in test user data. This operation also "hangs" a few seconds, before one is redirected to a landing page with an overview of the newly created user. The "identity claim"/username is used for later lookup on this webpage, when you want to apply the test user. Store the "identity claim"/username (as well as other on this page somewhere for later use). 

 to the test page at https://pp.mitid.dk/test-tool/frontend/#/view-identity and click "open simulator" for the test user you want to apply. If there is no test user loaded already on the webpage, then you can use the search bar in the top right corner to lookup the user by entering the "identity claim" created in step 1. When you hit the authorize URL at Signicat, and the MitID dialog starts, one is prompted for the "identity claim". Enter the "identity claim"/username that was created in step 1 and click "Continue".  Then, on the test webpage, select the link "open simulator" under the "App" section below the overview of the identity attributes. This should open a popup window ("App simulator"), and once the "identity claim" is entered into the authorize URL at Signicat (the MitID dialog), then this popup window should show an option with a green button with the label "confirm", to approve the authentication transaction. Once you click "confirm", then you should on the authorize URL be redirected back to the redirect URL with a response code, and the authentication process is finished."

### 6.) NemID
Contact support@signicat.com to get static test users. Each static test user comes with 1) a national ID number ("CPR number"), 2) an alias (username), 3) a password and 4) a static OTP card with a given number of one-time codes. When performing a NemID authentication on the authorize URL at Signicat, both the alias, the password and an OTP code are required (which exact OTP code to use is randomly selected in the dialog after entering alias+password).

### 7.) FTN
TBD
### 8.) Itsme
Contact support@signicat.com to get a set of test users in an Excel file. Each test user needs to be activated in a "testflight" app downloaded to a unique device. Instructions for how to download app, and activate test users follow below:

Download/installation:

To download/update E2E itsme App, please use one of the 2 links depending on your smartphone OS:
· itsme E2E - Android: https://install.appcenter.ms/orgs/bmid/apps/itsme-e2eandroid/
distribution_groups/partners%20e2e 
· itsme E2E - iOS: https://install.appcenter.ms/orgs/bmid/apps/itsme-e2eios/
distribution_groups/partners%20e2e
(please open these links directly on the dedicated mobile test device, in order to download the app)

For iPhone users, you need to trust the Belgian Mobile ID NV issuer (Go to Settings/General/Profiles &
Device Management)

Enrollment: 

Enrollment must be done via MyBank on a PC/Mac (use this link: https://e2emybank.labo.sixdots.be/ )
Click on Aanmelden. Input the card number (cf. test account file you get from support@signicat.com), click on Aanmelden (not Openid or itsme - Login), click on "Create itsme account". Then, enter the test phone number (cf. test account file that you get from support@signicat.com). Then, click until you get an identification token (not a Response Code). Then, open the E2E mobile app on the test device. Lastly, enter the same test phone number (cf. test account file that you get from support@signicat.com), enter the token, and you should be enrolled.

### 9.) eHerkenning
Success flow:
Edit the values you’d like to see back in the response. None of the values have an impact on the flow
Press SEND
Legend:
ServiceID: the service in the service catalog that has been used to log in
Pseudo: specific and persistant ID to identify the person who has logged in. Use this ID to connect to your own AIM solution.
KvK number: The Chamber of Commerce number of the company for which has been logged in
Intermediate KvK number: The Chamber of Commerce number of the represented service buyer/intermediary – optional flow type in eHerkenning
RSIN: alternative number to KvK, used to identify sole proprietorships

Please ignore BSN, eHerkenning hasn’t been designed for this attribute. <br />
<img src="./images/pic4.png" width="360" height="350" />


### 10.) DigID 
Select “DigID” in simulator
Fill in a value in the field BSN
Select the Level of Assurance that the authentication response should use
Press SEND
For testing purposes these values should be prepopulated. For further technical specifications, please see: https://www.logius.nl/sites/default/files/public/bestanden/diensten/DigiD/Koppelvlakspecificatie-SAML-DigiD.pdf
Signicat’s DigiD knowledgebase: https://documentation.signicat.nl/knowledgebase/about-digid 
More information about BSN: https://www.belastingdienst.nl/wps/wcm/connect/en/individuals/content/what-is-a-citizen-service-number  <br />
<img src="./images/pic5.png" width="380" height="300" />


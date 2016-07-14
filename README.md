# Master Maven plugin for adding Squencing.com's Real-Time Personalization technology to Android and Java apps
=========================================
This Master Maven plugin can be used to quickly add [Real-Time Personalization](https://sequencing.com/developer-documentation/what-is-real-time-personalization-rtp) to your app. This Master Plugin contains a customizable, end-to-end plug-n-play solution that quickly adds all necessary code (OAuth2, File Selector and App Chain coding) to your app.

Once this Master Plugin is added to your app all you'll need to do is:

1. add your [OAuth2 secret](https://sequencing.com/developer-center/new-app-oauth-secret)
2. add one or more [App Chain numbers](https://sequencing.com/app-chains/)
3. configure your app based on each [app chain's possible responses](https://sequencing.com/app-chains/)

To code Real-Time Personalization technology into apps, developers may [register for a free account](https://sequencing.com/user/register/) at Sequencing.com. App development with RTP is always free.

**The Master Plugin is also available in the following languages:**
* Android (Maven plugin) <-- this repo
* Java (Maven plugin)] <-- this repo
* [Objective-C (CocoaPod plugin)](https://github.com/SequencingDOTcom/CocoaPods-iOS-Master-Plugin-ObjectiveC)
* [Swift (CocoaPod plugin)](https://github.com/SequencingDOTcom/CocoaPods-iOS-Master-Plugin-Swift)

**Master Plugin - Plug-n-Play Samples** (These sample repos can be used as an out-of-the-box solution to quickly add Real-Time Personalization technology into your app):
* [Android (Maven) Plug-n-Play Sample](https://github.com/SequencingDOTcom/Android-Master-Plugin-Plug-N-Play-Sample)
* [Objective-C (CocoaPod) Plug-n-Play Sample](https://github.com/SequencingDOTcom/iOS-Objective-C-Master-Plugin-Plug-n-Play-Sample)

Contents
=========================================
* Implementation
* Master Gradle plugin integration
* OAuth Gradle plugin integration
* File selector Gradle plugin integration
* App chains
* Authentication flow
* Steps
* Resources
* Maintainers
* Contribute

Implementation
======================================
To implement this Master Plugin for your app:

1) [Register](https://sequencing.com/user/register/) for a free account

2) Add this Master Plugin to your app

3) [Generate an OAuth2 secret](https://sequencing.com/api-secret-generator) and insert the secret into the plugin

4) Add one or more [App Chain numbers](https://sequencing.com/app-chains/). The App Chain will provide genetic-based information you can use to personalize your app.

5) Configure your app based on each [app chain's possible responses](https://sequencing.com/app-chains/)

Master Gradle plugin integration
======================================

* see [gradle guides](https://docs.gradle.org/current/userguide/artifact_dependencies_tutorial.html) 
* include dependency into build.gradle file in dependencies section. Here is dependency declaration example:
	```
	dependencies {
   		compile 'com.sequencing:master-plugin:1.0.28' 
    }
	```
* integrate OAuth Gradle plugin
* integrate File selector Gradle plugin
* integrate AppChains Gradle plugin

OAuth Gradle plugin integration
======================================

You need to follow instructions below if you want to build in and use OAuth logic in your existing or new project.

* create a new Android Gradle based project (i.e. in Android Studio or Eclipse)
* add gradle dependency
	* see [gradle guides](https://docs.gradle.org/current/userguide/artifact_dependencies_tutorial.html) 
	* add dependency into build.gradle file in dependencies section. Here is dependency declaration example:
	```
	dependencies {
   		compile 'com.sequencing:android-oauth:1.0.18' 
    }
	```
* integrate autherization functionality
	* add imports
	```java
    import com.sequencing.androidoauth.core.OAuth2Parameters;
	import com.sequencing.androidoauth.core.ISQAuthCallback;
	import com.sequencing.androidoauth.core.SQUIoAuthHandler;
	import com.sequencing.oauth.core.Token;
    ```
    * for authorization you need to specify your application parameters at [AuthenticationParameters](https://github.com/SequencingDOTcom/Maven-OAuth-Java/blob/master/src/main/java/com/sequencing/oauth/config/AuthenticationParameters.java). Authorization plugin use custom url schema which you should define. For example:
    ```java
    AuthenticationParameters parameters = new AuthenticationParameters.ConfigurationBuilder()
                .withRedirectUri("[your custom url schema]/Default/Authcallback")
                .withClientId("[your client id]")
                .withClientSecret("[your client secret]")
                .build();
    ```
    * implement [ISQAuthCallback](https://github.com/SequencingDOTcom/Maven-Android-OAuth-Java/blob/master/src/main/java/com/sequencing/androidoauth/core/ISQAuthCallback.java).
    ```java
     /**
     * Callback for handling success authentication
     * @param token token of success authentication
     */
    void onAuthentication(Token token);

    /**
     * Callback of handling failure authentication
     * @param e exception of failure
     */
    void onFailedAuthentication(Exception e);
    ```
    * create View that will serve as initial element for authentication flow. It can be a Button or an extension of View class. Do not define onClickListener for this View.
    * create [SQUIoAuthHandler](https://github.com/SequencingDOTcom/Maven-Android-OAuth-Java/blob/master/src/main/java/com/sequencing/androidoauth/core/SQUIoAuthHandler.java) instance that is handling authentication process
    * register your authentication handler by invoking ```authenticate``` method with view, callback and app configuration
    
    ```java
	public void authenticate(View viewLogin, final ISQAuthCallback authCallback, AuthenticationParameters parameters);
    ```



File selector Gradle plugin integration
======================================

You need to follow instructions below if you want to build in and use OAuth logic in your existing or new project.
* create a new Android Gradle based project (i.e. in Android Studio or Eclipse)
* File selector module prepared as separate module, but it depends on a [SequencingOAuth2Client](https://github.com/SequencingDOTcom/Maven-OAuth-Java/blob/master/src/main/java/com/sequencing/oauth/core/SequencingOAuth2Client.java) instance from oAuth module. File selector can execute request to server for files with [SequencingOAuth2Client](https://github.com/SequencingDOTcom/Maven-OAuth-Java/blob/master/src/main/java/com/sequencing/oauth/core/SequencingOAuth2Client.java) instance only. Thus you need 2 modules to be installed: oAuth module and File Selector module
* add gradle dependency
	* see [gradle guides](https://docs.gradle.org/current/userguide/artifact_dependencies_tutorial.html) 
	* include the listed below dependency into build.gradle file in dependencies section. Here is dependency declaration example:
	```
	dependencies {
   		compile 'com.sequencing:file-selector:1.0.26'
    }
	```
* integrate autherization functionality
	* add imports
	```
    import com.sequencing.androidoauth.core.OAuth2Parameters;
    import com.sequencing.fileselector.core.ISQFileCallback;
    import com.sequencing.fileselector.core.SQUIFileSelectHandler;
    import com.sequencing.oauth.core.Token;
    ```
* oAuth Gradle plugin. See reference: [Maven-Android-OAuth-Java](https://github.com/SequencingDOTcom/Maven-Android-OAuth-Java)
* integrate file selector functionality
	* implement [ISQFileCallback](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/core/ISQFileCallback.java)
	```
     /**
     * Callback for handling selected file
     * @param entity selected file entity
     * @param activity activity of file selector
     */
    void onFileSelected(FileEntity entity, Activity activity);
	```
	* create [SQUIFileSelectHandler](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/core/SQUIFileSelectHandler.java) instance that is handling file selection process
	* register your file selection handler by invoking ```selectFile``` method 
	* when you invoke ```selectFile``` method will be started file selector UI
	* when user selects any file and clics on "Continue" button in UI will be invoked user [ISQFileCallback](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/core/ISQFileCallback.java) implementation and passed to him [FileEntity](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/FileEntity.java) object and current file selector activity
	* each file is represented as [FileEntity](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/FileEntity.java) object with following keys and values format:
	
      key name | type | description
      ------------- | ------------- | ------------- 
      DateAdded | String | date file was added
      Ext | String | file extension
      FileCategory | String | file category: Community, Uploaded, FromApps, Altruist
      FileSubType | String | file subtype
      FileType | String | file type
      FriendlyDesc1 | String | person name for sample files
      FriendlyDesc2 | String | person description for sample files
      Id | String | file ID
      Name | String | file name
      Population | String | 
      Sex | String |	the sex

* examples 
	* example of File selector UI
	
	![my files](https://raw.githubusercontent.com/SequencingDOTcom/Maven-Android-File-Selector-Java/master/Screenshots/file_selector_my_files.png)	

	* example of ```Select File``` button
	
	```Button btnFileSelector = (Button)findViewById(R.id.btnFileSelector);```
    * example of [SQUIFileSelectHandler](https://github.com/SequencingDOTcom/Maven-Android-File-Selector-Java/blob/master/src/main/java/com/sequencing/fileselector/core/SQUIFileSelectHandler.java) initialization

	```SQUIFileSelectHandler fileSelectHandler = new SQUIFileSelectHandler(this);```
    * example of ```selectFile``` method

	```
	fileSelectHandler.selectFile(OAuth2Parameters.getInstance().getOauth(), new ISQFileCallback() {

                    @Override
                    public void onFileSelected(FileEntity entity, Activity activity) {
                        Log.i(TAG, "File has been selected");

                        Toast toast = Toast.makeText(getApplicationContext(), entity.toString(), Toast.LENGTH_LONG);
                        toast.show();
                    }
                }, true, null);
	```


App chains
======================================
Search and find app chains -> https://sequencing.com/app-chains/

Each app chain is composed of 
* an **API request** to Sequencing.com
 * this request is secured using oAuth2
* analysis of the app user's genes
 * each app chain analyzes a specific trait or condition
 * there are thousands of app chains to choose from
 * all analysis occurs in real-time at Sequencing.com
* an **API response** to your app
 * the information provided by the response allows your app to tailor itself to the app user based on the user's genes.
 * the documentation for each app chain provides a list of all possible API responses. The response for most app chains are simply 'Yes' or 'No'.

Example
* App Chain: It is very important for this person's health to apply sunscreen with SPF +30 whenever it is sunny or even partly sunny.
* Possible responses: Yes, No, Insufficient Data, Error

While there are already app chains to personalize most apps, if you need something but don't see an app chain for it, tell us! (ie email us: gittaca@sequencing.com).

Authentication flow
======================================
Sequencing.com uses standard OAuth approach which enables applications to obtain limited access to user accounts on an HTTP service from 3rd party applications without exposing the user's password. OAuth acts as an intermediary on behalf of the end user, providing the service with an access token that authorizes specific account information to be shared.

![Authentication sequence diagram]
(https://github.com/SequencingDOTcom/oAuth2-code-and-demo/blob/master/screenshots/oauth_activity.png)


## Steps

### Step 1: Authorization Code Link

First, the user is given an authorization code link that looks like the following:

```
https://sequencing.com/oauth2/authorize?redirect_uri=REDIRECT_URL&response_type=code&state=STATE&client_id=CLIENT_ID&scope=SCOPES
```

Here is an explanation of the link components:

* https://sequencing.com/oauth2/authorize: the API authorization endpoint
* client_id=CLIENT_ID: the application's client ID (how the API identifies the application)
* redirect_uri=REDIRECT_URL: where the service redirects the user-agent after an authorization code is granted
* response_type=code: specifies that your application is requesting an authorization code grant
* scope=CODES: specifies the level of access that the application is requesting

![login dialog](https://github.com/SequencingDOTcom/oAuth2-code-and-demo/blob/master/screenshots/oauth_auth.png)

### Step 2: User Authorizes Application

When the user clicks the link, they must first log in to the service, to authenticate their identity (unless they are already logged in). Then they will be prompted by the service to authorize or deny the application access to their account. Here is an example authorize application prompt

![grant dialog](https://github.com/SequencingDOTcom/oAuth2-code-and-demo/blob/master/screenshots/oauth_grant.png)

### Step 3: Application Receives Authorization Code

If the user clicks "Authorize Application", the service redirects the user-agent to the application redirect URI, which was specified during the client registration, along with an authorization code. The redirect would look something like this (assuming the application is "php-oauth-demo.sequencing.com"):

```
https://php-oauth-demo.sequencing.com/index.php?code=AUTHORIZATION_CODE
```

### Step 4: Application Requests Access Token

The application requests an access token from the API, by passing the authorization code along with authentication details, including the client secret, to the API token endpoint. Here is an example POST request to Sequencing.com token endpoint:

```
https://sequencing.com/oauth2/token
```

Following POST parameters have to be sent

* grant_type='authorization_code'
* code=AUTHORIZATION_CODE (where AUTHORIZATION_CODE is a code acquired in a "code" parameter in the result of redirect from sequencing.com)
* redirect_uri=REDIRECT_URL (where REDIRECT_URL is the same URL as the one used in step 1)

### Step 5: Application Receives Access Token

If the authorization is valid, the API will send a JSON response containing the access token  to the application.

Resources
======================================
* [App chains](https://sequencing.com/app-chains)
* [File selector code](https://github.com/SequencingDOTcom/File-Selector-code)
* [Generate OAuth2 secret](https://sequencing.com/developer-center/new-app-oauth-secret)
* [Developer Center](https://sequencing.com/developer-center)
* [Developer Documentation](https://sequencing.com/developer-documentation/)

Maintainers
======================================
This repo is actively maintained by [Sequencing.com](https://sequencing.com/). Email the Sequencing.com bioinformatics team at gittaca@sequencing.com if you require any more information or just to say hola.

Contribute
======================================
We encourage you to passionately fork us. If interested in updating the master branch, please send us a pull request. If the changes contribute positively, we'll let it ride.

# Ionic-Cloudant-FeedbackApp 
An Ionic feedback app using Cloudant NoSQL service on IBM Bluemix. An easy to configure mobile app for receiving feedback at Meetups, Events etc.,

**Ionic** is a complete open-source SDK for hybrid mobile app development. Built on top of AngularJS and Apache Cordova, Ionic provides tools and services for developing hybrid mobile apps using Web technologies like CSS, HTML5, and Sass.

**Cloudant** is the distributed database as a service (DBaaS) built from the ground up to deliver fast-growing application data to the edge.

The App Runs on iOS and Android and build using **Ionic Version 1.7.13**

<strong>iOS:</strong>
<p align="center">
<img src="https://github.com/VidyasagarMSC/Ionic-Cloudant-FeedbackApp/blob/master/images/Simulator%20Screen%20Shot%2028-Nov-2016%2C%203.42.45%20PM.png" width="350">
<img src="https://github.com/VidyasagarMSC/Ionic-Cloudant-FeedbackApp/blob/master/images/Simulator%20Screen%20Shot%2028-Nov-2016%2C%203.43.08%20PM.png" width="350">
</p>

<strong>Android:</strong>
<p align="center">
<img src="https://github.com/VidyasagarMSC/Ionic-Cloudant-FeedbackApp/blob/master/images/Screenshot_20160616-095752.png" width="350">
<img src="https://github.com/VidyasagarMSC/Ionic-Cloudant-FeedbackApp/blob/master/images/Screenshot_20160616-095800.png" width="350">
</p>



## Creating a Cloudant NoSQL DB service on IBM Bluemix 

<ul>
<li>Don’t have Bluemix account? <a target="_blank" href="https://console.ng.bluemix.net/registration/?target=/catalog/services/conversation/" title="(Opens in a new tab or window)">Sign up</a> to create a free trial account.</li>
<li>Have a Bluemix account? Use <a target="_blank" href="https://console.ng.bluemix.net/catalog/services/cloudant-nosql-db" title="(Opens in a new tab or window)">this link</a>.</li>
</ul>

 Add a new Cloudant data service in just a few clicks:

1. Visit your Bluemix dashboard.
2. Click Catalog.
3. On the left Pane, Click on **Data & Analytics** under Services.
4. Click **Cloudant NoSQL DB** tile.
5. Enter a unique descriptive name in the Service name field.
6. Check Features, Images and Pricing Plans.
7. Click the **Create** button.

### Cloudant Dashboard 2.0
Once the Cloudant service is created, 

* Click on ***LAUNCH*** button to launch the Cloudant Dashboard 2.0 (Powerful querying, analytics, replication, and syncing happens here) on a seperate tab 
* Create a new database by clicking on **Create Database** on the top ribbon. Your database is created.
* From the left Pane, Click on **Account** -> CORS Tab -> Check **All domains ( * )**. *Not recommended for all usecases, this being a simple mobile app taking this liberty. [CORS Documentation](https://docs.cloudant.com/cors.html)

## Configuring Ionic app with a configuration file

* Install Ionic 
```
npm install -g cordova ionic@1.7.13
```
* Clone the repo 
```
$ git clone https://github.com/VidyasagarMSC/Ionic-Cloudant-FeedbackApp.git
```
* Open the unzipped folder in an IDE (I use Brackets) of your Choice and Navigate to **www/js** folder.
* Create a new Javascript file **app.config** *With extension the file will be app.config.js*
* Paste the below code in app.config.js,
```
angular.module('app').constant('CLOUDANTDB_CONFIG', {
    baseUrl: 'https://<hostname>',
    dbName: '<DBName>',
    userName: '<username>',
    password: '<password>'
});
```
* DBName - Name of the Cloudant NoSQL DB your created on Dashboard 2.0.
* For **hostname,username and password** - Navigate to the Cloudant Service page on Bluemix and Click on **Service Credentials** tab. 
* Click on **View Credentials** under Actions.

|   placeholder	|   Cloudant Service|
|---	|---	|
|  username 	|  username 	|
|  password 	|  password 	|
|  hostname  |  host      |
  
The CLOUDANTDB_CONFIG constant values are utilised in **controllers.js**

```
// Define the Credentials string to be encoded.
    var credentialstobeEncoded = CLOUDANTDB_CONFIG.userName + ":" + CLOUDANTDB_CONFIG.password;

    // Encode the String
    var encodedString = Base64.encode(credentialstobeEncoded);
    console.log("ENCODED: " + encodedString);

    $scope.createFeedback = function (feedback) {

        $http({
            method: 'POST',
            url: CLOUDANTDB_CONFIG.baseUrl + "/" + CLOUDANTDB_CONFIG.dbName,
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Basic ' + encodedString
            },
```
## Customize the App UI

* Images can be replaced with the same name under **img** folder.
* Customize the feedback fields in **feedback.html**
* There are validations on the fields based on the type. E.g., Email checks for @ in the entry. **Submit** will be disabled until the form is completely valid.

## Testing the App

Desktop browser Testing 

```
$ ionic serve
```
On an iOS Simulator or Android Emulator 
```
$ ionic emulate ios
$ ionic emulate android
```
<strong>Note:</strong> Follow the <a href="https://cordova.apache.org/docs/en/latest/guide/platforms/android">Android</a> and <a href="https://cordova.apache.org/docs/en/latest/guide/platforms/ios">iOS</a> platform guides to install required tools for development.

<h3>If you see this Error</h3>
<pre>Error: Cannot find module 'internal/fs'

at Function.Module._resolveFilename (module.js:470:15)

at Function.Module._load (module.js:418:25)

at Module.require (module.js:498:17)

at require (internal/module.js:20:19)

at evalmachine.&lt;anonymous&gt;:18:20

at Object.&lt;anonymous&gt; (/usr/local/lib/node_modules/ionic/node_modules/ionic-app-lib/node_modules/vinyl-fs/node_modules/graceful-fs/fs.js:11:1)

at Module._compile (module.js:571:32)

at Object.Module._extensions..js (module.js:580:10)

at Module.load (module.js:488:32)

at tryModuleLoad (module.js:447:12)

Cannot find module 'internal/fs' (CLI v1.7.13)</pre>
<h3>Resolution:</h3>
<ul>
 	<li>Check Node version on your system</li>
</ul>
<pre class="">node -v</pre>
<ul>
 	<li>If you see <strong>&nbsp;v7.5.0</strong>, run the below commands one after another to add <strong>v6</strong></li>
</ul>
<pre class="">export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
nvm install 6</pre>
<h3>If you see this error</h3>
<div class="extra-info-wrapper">
<div class="extra-info two-rows">
<div class="title-wrapper">
<pre class="">Error: Cannot read property ‘replace’ of undefined</pre>
</div>
</div>
</div>
<h3>Solution:</h3>
<pre class="">cd platforms/ios/cordova/node_modules/<br>sudo npm install ios-sim@latest</pre>

*Notes*: 
* This sample uses only the POST HTTP API call of Cloudant Service. To understand other HTTP API Verbs, Refer [Cloudant Documentation](https://docs.cloudant.com/basics.html#http-api)
* [Cloudant Client Libraries](https://docs.cloudant.com/libraries.html) 

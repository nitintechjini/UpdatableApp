This Updatable app pretty much the one more implementation of the Cordova-App-Loader found on github here:
https://github.com/markmarijnissen/cordova-app-loader with improved setup procedure description for dummies like myself.
 
Settings things up and adding updatability into your project. For any CLI I use 'ionic' as example.

1. Setup web server for app updates where contents of www folder should be copied
--* For the test purposes you can install simple node.js web server from here: https://www.npmjs.com/package/http-server
2. Ensure the platforms needed has been added into your app (e.g. android)
--* ionic platform add anroid 
3. Ensure the following plugins were installed (file, whitelist and file-transfer)
--* ionic plugin add cordova-plugin-file
--* ionic plugin add cordova-plugin-file-transfer
--* ionic plugin add cordova-plugin-whitelist
4. Copy bootstrap.js, cordova-app-loader-complete.js and autoupdate.js into www folder
5. Into index.html add main cordova-app-loader script reference and it's bootstrap script reference just *after* cordova.js. 
<script src="cordova-app-loader-complete.js"></script>
<script src="bootstrap.js" timeout="5100" manifest="manifest.json" server="http://localhost:8080/"></script>
6. Change *server* attribute to point to the root of your www where is index.html will resides.
7. Copy my example of manifest.json into www folder and then update its contents to include all the files of your app.
Fix manifest as follow:
--1. Remove from Index.html references to *all the scripts and styles of your own*. 
--2. Put them into manifest.json both *files* and *load* (see this app manifest.json as example). Please, pay attention, the *load* is actually specifies the JS/CSS loading order!
From now on they be loaded by boostrap.js. You will need it just once and also when some files are added/deleted from your app.
8. Write window.BOOTSTRAP_OK = true in your code when your app succesfully launches
9. In some initialization place (1st controller?) DI 'SelfUpdateService' and call it's 'ensureUpdate()' function. This will update app as needed and also set some event handlers.
10. Add into Index.html CSP meta tag:
<meta http-equiv="Content-Security-Policy" content="default-src * 'self' cdvfile://*; style-src 'unsafe-inline' 'self' cdvfile://*; script-src 'self' 'unsafe-eval' cdvfile://*">
and into Cordova's config.xml
<access origin="*"/>
<allow-navigation href="*://*/*"/>
    
#Known problems:
* HTML files (templates) don't get updated, though downloaded ok. This is because of relational 'templateUrl' based on Index.html. Will fix it some day... :) Maybe. 
    
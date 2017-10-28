# Backendless JS-SDK Patch for [Appcelerator](http://www.appcelerator.com/)

### Patched Items:

- Add polyfill for Promises
- Add Backendless namespace to global scope
- Override Backendless.XmlHttpRequest by `Ti.Network.createHTTPClient`
- fix File Upload


## How to use:

- put this patch next to JS-SDK file
````
-|
 |-backendless.js
 |-backendless-appcelerator.js
 
````

- require the path in your code once
````
//index file of your app
...

require('.../path-to/backendless-appcelerator.js')

Backendless.initApp(...)

...
````
 

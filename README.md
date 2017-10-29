# [Backendless JS-SDK](https://github.com/Backendless/JS-SDK) Patch for [Appcelerator](http://www.appcelerator.com/)

### Patch includes:

- Polyfill for Promise
- Backendless namespace in global scope
- Override Backendless.XmlHttpRequest
- Correct File Upload


## How to use:

- put this patch next to JS-SDK file
````
-|
 |-backendless.js
 |-backendless-appcelerator.js
 
````

- include the patch to your code before using any Backendless API
````
//index.js file of your app
...

require('.../path-to/backendless-appcelerator.js')

Backendless.initApp(...)

...
````

### Polyfill for Promise
The patch includes polyfill for [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), so you can feel free to use it your code

### Backendless namespace in global scope
The patch adds Backendless namespace in global scope, so be free to use Backendless without additional `require` [Backendless JS-SDK](https://github.com/Backendless/JS-SDK), just `require` the patch at once in the top of your index file and use Backendless namespace in any files

### Override Backendless.XmlHttpRequest
The patch override `Backendless.XmlHttpRequest` by `Ti.Network.createHTTPClient`.
You don't need add this fix manually anymore.

__before:__

```
Backendless.XMLHttpRequest = function () {
  return Ti.Network.createHTTPClient();
};
```

__after:__

```
//do nothing
```

### Correct File Upload
Appcelerator ENV has little differences with Web and Nodejs environments, so the patch fix it.

Examples:
```
function uploadTextFile() {
  var textFile = Ti.Filesystem.getFile(Ti.Filesystem.tempDirectory, 'my-text-file.txt');
  textFile.write('Hello world!');

  Backendless.Files.upload(textFile, 'text-files-directory', true)
    .then(function (result) {
      Ti.API.info('fileURL: ' + result.fileURL);
    })
    .catch(function (error) {
      Ti.API.error(error);
    });
}
```

```
function uploadImgFileFromPhotoGallery() {
  Titanium.Media.openPhotoGallery({
    success: function (event) {
      var imageFile = Ti.Filesystem.getFile(Ti.Filesystem.tempDirectory, 'my-image-file.png');
      imageFile.write(event.media.imageAsResized(100, 100));

      Backendless.Files.upload(imageFile, 'img-files-directory', true)
        .then(function (fileURL) {
          Ti.API.info('fileURL: ' + JSON.stringify(fileURL));
        })
        .catch(function (error) {
          Ti.API.error(error);
        });
    }
  });
}

```

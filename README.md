# [Backendless JS-SDK](https://github.com/Backendless/JS-SDK) Patch for [Appcelerator](http://www.appcelerator.com/)

### Patch includes:

- [Polyfill for Promise](#polyfill-for-promise)
- [Backendless namespace in global scope](#backendless-namespace-in-global-scope)
- [Override Backendless.XmlHttpRequest](#override-backendlessxmlhttprequest)
- [Correct File Upload](#correct-file-upload)


## How to use:

- put this patch next to [JS-SDK file](https://github.com/Backendless/JS-SDK)
````
-|
 |-backendless.js
 |-backendless-appcelerator.js
 
````

- include the patch to your code before using any Backendless API
````
//index.js file of your app
...

require('.../path-to/backendless-appcelerator')

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


#### Below are a few links to help with the development:

<pre><code>Backendless Documentation: <a href="https://backendless.com/products/documentation/">https://backendless.com/products/documentation/</a>
Support Home:    <a href="slack.backendless.com">slack.backendless.com</a>
Slack Channel:   <a href="http://support.backendless.com">http://support.backendless.com</a>
Blog:            <a href="http://backendless.com/blog">http://backendless.com/blog</a>
YouTube Channel: <a href="http://youtube.com/backendless">http://youtube.com/backendless</a>
Facebook:        <a href="http://facebook.com/backendless">http://facebook.com/backendless</a>
Twitter:         <a href="http://twitter.com/backendless">http://twitter.com/backendless</a>
</code></pre>

<p>We would love to hear from you. Please let us know about your experiences using Backendless. You can reach us through the support page at <a href="http://support.backendless.com">http://support.backendless.com</a> or email: support@backendless.com</p>

<p>Thank you and endlessly happy coding! <br>
The Backendless Team</p>


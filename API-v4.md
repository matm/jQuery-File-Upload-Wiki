## Initialization
The basic initialization of the File Upload plugin is by calling it on an element node (usually the file upload form or a container node holding the form):
```js
$('#file_upload').fileUpload();
```
The first (or second, if the first is the string `'init'`) parameter can be an object setting various [[Options v4]].

## Setting options after initialization
See [[Options v4]] documentation.

## De-initialization
To remove the file upload widget from the element node, call the *destroy* method:
```js
$('#file_upload').fileUpload('destroy');
```
This will also remove any added event handlers.

## Programmatic file upload
Usually, file uploads are invoked by selecting files via the file upload button or dropping files on the drop zone.  
However it is also possible to upload one or multiple files programmatically:
```js
$('#file_upload').fileUpload('upload', files);
```
The second parameter (*files*) can hold a [File](https://developer.mozilla.org/en/DOM/File) or [Blob](https://developer.mozilla.org/en/DOM/Blob) object or an array of these objects.
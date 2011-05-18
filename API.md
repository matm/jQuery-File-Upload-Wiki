## Initialization
The basic initialization of the File Upload widget is by calling the *fileupload* method on a jQuery collection with the target HTML element (usually a container element holding the file upload form, or the file upload form itself):
```js
$('#fileupload').fileupload();
```
The method accepts an object as first argument that allows to initialize the widget with various [[Options]]:
```js
$('#fileupload').fileupload({
    url: '/path/to/upload/handler.json',
    sequentialUploads: true
});
```

## Setting options after initialization
It is also possible to set [[Options]] after plugin initialization:
```js
$('#fileupload').fileupload(
    'option',
    'url',
    '/path/to/upload/handler.json'
);
```

Or setting multiple [[Options]] at once:
```js
$('#fileupload').fileupload(
    'option',
    {
        url: '/path/to/upload/handler.json',
        sequentialUploads: true
    }
);
```

## De-initialization
To remove the file upload widget from the element node, call the *destroy* method:
```js
$('#fileupload').fileupload('destroy');
```
This will also remove any added event listeners.

## Disable/Enable
As other widgets based on jQuery UI Widget, the File Upload widget can be disabled/enabled:
```js
$('#fileupload').fileupload('disable');
```
```js
$('#fileupload').fileupload('enable');
```

## Programmatic file upload
Usually, file uploads are invoked by selecting files via the file upload button or dropping files on the drop zone.

However it is also possible to files programmatically.  
Files can be added to the upload table:
```js
$('#fileupload').fileupload('add', {files: filesList});
```
Or they can be send directly:
```js
$('#fileupload').fileupload('send', {files: filesList});
```

The second argument must be an object with an array (or array-like list) of [File](https://developer.mozilla.org/en/DOM/File) or [Blob](https://developer.mozilla.org/en/DOM/Blob) objects as *files* property.  
Other properties allow to override options for the file upload, e.g. the upload url:
```js
$('#fileupload').fileupload('add', {files: filesList, url: '/path/to/upload/handler.json'});
```
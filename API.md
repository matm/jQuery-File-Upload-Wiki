## Initialization
The File Upload widget is initialized by calling the *fileupload* method on a jQuery collection with the target HTML element:

```js
$('#fileupload').fileupload();
```

The target element is usually a container element holding the file upload form, or the file upload form itself, but it can also be just the file input element itself, if an url is provided as options parameter.

The initialization method's first argument is an object that allows you to initialize the widget with various [[Options]]:

```js
$('#fileupload').fileupload({
    url: '/path/to/upload/handler.json',
    sequentialUploads: true
});
```

### Data attributes
It is possible to pass options to the initialization method as [HTML5 data attributes](http://api.jquery.com/data/#data-html5):

```html
<input id="fileupload" type="file" name="files[]" multiple
    data-url="/path/to/upload/handler.json"
    data-sequential-uploads="true"
    data-form-data='{"script": "true"}'>
```

```js
/* Initializes the File Upload widget with
{
    url: '/path/to/upload/handler.json',
    sequentialUploads: true,
    formData: {script: true}
}
*/
$('#fileupload').fileupload();
```

## Options
It is possible to change [[Options]] after initializing the widget:

```js
$('#fileupload').fileupload(
    'option',
    'url',
    '/path/to/upload/handler.json'
);
```

If no value is specified, the *option* method acts as a getter:

```js
var dropZone = $('#fileupload').fileupload(
    'option',
    'dropZone'
);
```

Multiple [[Options]] can be set at once by providing an object as second parameter:

```js
$('#fileupload').fileupload(
    'option',
    {
        url: '/path/to/upload/handler.json',
        sequentialUploads: true
    }
);
```

## Deinitialization
To remove the file upload widget functionality from the element node, call the *destroy* method:

```js
$('#fileupload').fileupload('destroy');
```

This will also remove any added event listeners.

## Disable/Enable
As other widgets based on [jQuery UI Widget](http://api.jqueryui.com/jQuery.widget/), the File Upload widget can be disabled/enabled:

```js
$('#fileupload').fileupload('disable');
```

```js
$('#fileupload').fileupload('enable');
```

## Retrieving overall progress data:

```js
var overallProgress = $('#fileupload').fileupload('progress');
```

## Retrieving the number of active uploads:

```js
var activeUploads = $('#fileupload').fileupload('active');
```

## Programmatic file upload
Usually, file uploads are invoked by selecting files via file input button or by dropping files on the drop zone.

However it is also possible to upload files programmatically for browsers with support for [XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads (see [[Browser support]]):

```js
$('#fileupload').fileupload('add', {files: filesList});
```

The second argument must be an object with an array (or array-like list) of [File](https://developer.mozilla.org/en/DOM/File) or [Blob](https://developer.mozilla.org/en/DOM/Blob) objects as *files* property.  
Other properties allow you to override options for the file upload, e.g. the upload url:

```js
$('#fileupload').fileupload('add', {files: filesList, url: '/path/to/upload/handler.json'});
```

The *add* method uploads files by adding them to the upload queue, the same way that files are added via the file input button or drag&drop.  
Files can also be sent directly using the *send* method:

```js
$('#fileupload').fileupload('send', {files: filesList});
```

The *send* method returns a [jqXHR](http://api.jquery.com/jQuery.ajax/#jqXHR) object, that allows to bind callbacks to the ajax file upload request(s):

```js
var jqXHR = $('#fileupload').fileupload('send', {files: filesList})
    .success(function (result, textStatus, jqXHR) {/* ... */})
    .error(function (jqXHR, textStatus, errorThrown) {/* ... */})
    .complete(function (result, textStatus, jqXHR) {/* ... */});
```

**Note**: The *send* API method sends the given files directly, without splitting them up into multiple requests.
So if your files argument is made up of 3 files, it will still only send one request.
If the *multipart* option is true, it will still send all 3 files as part of one multipart request, else it will only send the first file.  
So if you need to send files with multiple requests, either call the *send* API method multiple times, or use the *add* API method instead.

### Programmatic file uploads for browsers without support for XHR file uploads
It is also possible to use the *add* and *send* API methods for browsers without support for [XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads, by making use of the *fileInput* option:

```js
$('#some-file-input-field').bind('change', function (e) {
    $('#fileupload').fileupload('add', {
        fileInput: $(this)
    });
});
```

The fileInput property must be a jQuery collection with an input of type file with a valid files selection.

Non-[XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads make use of the [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js).

## Callbacks
The File Upload widget provides several callback hooks.  
One way of using them is to provide callback methods as part of the [[Options]] object:

```js
$('#fileupload').fileupload({
    drop: function (e, data) {
        $.each(data.files, function (index, file) {
            alert('Dropped file: ' + file.name);
        });
    },
    change: function (e, data) {
        $.each(data.files, function (index, file) {
            alert('Selected file: ' + file.name);
        });
    }
});
```

The second way of using them is by binding event listeners to the widget element:

```js
$('#fileupload')
    .bind('fileuploaddrop', function (e, data) {/* ... */})
    .bind('fileuploadchange', function (e, data) {/* ... */});
```

Each event name has "fileupload" as prefix.

**Note:**
Adding additional event listeners via *bind* method is the preferred option to preserve callback settings by the [jQuery File Upload UI version](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js).

One special callback is the *add* callback, as it provides a *submit* method for the data argument, that will start the file upload:

```js
$('#fileupload').fileupload({
    add: function (e, data) {
        data.submit();
    }
});
```

The submit method of the data argument given to the *add* callback returns a [jqXHR](http://api.jquery.com/jQuery.ajax/#jqXHR) object, that allows to bind callbacks to the ajax file upload request:

```js
$('#fileupload').fileupload({
    add: function (e, data) {
        var jqXHR = data.submit()
            .success(function (result, textStatus, jqXHR) {/* ... */})
            .error(function (jqXHR, textStatus, errorThrown) {/* ... */})
            .complete(function (result, textStatus, jqXHR) {/* ... */});
    }
});
```

## How to cancel an upload
Uploads can be canceled by invoking the *abort* method on a [jqXHR](http://api.jquery.com/jQuery.ajax/#jqXHR) object:

```js
var jqXHR = $('#fileupload').fileupload('send', {files: filesList})
    .error(function (jqXHR, textStatus, errorThrown) {
        if (errorThrown === 'abort') {
            alert('File Upload has been canceled');
        }
    });
$('button.cancel').click(function (e) {
    jqXHR.abort();
});
```

## How to retrieve extra parameters before submitting files
See the guide on how to [[Submit files asynchronously]].
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
As other widgets based on jQuery UI Widget, the File Upload widget can be disabled/enabled:
```js
$('#fileupload').fileupload('disable');
```
```js
$('#fileupload').fileupload('enable');
```

## Programmatic file upload
Usually, file uploads are invoked by selecting files via file input button or by dropping files on the drop zone.

However it is also possible to upload files programmatically:
```js
$('#fileupload').fileupload('add', {files: filesList});
```

The second argument must be an object with an array (or array-like list) of [File](https://developer.mozilla.org/en/DOM/File) or [Blob](https://developer.mozilla.org/en/DOM/Blob) objects as *files* property.  
Other properties allow to override options for the file upload, e.g. the upload url:
```js
$('#fileupload').fileupload('add', {files: filesList, url: '/path/to/upload/handler.json'});
```

The *add* method uploads files by adding them to the upload queue, the same way that files are added via the file input button or drag&drop.  
Files can also be send directly using the *send* method:
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

The second way of using them is by by binding event listeners to the widget element:
```js
$('#fileupload')
    .bind('fileuploaddrop', function (e, data) {/* ... */})
    .bind('fileuploadchange', function (e, data) {/* ... */});
```
Each event name has "fileupload" as prefix.

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
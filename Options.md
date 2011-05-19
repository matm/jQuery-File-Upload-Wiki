The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js)) providing the File Upload API and an additional plugin providing a complete user interface ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).  
All options of the basic version are present in the UI version, while the latter introduces some additional options.

For example code on how to use the options, please refer to the [[API]] documentation.

## AJAX Options

The jQuery File Upload plugin makes use of [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/) for the file upload requests. This is true even for browsers without support for [XHR](https://developer.mozilla.org/en/xmlhttprequest), thanks to the [Iframe Transport plugin](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.iframe-transport.js).

The options set for the File Upload plugin are passed to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/) and allow to define any ajax settings or callbacks.   
The ajax options *processData*, *contentType* and *cache* are set to *false* for the file uploads to work and should not be changed.  
The following options are also set by the plugin, but can be useful to customize:

### url
A string containing the URL to which the request is sent.  
If undefined or empty, it is set to the *action* property of the file upload form if available, or else the URL of the current page.

* Type: *string*
* Example: `'/path/to/upload/handler.json'`

### type
The HTTP request method for the file uploads. Can be "POST" or "PUT" and defaults to "POST".

* Type: *string*
* Example: `'PUT'`

## General Options

### namespace
The namespace used for event handler binding on the dropZone and fileInput collections.  
If not set, the name of the widget ("fileupload") is used.

* Type: *string*
* Example: `'myfileupload'`

### dropZone
The drop target [jQuery object](http://api.jquery.com/Types/#jQuery), by the default the complete document.  
Set to null or an empty collection to disable drag & drop support:

* Type: *jQuery Object*
* Default: `$(document)`

### fileInput
The file input field [jQuery object](http://api.jquery.com/Types/#jQuery), that is listened for change events.  
If undefined, it is set to the file input fields inside of the widget element on plugin initialization.  
Set to null or an empty collection to disable the change listener.

* Type: *jQuery Object*
* Example: `$('input:file')`

### replaceFileInput
By default, the file input field is replaced with a clone after each input field change event.  
This is required for iframe transport queues and allows change events to be fired for the same file selection, but can be disabled by setting this option to false.

* Type: *boolean*
* Default: `true`

### paramName
The parameter name for the file form data (the request argument name).  
If undefined or empty, the name property of the file input field is used, or "files[]" if the file input name property is also empty.

* Type: *string*
* Example: `'attachments[]'`

### singleFileUploads
By default, each file of a selection is uploaded using an individual request for [XHR](https://developer.mozilla.org/en/xmlhttprequest) type uploads.  
Set this option to false to upload file selections in one request each.

**Note:** Uploading multiple files with one request requires the multipart option to be set to *true* (the default).

* Type: *boolean*
* Default: `true`

### sequentialUploads
Set this option to true to issue all file upload requests in a sequential order instead of simultaneous requests.

* Type: *boolean*
* Default: `false`

### forceIframeTransport
Set this option to true to force iframe transport uploads, even if the browser is capable of [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
This can be useful for cross-site file uploads, if the [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_Access_Control#Access-Control-Allow-Origin) header cannot be set for the server-side upload handler which is required for cross-site [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.

* Type: *boolean*
* Default: `false`

### multipart
By default, [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads are sent as multipart/form-data.  
The iframe transport is always using multipart/form-data.  
If this option is set to *false*, the file content is streamed to the server url instead of sending a [RFC 2388](http://www.ietf.org/rfc/rfc2388.txt) multipart message for [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.   
Non-multipart uploads are also referred to as [HTTP PUT file upload](http://de.php.net/manual/en/features.file-upload.put-method.php).  

**Note:** Additional form data is ignored when the multipart option is set to *false*.

* Type: *boolean*
* Default: `true`

### maxChunkSize
To upload large files in smaller chunks, set this option to a preferred maximum chunk size. If set to 0, null or undefined, or the browser does not support the required [Blob API](https://developer.mozilla.org/en/DOM/Blob), files will be uploaded as a whole.

For chunked uploads to work in Mozilla Firefox, the *multipart* option has to be set to *false*. This is due to Gecko 2.0 (Firefox 4 etc.) adding blobs with an empty filename when building a multipart upload request using the [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) interface - see [Bugzilla entry #649150](https://bugzilla.mozilla.org/show_bug.cgi?id=649150). Several server-side frameworks (including PHP and Django) cannot handle multipart file uploads with empty filenames.

**Note:** If this option is enabled and *singleFileUploads* is set to *false*, only the first file of a selection will be uploaded.

* Type: *number*
* Default: `undefined`
* Example: `10000000`

### uploadedBytes
When a non-multipart upload or a chunked multipart upload has been aborted, this option can be used to resume the upload by setting it to the size of the already uploaded bytes. This option is most useful when modifying the options object inside of the "add" or "send" callbacks, as the options are cloned for each file upload.

* Type: *number*
* Default: `undefined`
* Example: `10000000`

### recalculateProgress
By default, failed (abort or error) file uploads are removed from the global progress calculation.  
Set this option to false to prevent recalculating the global progress data.

* Type: *boolean*
* Default: `true`

### formData
Additional form data to be sent along with the file uploads can be set using this option, which accepts an array of objects with name and value properties, a function returning such an array, a [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) object (for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads), or a simple object.  
The form of the first fileInput is given as parameter to the function.

**Note:** Additional form data is ignored when the *multipart* option is set to *false*.

* Type: *Array*, *Object*, *function* or *FormData*
* Default: A function returning the form fields as [serialized Array](http://api.jquery.com/serializeArray):
```js
function (form) {
    return form.serializeArray();
}
```
* Example:
```js
[
  {
    name: 'a',
    value: 1
  },
  {
    name: 'b',
    value: 2
  }
]
```

## Callback Options

All callbacks are of type *function* and can also be bound as event listeners, using the callback name plus "fileupload" as prefix:

```js
$('#fileupload')
    .bind('fileuploadadd', function (e, data) {/* ... */})
    .bind('fileuploadsend', function (e, data) {/* ... */});
```

Adding additional event listeners via *bind* method is the preferred option to preserve callback settings by the [jQuery File Upload UI version](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js).

### add
The add callback is invoked as soon as files are added to the fileupload widget - via file input selection, drag & drop or *add* [[API]] call.  
If the *singleFileUploads* option is enabled, this callback will be called once for each file in the selection for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uplaods, else once for each file selection.  
The upload starts when the *submit* method is invoked on the data parameter.  
The data object contains a *files* property holding the added files and allows to override plugin options as well as define ajax settings.  
*data.submit()* returns a Promise object and allows to attach additional handlers using jQuery's [Deferred](http://api.jquery.com/category/deferred-object/) callbacks.

* Default:
```js
function (e, data) {
    data.submit();
}
```
* Example:
```js
function (e, data) {
    $.each(data.files, function (index, file) {
        alert('Added file: ' + file.name);
    });
    data.url = '/path/to/upload/handler.json';
    var jqXHR = data.submit()
        .success(function (result, textStatus, jqXHR) {/* ... */})
        .error(function (jqXHR, textStatus, errorThrown) {/* ... */})
        .complete(function (result, textStatus, jqXHR) {/* ... */});
}
```

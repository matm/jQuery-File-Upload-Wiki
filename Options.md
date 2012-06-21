The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload.js)) providing the File Upload [[API]] and an additional plugin providing a complete user interface ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-ui.js)).  
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

**Note:**  
"PUT" is only supported by browser supporting XHR file uploads, as iframe transport uploads rely on standard HTML forms which only support "POST" file uploads. See [[Browser support]].

### dataType
The type of data that is expected back from the server.  

**Note:** The UI version of the File Upload plugin sets this option to "json" by default.

* Type: *string*
* Example: `'json'`

## General Options

### namespace
The namespace used for event handler binding on the dropZone and fileInput collections.  
If not set, the name of the widget ("fileupload") is used.

* Type: *string*
* Example: `'myfileupload'`

### dropZone
The drop target [jQuery object](http://api.jquery.com/Types/#jQuery), by the default the complete document.  
Set to null or an empty jQuery collection to disable drag & drop support:

* Type: *jQuery Object*
* Default: `$(document)`

**Note:**  
If you want to allow specific drop zones but disable the default browser action for file drops on the document, add the following JavaScript code:

```js
$(document).bind('drop dragover', function (e) {
    e.preventDefault();
});
```

Note that preventing the default action for both the "drop" and "dragover" events is required to disable the default browser drop action.

### fileInput
The file input field [jQuery object](http://api.jquery.com/Types/#jQuery), that is listened for change events.  
If undefined, it is set to the file input fields inside of the widget element on plugin initialization.  
Set to null or an empty jQuery collection to disable the change listener.

* Type: *jQuery Object*
* Example: `$('input:file')`

### replaceFileInput
By default, the file input field is replaced with a clone after each input field change event.  
This is required for iframe transport queues and allows change events to be fired for the same file selection, but can be disabled by setting this option to false (more in-depth information can be found [here](https://github.com/blueimp/jQuery-File-Upload/issues/638#issuecomment-2215669)).

* Type: *boolean*
* Default: `true`

### paramName
The parameter name for the file form data (the request argument name).  
If undefined or empty, the name property of the file input field is used, or "files[]" if the file input name property is also empty. Can be a string or an array of strings.

* Type: *string* or *array*
* Example: `'attachments[]'`

### singleFileUploads
By default, each file of a selection is uploaded using an individual request for [XHR](https://developer.mozilla.org/en/xmlhttprequest) type uploads.  
Set this option to false to upload file selections in one request each.

**Note:** Uploading multiple files with one request requires the multipart option to be set to *true* (the default).

* Type: *boolean*
* Default: `true`

### limitMultiFileUploads
To limit the number of files uploaded with one [XHR](https://developer.mozilla.org/en/xmlhttprequest) request, set the following option to an integer greater than 0:

* Type: *number*
* Default: `undefined`
* Example: `3`

**Note:** This option is ignored, if *singleFileUploads* is set to true.

### sequentialUploads
Set this option to true to issue all file upload requests in a sequential order instead of simultaneous requests.

* Type: *boolean*
* Default: `false`

### limitConcurrentUploads
To limit the number of concurrent uploads, set this option to an integer value greater than 0.

* Type: *number*
* Default: `undefined`
* Example: `3`

### forceIframeTransport
Set this option to true to force iframe transport uploads, even if the browser is capable of [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
This can be useful for cross-site file uploads, if the [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_Access_Control#Access-Control-Allow-Origin) header cannot be set for the server-side upload handler which is required for cross-site [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.

* Type: *boolean*
* Default: `false`

### redirect
Set this option to the location of a redirect url on the origin server (the server that hosts the file upload form), for cross-domain iframe transport uploads. If set, this value is sent as part of the form data to the upload server.  
The upload server is supposed to redirect the browser to this url after the upload completes and append the upload information as URL-encoded JSON string to the redirect URL, e.g. by replacing the "%s" character sequence.

* Type: *string*
* Example: `'http://example.org/cors/result.html?%s'`

### redirectParamName
The parameter name for the redirect url, sent as part of the form data and set to 'redirect' if this option is empty.

* Type: *string*
* Example: `'redirect-url'`

### postMessage
Set this option to the location of a postMessage window on the upload server, to enable cross-domain [postMessage transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) uploads.

* Type: *string*
* Example: `'http://example.org/upload/postmessage.html'`

**Note:** This feature is currently only fully supported by Google Chrome.

### multipart
By default, [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads are sent as multipart/form-data.  
The iframe transport is always using multipart/form-data.  
If this option is set to *false*, the file content is streamed to the server url instead of sending a [RFC 2388](http://www.ietf.org/rfc/rfc2388.txt) multipart message for [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.   
Non-multipart uploads are also referred to as [HTTP PUT file upload](http://de.php.net/manual/en/features.file-upload.put-method.php).  

**Note:** Additional form data is ignored when the multipart option is set to *false*.  
Non-multipart uploads (*multipart: false*) are broken in Safari 5.1 - see [issue #700](https://github.com/blueimp/jQuery-File-Upload/issues/700).

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

### progressInterval
The minimum time interval in milliseconds to calculate and trigger progress events.

* Type: *number*
* Default: `100`

### bitrateInterval
The minimum time interval in milliseconds to calculate progress bitrate.

* Type: *number*
* Default: `500`

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
    .bind('fileuploadsubmit', function (e, data) {/* ... */})
    .bind('fileuploadsend', function (e, data) {/* ... */})
    .bind('fileuploaddone', function (e, data) {/* ... */})
    .bind('fileuploadfail', function (e, data) {/* ... */})
    .bind('fileuploadalways', function (e, data) {/* ... */})
    .bind('fileuploadprogress', function (e, data) {/* ... */})
    .bind('fileuploadprogressall', function (e, data) {/* ... */})
    .bind('fileuploadstart', function (e) {/* ... */})
    .bind('fileuploadstop', function (e) {/* ... */})
    .bind('fileuploadchange', function (e, data) {/* ... */})
    .bind('fileuploadpaste', function (e, data) {/* ... */})
    .bind('fileuploaddrop', function (e, data) {/* ... */})
    .bind('fileuploaddragover', function (e) {/* ... */});
```

**Note:**
Adding additional event listeners via *bind* method is the preferred option to preserve callback settings by the [jQuery File Upload UI version](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js).

### add
The add callback can be understood as the callback for the file upload request queue. It is invoked as soon as files are added to the fileupload widget - via file input selection, drag & drop or *add* [[API]] call.
  
The data parameter allows to override plugin options as well as define ajax settings.  
*data.files* holds a list of files for the upload request: If the *singleFileUploads* option is enabled (which is the default), the add callback will be called once for each file in the selection for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads, with a *data.files* array length of one, as each file is uploaded individually. Else the *add* callback will be called once for each file selection.

The upload starts when the *submit* method is invoked on the data parameter.  
*data.submit()* returns a [Promise](http://api.jquery.com/Types/#Promise) object and allows to attach additional handlers using jQuery's [Deferred](http://api.jquery.com/category/deferred-object/) callbacks.

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

### submit
Callback for the submit event of each file upload.  
If this callback returns false, the file upload request is not started.

* Example:

```js
function (e, data) {
    var input = $('#input');
    data.formData = {example: input.val()};
    if (!data.formData.example) {
      input.focus();
      return false;
    }
}
```

### send
Callback for the start of each file upload request.  
If this callback returns false, the file upload request is aborted.

* Example:

```js
function (e, data) {
    if (data.files.length > 10) {
        return false;
    }
}
```

### done
Callback for successful upload requests. This callback is the equivalent to the success callback provided by [jQuery ajax()](http://api.jquery.com/jQuery.ajax/) and will also be called if the server returns a JSON response with an error property.

* Example:

```js
function (e, data) {
    // data.result
    // data.textStatus;
    // data.jqXHR;
}
```

### fail
Callback for failed (abort or error) upload requests. This callback is the equivalent to the error callback provided by [jQuery ajax()](http://api.jquery.com/jQuery.ajax/) and will not be called if the server returns a JSON response with an error property, as this counts as successful request due to the successful HTTP response.

* Example:

```js
function (e, data) {
    // data.errorThrown
    // data.textStatus;
    // data.jqXHR;
}
```

### always
Callback for completed (success, abort or error) upload requests. This callback is the equivalent to the complete callback provided by [jQuery ajax()](http://api.jquery.com/jQuery.ajax/).

* Example:

```js
function (e, data) {
    // data.result
    // data.textStatus;
    // data.jqXHR;
}
```

### progress
Callback for upload progress events.

* Example:

```js
function (e, data) {
    var progress = parseInt(data.loaded / data.total * 100, 10);
}
```

### progressall
Callback for global upload progress events.

* Example:

```js
function (e, data) {
    var progress = parseInt(data.loaded / data.total * 100, 10);
}
```

### start
Callback for uploads start, equivalent to the global [ajaxStart](http://api.jquery.com/ajaxStart/) event (but for file upload requests only).

* Example:

```js
function (e) {
    alert('Uploads started');
}
```

### stop
Callback for uploads stop, equivalent to the global [ajaxStop](http://api.jquery.com/ajaxStop/) event (but for file upload requests only).

* Example:

```js
function (e) {
    alert('Uploads finished');
}
```

### change
Callback for change events of the fileInput collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        alert('Selected file: ' + file.name);
    });
}
```

### paste
Callback for paste events to the dropZone collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        alert('Pasted file type: ' + file.type);
    });
}
```

### drop
Callback for drop events of the dropZone collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        alert('Dropped file: ' + file.name);
    });
}
```

### dragover
Callback for dragover events of the dropZone collection.

* Example:

```js
function (e) {
    // e.dataTransfer
}
```

## File Processing Options

### process
A list of file processing actions.

* Type: *array*
* Default: `[]` (empty array)
* Example:

```js
[
    {
        action: 'load',
        fileTypes: /^image\/(gif|jpeg|png)$/,
        maxFileSize: 20000000 // 20MB
    },
    {
        action: 'resize',
        maxWidth: 1920,
        maxHeight: 1200,
        minWidth: 800,
        minHeight: 600
    },
    {
        action: 'save'
    }
],
```

## Additional Options for the UI version

### autoUpload
By default, files added to the UI widget are uploaded as soon as the user clicks on the start buttons. To enable automatic uploads, set this option to true.

* Type: *boolean*
* Default: `false`

### maxNumberOfFiles
This option limits the number of files that are allowed to be uploaded using this widget.  
By default, unlimited file uploads are allowed.

* Type: *number*
* Example: `10`

### maxFileSize
The maximum allowed file size in bytes, by default unlimited.

**Note:** This option has only an effect for browsers supporting the [File API](https://developer.mozilla.org/en/DOM/file).

* Type: *number*
* Example: `5000000`

### minFileSize
The minimum allowed file size, by default 1 byte.

**Note:** This option has only an effect for browsers supporting the [File API](https://developer.mozilla.org/en/DOM/file).

* Type: *number*
* Example: `1`

### acceptFileTypes
The regular expression for allowed file types, matches against either file type or file name as only browsers with support for the [File API](https://developer.mozilla.org/en/DOM/file) report the file type.

* Type: *Regular Expression*
* Example: `/(\.|\/)(gif|jpe?g|png)$/i`

### previewSourceFileTypes
The regular expression to define for which files a preview image is shown, matched against the file type.

**Note:** Preview images (before upload) are only displayed for browsers supporting the *URL* or *webkitURL* APIs or the *readAsDataURL* method of the [FileReader](https://developer.mozilla.org/en/DOM/FileReader) interface.

* Type: *Regular Expression*
* Default: `/^image\/(gif|jpeg|png)$/`

### previewMaxSourceFileSize
The maximum file size for preview images in bytes.

* Type: *number*
* Default: `5000000`

### previewMaxWidth
The maximum width of the preview images.

* Type: *number*
* Default: `80`

### previewMaxHeight
The maximum height of the preview images.

* Type: *number*
* Default: `80`

### previewAsCanvas
By default, preview images are displayed as [canvas](https://developer.mozilla.org/en/HTML/canvas) elements if supported by the browser.  
Set this option to false to always display preview images as *img* elements.

* Type: *boolean*
* Default: `true`

### filesContainer
The container for the files listed for upload / download.  
Is transformed into a [jQuery object](http://api.jquery.com/Types/#jQuery) if set as DOM node or string.

* Type: *object*
* Default: `widgetContainer.find('.files')`
* Example: `'#files'`

### prependFiles
By default, files are appended to the files container.  
Set this option to true, to prepend files instead.

* Type: *boolean*
* Default: `false`

### uploadTemplate
The upload template function - see [[Template Engine]].

* Type: *function*

### uploadTemplateId
The ID of the upload template, given as parameter to the [tmpl()](https://github.com/blueimp/JavaScript-Templates) method to set the **uploadTemplate** option.

* Type: *string*
* Default: `'template-upload'`

### downloadTemplate
The download template function - see [[Template Engine]].

* Type: *function*

### downloadTemplateId
The ID of the download template, given as parameter to the [tmpl()](https://github.com/blueimp/JavaScript-Templates) method to set the **downloadTemplate** option.

* Type: *string*
* Default: `'template-upload'`

## Additional Callback Options for the UI version

All callbacks are of type function and can also be bound as event listeners, using the callback name plus "fileupload" as prefix:

```js
$('#fileupload')
    .bind('fileuploaddestroy', function (e, data) {/* ... */})
    .bind('fileuploadadded', function (e, data) {/* ... */})
    .bind('fileuploadstarted', function (e, data) {/* ... */})
    .bind('fileuploadsent', function (e, data) {/* ... */})
    .bind('fileuploadcompleted', function (e, data) {/* ... */})
    .bind('fileuploadfailed', function (e, data) {/* ... */})
    .bind('fileuploadstopped', function (e) {/* ... */})
    .bind('fileuploaddestroyed', function (e) {/* ... */});
```

### destroy
Callback for file deletion events.

**Note:** Since the UI version already sets this callback option, it is recommended to use the event binding method to attach additional event listeners.

* Example:

```js
function (e, data) {
    // data.context: download row,
    // data.url: deletion url,
    // data.type: deletion request type, e.g. "DELETE",
    // data.dataType: deletion response type, e.g. "json"
}
```

* Event binding example:

```js
$('#fileupload')
    .bind('fileuploaddestroy', function (e, data) {/* ... */});
```

### added
The **added** callback is the equivalent to the **add** callback and is triggered after the upload template has been rendered and the transition effects have completed.

### started
The **started** callback is the equivalent to the **start** callback and is triggered after the start callback has run and the transition effects called in the start callback have completed.

**Note:** Unlike the start callback, which is always called before all send callbacks, the started callback might be called after the sent callbacks, depending on the duration of the transition effects in those callbacks.

### sent
The **sent** (note the "t" instead of the "d") callback is the equivalent to the **send** callback and is triggered after the send callback has run and the files are about to be sent.

### completed
The **completed** callback is the equivalent to the **done** callback and is triggered after successful uploads after the download template has been rendered and the transition effects have completed.

### failed
The **failed** callback is the equivalent to the **fail** callback and is triggered after failed uploads after the download template has been rendered and the transition effects have completed.

### stopped
The **stopped** callback is the equivalent to the **stop** callback and is triggered after the stop callback has run and the transition effects called in the stop callback have completed.

**Note:** Unlike the stop callback, which is always called after all done callbacks, the stopped callback might be called before the completed callbacks, depending on the duration of the transition effects in those callbacks.

### destroyed
The **destroyed** callback is the equivalent to the **destroy** callback and is triggered after files have been deleted, the transition effects have completed and the download template has been removed.
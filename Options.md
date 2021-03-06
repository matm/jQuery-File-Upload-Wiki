The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload.js)) providing the File Upload [[API]] and an additional plugin providing a complete user interface ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-ui.js)).  
All options of the basic version are present in the UI version, while the latter introduces some additional options.

For example code on how to use the options, please refer to the [[API]] documentation.

## AJAX Options

The jQuery File Upload plugin makes use of [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/) for the file upload requests. This is true even for browsers without support for [XHR](https://developer.mozilla.org/en/xmlhttprequest), thanks to the [Iframe Transport plugin](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js).

The options set for the File Upload plugin are passed to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/) and allow to define any ajax settings or callbacks.   
The ajax options `processData`, `contentType` and `cache` are set to `false` for the file uploads to work and should not be changed.  
The `timeout` setting is set to `0`. See [pull request #3399](https://github.com/blueimp/jQuery-File-Upload/pull/3399) for the reasoning behind this.
The following options are also set by the plugin, but can be useful to customize:

### url
A string containing the URL to which the request is sent.  
If undefined or empty, it is set to the *action* property of the file upload form if available, or else the URL of the current page.

* Type: *string*
* Example: `'/path/to/upload/handler.json'`

### type
The HTTP request method for the file uploads. Can be "POST", "PUT" or "PATCH" and defaults to "POST".

* Type: *string*
* Example: `'PUT'`

**Note:**  
"PUT" and "PATCH" are only supported by browser supporting XHR file uploads, as iframe transport uploads rely on standard HTML forms which only support "POST" file uploads. See [[Browser support]].  
If the type is defined as "PUT" or "PATCH", the iframe transport will send the files via "POST" and transfer the original method as "_method" URL parameter.

**Note:**
As was noted above, it's a common practice to use "_method" to transfer the type of your request. For example, "Ruby on Rails" framework uses a hidden input with the name "_method" within each form, so it will likely override the value that you will set here.

### dataType
The type of data that is expected back from the server.  

**Note:** The UI version of the File Upload plugin sets this option to "json" by default.

* Type: *string*
* Example: `'json'`

## General Options

* Type: *string*
* Example: `'myfileupload'`

### dropZone
The drop target [jQuery object](http://api.jquery.com/Types/#jQuery), by default the complete document.  
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

### pasteZone
The paste target [jQuery object](http://api.jquery.com/Types/#jQuery), by the default the complete document.  
Set to a jQuery collection to enable paste support:

* Type: *jQuery Object*
* Default: `undefined `

**Note:**  
Uploading files via copy&paste is currently only supported by Google Chrome.

### fileInput
The file input field [jQuery object](http://api.jquery.com/Types/#jQuery), that is listened for change events.  
If undefined, it is set to the file input fields inside of the widget element on plugin initialization.  
Set to null or an empty jQuery collection to disable the change listener.

* Type: *jQuery Object*
* Example: `$('input:file')`

### replaceFileInput
By default, the file input field is replaced with a clone after each input field change event.  
This is required for iframe transport queues and allows change events to be fired for the same file selection, but can be disabled by setting this option to false (more in-depth information can be found [in the FAQ](https://github.com/blueimp/jQuery-File-Upload/wiki/Frequently-Asked-Questions#why-is-the-file-input-field-cloned-and-replaced-after-each-selection)).

* Type: *boolean*
* Default: `true`

### paramName
The parameter name for the file form data (the request argument name).  
If undefined or empty, the name property of the file input field is used, or "files[]" if the file input name property is also empty. Can be a string or an array of strings.

* Type: *string* or *array*
* Example: `'attachments[]'`

### formAcceptCharset
Allows to set the accept-charset attribute for the iframe upload forms.  
If formAcceptCharset is not set, the accept-charset attribute of the file upload widget form is used, if available.

* Type: *string*
* Example: `'utf-8'`

### singleFileUploads
By default, each file of a selection is uploaded using an individual request for [XHR](https://developer.mozilla.org/en/xmlhttprequest) type uploads.  
Set this option to false to upload file selections in one request each.

**Note:** Uploading multiple files with one request requires the multipart option to be set to *true* (the default).

* Type: *boolean*
* Default: `true`

### limitMultiFileUploads
To limit the number of files uploaded with one [XHR](https://developer.mozilla.org/en/xmlhttprequest) request, set the following option to an integer greater than 0:

* Type: *integer*
* Default: `undefined`
* Example: `3`

**Note:** This option is ignored, if *singleFileUploads* is set to true or *limitMultiFileUploadSize* is set and the browser reports file sizes.

### limitMultiFileUploadSize
The following option limits the number of files uploaded with one [XHR](https://developer.mozilla.org/en/xmlhttprequest) request to keep the request size under or equal to the defined limit in bytes:

* Type: *integer*
* Default: `undefined`
* Example: `1000000`

**Note:** This option is ignored, if *singleFileUploads* is set to true.

### limitMultiFileUploadSizeOverhead
Multipart file uploads add a number of bytes to each uploaded file, therefore the following option adds an overhead for each file used in the limitMultiFileUploadSize configuration:

* Type: *integer*
* Default: `512`

### sequentialUploads
Set this option to true to issue all file upload requests in a sequential order instead of simultaneous requests.

* Type: *boolean*
* Default: `false`

### limitConcurrentUploads
To limit the number of concurrent uploads, set this option to an integer value greater than 0.

* Type: *integer*
* Default: `undefined`
* Example: `3`

**Note:** This option is ignored, if *sequentialUploads* is set to true.

### forceIframeTransport
Set this option to true to force iframe transport uploads, even if the browser is capable of [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
This can be useful for cross-site file uploads, if the [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_Access_Control#Access-Control-Allow-Origin) header cannot be set for the server-side upload handler which is required for cross-site [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.

* Type: *boolean*
* Default: `false`

### initialIframeSrc
This option is only used by the iframe transport and allows to override the URL of the initial iframe src.

* Type: *string*
* Default: `'javascript:false;'`

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
Set this option to the location of a [postMessage API](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) on the upload server, to enable cross-domain [postMessage transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) uploads.

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

For chunked uploads to work in Mozilla Firefox <7, the *multipart* option has to be set to *false*. This is due to Gecko 2.0 (Firefox 4 etc.) adding blobs with an empty filename when building a multipart upload request using the [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) interface - see [Bugzilla entry #649150 (Fixed in FF 7.0)](https://bugzilla.mozilla.org/show_bug.cgi?id=649150). Several server-side frameworks (including PHP and Django) cannot handle multipart file uploads with empty filenames.

**Note:** If this option is enabled and *singleFileUploads* is set to *false*, only the first file of a selection will be uploaded.

* Type: *integer*
* Default: `undefined`
* Example: `10000000`

### uploadedBytes
When a non-multipart upload or a chunked multipart upload has been aborted, this option can be used to resume the upload by setting it to the size of the already uploaded bytes. This option is most useful when modifying the options object inside of the "add" or "send" callbacks, as the options are cloned for each file upload.

* Type: *integer*
* Default: `undefined`
* Example: `10000000`

### recalculateProgress
By default, failed (abort or error) file uploads are removed from the global progress calculation.  
Set this option to false to prevent recalculating the global progress data.

* Type: *boolean*
* Default: `true`

### progressInterval
The minimum time interval in milliseconds to calculate and trigger progress events.

* Type: *integer*
* Default: `100`

### bitrateInterval
The minimum time interval in milliseconds to calculate progress bitrate.

* Type: *integer*
* Default: `500`

### autoUpload
By default, files added to the widget are uploaded as soon as the user clicks on the start buttons. To enable automatic uploads, set this option to true.

* Type: *boolean*
* Default: `true`

Please note that in the basic File Upload plugin, this option is set to **true** by default.

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
    .bind('fileuploaddragover', function (e) {/* ... */})
    .bind('fileuploadchunksend', function (e, data) {/* ... */})
    .bind('fileuploadchunkdone', function (e, data) {/* ... */})
    .bind('fileuploadchunkfail', function (e, data) {/* ... */})
    .bind('fileuploadchunkalways', function (e, data) {/* ... */});
```

**Note:**
Adding additional event listeners via *bind* (or *on* method with jQuery 1.7+) method is the preferred option to preserve callback settings by the [jQuery File Upload UI version](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js).

### add
The add callback can be understood as the callback for the file upload request queue. It is invoked as soon as files are added to the fileupload widget - via file input selection, drag & drop or *add* [[API]] call.
  
The data parameter allows to override plugin options as well as define ajax settings.  
*data.files* holds a list of files for the upload request: If the *singleFileUploads* option is enabled (which is the default), the add callback will be called once for each file in the selection for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads, with a *data.files* array length of one, as each file is uploaded individually. Else the *add* callback will be called once for each file selection.

The upload starts when the *submit* method is invoked on the data parameter.  
*data.submit()* returns a [Promise](http://api.jquery.com/Types/#Promise) object and allows to attach additional handlers using jQuery's [Deferred](http://api.jquery.com/category/deferred-object/) callbacks.

The default *add* callback submits the files if the *autoUpload* option is set to *true* (the default for the basic plugin). It also handles processing of files before upload, if any process handlers have been registered.

* Default:

```js
function (e, data) {
    if (data.autoUpload || (data.autoUpload !== false &&
            $(this).fileupload('option', 'autoUpload'))) {
        data.process().done(function () {
            data.submit();
        });
    }
}
```

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        console.log('Added file: ' + file.name);
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
      data.context.find('button').prop('disabled', false);
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
    console.log('Uploads started');
}
```

### stop
Callback for uploads stop, equivalent to the global [ajaxStop](http://api.jquery.com/ajaxStop/) event (but for file upload requests only).

* Example:

```js
function (e) {
    console.log('Uploads finished');
}
```

### change
Callback for change events of the fileInput collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        console.log('Selected file: ' + file.name);
    });
}
```

### paste
Callback for paste events to the dropZone collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        console.log('Pasted file type: ' + file.type);
    });
}
```

### drop
Callback for drop events of the dropZone collection.

* Example:

```js
function (e, data) {
    $.each(data.files, function (index, file) {
        console.log('Dropped file: ' + file.name);
    });
}
```

### dragover
Callback for dragover events of the dropZone collection.

To prevent the plugin from calling the *preventDefault()* method on the original dragover event object and setting the *dataTransfer.dropEffect* to *copy*, call the preventDefault() method on the event object of the **fileuploaddragover** callback:

```js
function (e, data) {
    e.preventDefault(); // Prevents the default dragover action of the File Upload widget
}
```

**Note:** The file upload plugin only provides a dragover callback, as it is used to make drag&drop callbacks work. If you want callbacks for additional drag events, simply bind to these events with jQuery's native event binding mechanism on your dropZone element, e.g.:

```js
$('#fileupload').on('dragleave', function (e) {
    // dragleave callback implementation
});
```

### chunksend
Callback for the start of each chunk upload request.  
If this callback returns false, the chunk upload request is aborted.

### chunkdone
Callback for successful chunk upload requests.

### chunkfail
Callback for failed (abort or error) chunk upload requests

### chunkalways
Callback for completed (success, abort or error) chunk upload requests.

## File Processing Options

### processQueue
A list of file processing actions.

* Type: *array*
* Default: `[]` (empty array)
* Example:

```js
[
    {
        action: 'loadVideo',
        // Use the action as prefix for the "@" options:
        prefix: true,
        fileTypes: '@',
        disabled: '@disableVideoPreview'
    },
    {
        action: 'validate',
        // Always trigger this action,
        // even if the previous action was rejected: 
        always: true,
        acceptFileTypes: '@'
    }
]
```

Each item in the process queue must be an object with an **action** property of type *string*.  
This action must be defined as a property of type function of **$.blueimp.fileupload.prototype.processActions**.  
e.g. ``action: 'validate'`` must exist as function *$.blueimp.fileupload.prototype.processActions.validate*.  

The items in the process queue will be applied in sequential order to each selected file when processing the files selection. 

The process functions get passed two arguments when called in the processing queue:
* *data*: A copy of the data object that is passed to the *add* callback, with *data.files* referencing the files array.  
Additionally, the data object has an *index* property set to the index of the current file to be processed.
* *options*: The options object of the current process action.

The *this* object of the process action is set to the widget root, not to *processActions*. This allows to access the global widget options via ``this.options``.

The process function is supposed to return either the **data** object, or a [Promise](http://api.jquery.com/Types/#Promise) object which resolves or rejects with the **data** object as argument.

A simplified *validate* process action as example:

```js
$.widget('blueimp.fileupload', $.blueimp.fileupload, {

    options: {
        acceptFileTypes: /(\.|\/)(gif|jpe?g|png)$/i,
        processQueue: {
            action: 'validate',
            acceptFileTypes: '@',
            disabled: '@disableValidation'
        }
    },

    processActions: {

        validate: function (data, options) {
            if (options.disabled) {
                return data;
            }
            var dfd = $.Deferred(),
                file = data.files[data.index];
            if (!options.acceptFileTypes.test(file.type)) {
                file.error = 'Invalid file type.';
                dfd.rejectWith(this, [data]);
            } else {
                dfd.resolveWith(this, [data]);
            }
            return dfd.promise();
        }

    }

});
```

#### @-Options
Each property of a process queue item that starts with an "@"-sign will be assigned its value following this set of rules:

* Remove the "@"-sign.
* If the resulting string is not empty, set the property value to the global option of the same name.
e.g. ``disabled: '@disableVideoPreview'`` will be set to the global *disableVideoPreview* option.
* If the property string is empty, check if the process object has the prefix property set to *true*:
    * If no, set its value to the global option with the same name as the property.
    e.g. ``acceptFileTypes: '@'`` will be set to the global *acceptFileTypes* option.
    * If yes, set its value to the global option with name of the property plus its action value as prefix in camel case.
    e.g. ``fileTypes: '@'`` from the *loadVideo* process above will be set to the global *loadVideoFileTypes* option, as the *prefix* property is set to *true*.

## Processing Callback Options

All callbacks are of type *function* and can also be bound as event listeners, using the callback name plus "fileupload" as prefix:

```js
$('#fileupload')
    .bind('fileuploadprocessstart', function (e) {/* ... */})
    .bind('fileuploadprocess', function (e, data) {/* ... */})
    .bind('fileuploadprocessdone', function (e, data) {/* ... */})
    .bind('fileuploadprocessfail', function (e, data) {/* ... */})
    .bind('fileuploadprocessalways', function (e, data) {/* ... */})
    .bind('fileuploadprocessstop', function (e) {/* ... */});
```

Note that the data object contains two arrays:
* files - which contains the result of the process applied.
* originalFiles - the original uploaded files.

It also contains an index parameter that tells you which file was worked on this time.

### processstart
Callback for the start of the fileupload processing queue.

* Example:

```js
function (e) {
    console.log('Processing started...');
}
```

### process
Callback for the start of an individual file processing queue.

* Example:

```js
function (e, data) {
    console.log('Processing ' + data.files[data.index].name + '...');
}
```

### processdone
Callback for the successful end of an individual file processing queue.

* Example:

```js
function (e, data) {
    console.log('Processing ' + data.files[data.index].name + ' done.');
}
```

### processfail
Callback for the failure of an individual file processing queue.

* Example:

```js
function (e, data) {
    console.log('Processing ' + data.files[data.index].name + ' failed.');
}
```

### processalways
Callback for the end (done or fail) of an individual file processing queue.

* Example:

```js
function (e, data) {
    console.log('Processing ' + data.files[data.index].name + ' ended.');
}
```

### processstop
Callback for the stop of the fileupload processing queue.

* Example:

```js
function (e) {
    console.log('Processing stopped.');
}
```

## Image Preview & Resize Options

### disableImageHead
Disable parsing and storing the image header.

* Type: *boolean*
* Default: `false`

### disableExif
Disable parsing Exif data.

* Type: *boolean*
* Default: `false`

### disableExifThumbnail
Disable parsing the Exif Thumbnail.

* Type: *boolean*
* Default: `false`

### disableExifSub
Disable parsing the Exif Sub IFD (additional Exif info).

* Type: *boolean*
* Default: `false`

### disableExifGps
Disable parsing Exif Gps data.

* Type: *boolean*
* Default: `false`

### disableImageMetaDataLoad
Disable parsing image meta data (image head and Exif data).

* Type: *boolean*
* Default: `false`

### disableImageMetaDataSave
Disables saving the image meta data into the resized images.

* Type: *boolean*
* Default: `false`

### loadImageFileTypes
The regular expression for the types of images to load, matched against the file type.

* Type: *Regular Expression*
* Default: `/^image\/(gif|jpeg|png)$/`

### loadImageMaxFileSize
The maximum file size of images to load.

* Type: *integer*
* Default: `10000000`

### loadImageNoRevoke
Don't revoke the object URL created to load the image.

* Type: *boolean*
* Default: `false`

### disableImageLoad
Disable loading and therefore processing of images.

* Type: *boolean*
* Default: `false`

### imageMaxWidth
The maximum width of resized images.

* Type: *integer*
* Default: `5000000`

### imageMaxHeight
The maximum height of resized images.

* Type: *integer*
* Default: `5000000`

### imageMinWidth
The minimum width of resized images.

* Type: *integer*
* Default: `undefined`

### imageMinHeight
The minimum height of resized images.

* Type: *integer*
* Default: `undefined`

### imageCrop
Define if resized images should be cropped or only scaled.

* Type: *boolean*
* Default: `false`

### imageOrientation
Defines the image orientation (1-8) or takes the orientation value from Exif data if set to *true*.

* Type: *integer* or *boolean*
* Default: `false`

### imageForceResize
If set to `true`, forces writing to and saving images from canvas, even if the original image fits the maximum image constraints.

* Type: *integer* or *boolean*
* Default: `undefined`

### disableImageResize
Disables the resize image functionality.

* Type: *boolean*
* Default: `true`

### imageQuality
Sets the quality parameter given to the [canvas.toBlob()](https://developer.mozilla.org/en/docs/Web/API/HTMLCanvasElement) call when saving resized images.

* Type: *float*
* Default: `undefined`

### imageType
Sets the type parameter given to the [canvas.toBlob()](https://developer.mozilla.org/en/docs/Web/API/HTMLCanvasElement) call when saving resized images.

* Type: *string*
* Default: *The image type of the original file, e.g. `image/jpg`*

### previewMaxWidth
The maximum width of the preview images.

* Type: *integer*
* Default: `80`

### previewMaxHeight
The maximum height of the preview images.

* Type: *integer*
* Default: `80`

### previewMinWidth
The minimum width of preview images.

* Type: *integer*
* Default: `undefined`

### previewMinHeight
The minimum height of preview images.

* Type: *integer*
* Default: `undefined`

### previewCrop
Define if preview images should be cropped or only scaled.

* Type: *boolean*
* Default: `false`

### previewOrientation
Defines the preview orientation (1-8) or takes the orientation value from Exif data if set to *true*.

* Type: *integer* or *boolean*
* Default: `true`

### previewThumbnail
Create the preview using the Exif data thumbnail.

* Type: *boolean*
* Default: `true`

### previewCanvas
Define if preview images should be resized as canvas elements.

* Type: *boolean*
* Default: `true`

### imagePreviewName
Define the name of the property that the preview element is stored as on the File object.

* Type: *string*
* Default: `'preview'`

### disableImagePreview
Disables image previews.

* Type: *boolean*
* Default: `false`

## Audio preview options

### loadAudioFileTypes
The regular expression for the types of audio files to load, matched against the file type.

* Type: *Regular Expression*
* Default: `/^audio\/.*$/`

### loadAudioMaxFileSize
The maximum file size of audio files to load.

* Type: *integer*
* Default: `undefined`

### audioPreviewName
Define the name of the property that the preview element is stored as on the File object.

* Type: *string*
* Default: `'preview'`

### disableAudioPreview
Disable audio previews.

* Type: *boolean*
* Default: `false`

## Video preview options

### loadVideoFileTypes
The regular expression for the types of video files to load, matched against the file type.

* Type: *Regular Expression*
* Default: `/^video\/.*$/`

### loadVideoMaxFileSize
The maximum file size of video files to load.

* Type: *integer*
* Default: `undefined`

### videoPreviewName
Define the name of the property that the preview element is stored as on the File object.

* Type: *string*
* Default: `'preview'`

### disableVideoPreview
Disable video previews.

* Type: *boolean*
* Default: `false`

## Validation options

### acceptFileTypes
The regular expression for allowed file types, matches against either file type or file name as only browsers with support for the [File API](https://developer.mozilla.org/en/DOM/file) report the file type.

* Type: *Regular Expression*
* Default: `undefined`
* Example: `/(\.|\/)(gif|jpe?g|png)$/i`

### maxFileSize
The maximum allowed file size in bytes.

* Type: *integer*
* Default: `undefined`
* Example: `10000000` // 10 MB

**Note:** This option has only an effect for browsers supporting the [File API](https://developer.mozilla.org/en/DOM/file).

### minFileSize
The minimum allowed file size in bytes.

* Type: *integer*
* Default: `undefined`
* Example: `1` // 1 Byte

**Note:** This option has only an effect for browsers supporting the [File API](https://developer.mozilla.org/en/DOM/file).

### maxNumberOfFiles
This option limits the number of files that are allowed to be uploaded using this widget.  
By default, unlimited file uploads are allowed.

* Type: *integer*
* Example: `10`

**Note**:  
The **maxNumberOfFiles** option depends on the getNumberOfFiles option, which is defined by the UI and AngularJS implementations.

### disableValidation
Disables file validation.

* Type: *boolean*
* Default: `false`

## Additional Options for the UI version

### getFilesFromResponse
Callback to retrieve the list of files from the server response.  
Is given the data argument of the **done** callback, which contains the result property.  
Must return an array.

* Type: *function*
* Example: `function (data) {return data.result.files;}`

### getNumberOfFiles
This option is a function that returns the current number of files selected and uploaded.  
It is used in the maxNumberOfFiles validation.

* Type: *function*
* Example: `function () {return 5;}`

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
* Default: `'template-download'`

## Additional Callback Options for the UI version

All callbacks are of type function and can also be bound as event listeners, using the callback name plus "fileupload" as prefix:

```js
$('#fileupload')
    .bind('fileuploaddestroy', function (e, data) {/* ... */})
    .bind('fileuploaddestroyed', function (e, data) {/* ... */})
    .bind('fileuploadadded', function (e, data) {/* ... */})
    .bind('fileuploadsent', function (e, data) {/* ... */})
    .bind('fileuploadcompleted', function (e, data) {/* ... */})
    .bind('fileuploadfailed', function (e, data) {/* ... */})
    .bind('fileuploadfinished', function (e, data) {/* ... */})
    .bind('fileuploadstarted', function (e) {/* ... */})
    .bind('fileuploadstopped', function (e) {/* ... */});
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

### destroyed
The **destroyed** callback is the equivalent to the **destroy** callback and is triggered after files have been deleted, the transition effects have completed and the download template has been removed.

### added
The **added** callback is the equivalent to the **add** callback and is triggered after the upload template has been rendered and the transition effects have completed.

### sent
The **sent** (note the "t" instead of the "d") callback is the equivalent to the **send** callback and is triggered after the send callback has run and the files are about to be sent.

### completed
The **completed** callback is the equivalent to the **done** callback and is triggered after successful uploads after the download template has been rendered and the transition effects have completed.

### failed
The **failed** callback is the equivalent to the **fail** callback and is triggered after failed uploads after the download template has been rendered and the transition effects have completed.

### finished
The **finished** callback is the equivalent to the **always** callback and is triggered after both completed and failed uploads after the equivalent template has been rendered and the transition effects have completed.

### started
The **started** callback is the equivalent to the **start** callback and is triggered after the start callback has run and the transition effects called in the start callback have completed.

**Note:** Unlike the start callback, which is always called before all send callbacks, the started callback might be called after the sent callbacks, depending on the duration of the transition effects in those callbacks.

### stopped
The **stopped** callback is the equivalent to the **stop** callback and is triggered after the stop callback has run and the transition effects called in the stop callback and all done callbacks have completed.

The **stopped** callback is therefore always triggered after each completed, failed and finished callback is done.
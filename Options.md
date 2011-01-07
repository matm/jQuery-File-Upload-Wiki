The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js)) and an additional plugin providing an advanced user interface ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).
 
This page lists the various options that can be set for the plugin. They can be used like this with the basic version:
```js
$('.upload').fileUpload({
    namespace: 'file_upload_1',
    url: '/path/to/upload/handler.json'
});
```

And like this with the advanced user interface version:
```js
$('.upload').fileUploadUI({
    namespace: 'file_upload_1',
    url: '/path/to/upload/handler.json'
    /* ... */
});
```

**Note:** The advanced user interface version requires four settings, which must be set:

* uploadTable
* downloadTable
* buildUploadRow
* buildDownloadRow

## Options for the basic jQuery File Upload Plugin

### namespace
Allows to use multiple instances of the File Upload Plugin on the same page by avoiding event handler collisions.

* Type: *String*
* Default: `'file_upload'`

### cssClass
The CSS class that is added on plugin initialization to the dropZone.  
The dropZone being the HTML form or HTML element containing the form where files can be dropped.

* Type: *String*
* Default: `'file_upload'`

### url
The url to which the file upload form is submitted.

* Type: *String*
* Default: The action attribute of the file upload form.
* Example: `'/path/to/upload/handler.json'`

### method
The method of the HTTP request used to send the file(s) to the server.  
Can be *POST* (multipart/formdata file upload) or *PUT* (streaming file upload).

* Type: *String*
* Default: The method attribute of the file upload form (*POST*).

### fieldName
The parameter name used to submit the file(s) to the server.

* Type: *String*
* Default: The name attribute of the file input field of the form.
* Example: `'file'`

### multipart
If set to *false*, streams the file content to the server url instead of sending a [RFC 2388](http://www.ietf.org/rfc/rfc2388.txt) multipart message.  
Non-multipart uploads are also referred to as [HTTP PUT file upload](http://de.php.net/manual/en/features.file-upload.put-method.php).

* Type: *boolean*
* Default: *true*

### formData
Allows to define additional parameters, that are send with the file(s) to the server url.  
Accepts an Array of Objects with name and value attributes, a Function returning such an Array or a simple Object.  
**Note:** Additional form data is ignored when the multipart option is set to *false*.

* Type: *Array*, *Object* or *function*
* Default: A function returning the form fields as [serialized Array](http://api.jquery.com/serializeArray).
* Example:
```js
[
  {
    name: a
    value: 1
  },
  {
    name: b
    value: 2
  }
]
```

### withCredentials
Indicates whether or not cross-site XMLHttpRequest file uploads should be made using credentials such as cookies or authorization headers.  
Sets the [withCredentials property](https://developer.mozilla.org/en/xmlhttprequest#Properties) on the XMLHttpRequest object.

* Type: *boolean*
* Default: *false*

### onProgress
A callback function that is called on upload progress.  
**Note:** This is only called for browsers which support the [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object. Also, the browser must support either the [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) or [FileReader](https://developer.mozilla.org/en/DOM/FileReader) interfaces or the multipart option has to be set to *false*.  
The jQuery File Upload UI Plugin makes use of this callback to update the progress bar.

* Type: *function*
* Arguments:
    1. event: XHR onprogress event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    var progress = parseInt(event.loaded / event.total * 100, 10);
    /* ... */
}
```

### onLoad
A callback function that is called when the file upload is complete.  
The jQuery File Upload UI Plugin makes use of this callback to remove the uploadRow, parse the JSON response and add the downloadRow.

* Type: *function*
* Arguments:
    1. event: XHR onload event object / IFrame onload event object (legacy browsers).
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    var json;
    if (xhr) {
        json = $.parseJSON(xhr.responseText);
    } else {
        json = $.parseJSON($(event.target).contents().text());
    }
    /* ... */
}
```

### onAbort
A callback function that is called when the file upload has been cancelled.  
**Note:** The functionality to cancel uploads is only implemented by the jQuery File Upload UI Plugin. 

* Type: *function*
* Arguments:
    1. event: XHR onload event object / null (legacy browsers).
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    if (handler.uploadRow) {
        handler.uploadRow.fadeOut(function () {
            $(this).remove();
        });
    }
}
```

### onError
A callback function that is called on XHR upload or JSON parsing errors.  
**Note:** JSON parsing is only implemented by the jQuery File Upload UI Plugin.

* Type: *function*
* Arguments:
    1. event: XHR onerror event object / JSON parsing error event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow`, `handler.progressbar` and `handler.originalEvent` with references to the uploadRow and progressbar as well as the original onLoad event on a JSON parsing error.
* Example:
```js
function (event, files, index, xhr, handler) {
    // For JSON parsing errors, the load event is saved as handler.originalEvent:
    if (handler.originalEvent) {
        /* handle JSON parsing errors ... */
    } else {
        /* handle XHR upload errors ... */
    }
}
```

### init
This callback function is called as soon as files have been selected or dropped.  
If not set, the upload starts automatically.  
If set, the upload starts when the callBack parameter is called.  
The jQuery File Upload UI plugin makes use of this callback and provides an equivalent with the initCallBack option.

* Type: *function*
* Arguments:
    1. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    2. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    3. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    4. callBack: The function to be called to start the file upload. Accepts an object as argument which allows to override current settings.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.
* Example:
```js
function (files, index, xhr, callBack, settings) {
    callBack({url: '/path/to/upload/handler.json'});
}
```

### onDocumentDragEnter
This callback function is called when files are dragged into the document window.

* Type: *function*
* Arguments:
    1. event: dragenter event object.

### onDocumentDragOver
This callback function is called when files are dragged inside the document window.

* Type: *function*
* Arguments:
    1. event: dragover event object.

### onDocumentDragLeave
This callback function is called when files are dragged outside the document window.

* Type: *function*
* Arguments:
    1. event: dragleave event object.

### onDragEnter
This callback function is called when files are dragged into the dropZone area.

* Type: *function*
* Arguments:
    1. event: dragenter event object.

### onDragOver
This callback function is called when files are dragged inside the dropZone area.

* Type: *function*
* Arguments:
    1. event: dragover event object.

### onDragLeave
This callback function is called when files are dragged outside the dropZone area.

* Type: *function*
* Arguments:
    1. event: dragleave event object.

### onDrop
This callback function is called when files are dropped on the dropZone area.

* Type: *function*
* Arguments:
    1. event: drop event object.

### onInputClick
This callback function is called when the file upload field is clicked.

* Type: *function*
* Arguments:
    1. event: click event object.

### onInputChange
This callback function is called when files are selected with the file upload field.

* Type: *function*
* Arguments:
    1. event: change event object.

## Additional options available for the jQuery File Upload UI Plugin

### progressSelector
The [jQuery selector](http://api.jquery.com/category/selectors/) used to select the progress element inside the current upload row.

* Type: *String*
* Default: `'.file_upload_progress div'`

### cancelSelector
The [jQuery selector](http://api.jquery.com/category/selectors/) used to select the cancel element inside the current upload row.

* Type: *String*
* Default: `'.file_upload_cancel div'`

### cssClassSmall
The CSS class that is used for the reduzed version of the dropZone.

* Type: *String*
* Default: `'file_upload_small'`

### cssClassLarge
The CSS class that is used for the enlarged version of the dropZone.

* Type: *String*
* Default: `'file_upload_large'`

### cssClassHighlight
The CSS class that is used for the highlighted version of the dropZone.

* Type: *String*
* Default: `'file_upload_highlight'`

### dropEffect
The [jQuery UI effect](http://jqueryui.com/demos/effect/) used to visualize the file drop.  
**Note**: This is only used, if the [jQuery UI](http://jqueryui.com/) library is available.

* Type: *String*
* Default: `'highlight'`

### initProgressBar
Allows to override the progressbar visualization.  
Must return an object providing a [progressbar value method](http://jqueryui.com/demos/progressbar/#method-value).

* Type: *function*
* Arguments:
    1. node: dragover event object.
    2. value: initial progress value (0-100).
* Example:
```js
function (node, value) {
    var progressbar = $('<progress value="' + value + '" max="100"/>').appendTo(node);
    progressbar.progressbar = function (key, value) {
        progressbar.attr('value', value);
    };
    return progressbar;
}
```

### uploadTable
The jQuery object for the upload table.  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Upload rows are appended to this table.  
**Note:** Required for the jQuery File Upload UI Plugin.

* Type: *Object*
* Example:
```js
$('.upload_files')
```

### downloadTable
The jQuery object for the download table (can be the same node as the upload table).  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Download rows are appended to this table.  
**Note:** Required for the jQuery File Upload UI Plugin.

* Type: *Object*
* Example:
```js
$('.download_files')
```

### buildUploadRow
This function is supposed to return the HTML content for the upload row.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the uploadTable.  
**Note:** Required for the jQuery File Upload UI Plugin.

* Type: *function*
* Arguments:
    1. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    2. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
* Example:
```js
function (files, index) {
    var file = files[index];
    return $(
        '<tr style="display:none">' +
        '<td>' + file.name + '<\/td>' +
        '<td class="file_upload_progress"><div><\/div><\/td>' +
        '<td class="file_upload_cancel">' +
        '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
        '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
        '<\/div>' +
        '<\/td>' +
        '<\/tr>'
    );
}
```

### buildDownloadRow
This function is supposed to return the HTML content for the download row.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the downloadTable.  
**Note:** Required for the jQuery File Upload UI Plugin.

* Type: *function*
* Arguments:
    1. file: JSON response with information for the uploaded file.
* Example:
```js
function (file) {
    return $(
        '<tr style="display:none"><td>' + file.name + '<\/td><\/tr>'
    );
}
```

### initCallBack
This callback function is called as soon as files have been selected or dropped and the uploadRow has been added to the uploadTable.  
If not set, the upload starts automatically.  
If set, the upload starts when the callBack parameter is called.  
**Note:** This is the equivalent to the basic file upload's *init* callBack option.

* Type: *function*
* Arguments:
    1. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    2. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    3. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    4. callBack: The function to be called to start the file upload. Accepts an object as argument which allows to override current settings.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       Provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (files, index, xhr, callBack, settings) {
    callBack({url: '/path/to/upload/handler.json'});
}
```

### onComplete
This callback function is called when the upload is complete and after the downloadRow has been added to the downloadTable.  
It allows adding custom functionality on upload completion without having to override and implement the onLoad callback.

* Type: *function*
* Arguments:
    1. event: XHR onload event object / IFrame onload event object (legacy browsers).
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    5. handler: A reference to the uploadHandler, giving access to all handler methods and upload settings.  
       Provides the attributes `handler.response`, `handler.downloadRow` and `handler.progressbar` with references to the JSON response and the downloadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    var json = handler.response;
    /* ... */
}
```
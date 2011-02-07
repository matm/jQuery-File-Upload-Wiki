The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js)) providing the basic File Upload API and an additional plugin providing a user interface API ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).
 
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

**Note:** The advanced user interface version requires four options, as described in the [[Setup]] Guide:

* uploadTable
* downloadTable
* buildUploadRow
* buildDownloadRow

## Options for the basic jQuery File Upload Plugin

### namespace
Allows to use multiple instances of the File Upload Plugin on the same upload form.  
Mutliple instances of the plugin can be used on the same page without having to set different namespaces.

* Type: *String*
* Default: `'file_upload'`

### cssClass
The CSS class that is added on plugin initialization to the dropZone.  
The dropZone is the HTML form or HTML DOM node containing the form and defines the area on which files can be dropped.

* Type: *String*
* Default: `'file_upload'`

### dragDropSupport
If set to *false*, disables the drag and drop support of the plugin.  
This prevents the plugin from adding listeners for drag and drop events but does not prevent default browser drag and drop handling.  
For example, some browsers will allow to change the file input field value via drag and drop.

* Type: *boolean*
* Default: *true*

### dropZone
The jQuery DOM node where files can be dropped.  
By default this is the element node on which the plugin is called - the upload form or the container holding the form.

* Type: *Object*
* Example: `$('#drop_zone')`

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

### multiFileRequest
If set to *true*, uploads multiple selected files with one request instead of using one request per file.  
If this option is *true*, the *index* parameter used for the callBacks is *undefined* for browsers which support multiple file selection.   
**Note:** Uploading multiple files with one request requires the multipart option to be set to *true*.

* Type: *boolean*
* Default: *false*

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
    name: a,
    value: 1
  },
  {
    name: b,
    value: 2
  }
]
```

### withCredentials
Indicates whether or not cross-site XMLHttpRequest file uploads should be made using credentials such as cookies or authorization headers.  
Sets the [withCredentials property](https://developer.mozilla.org/en/xmlhttprequest#Properties) on the XMLHttpRequest object.

* Type: *boolean*
* Default: *false*

### forceIframeUpload
If set to *true*, forces the use of iframes for the upload process, even if the browser is capable of [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
This can be useful for cross-site file uploads, if the [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_Access_Control#Access-Control-Allow-Origin) header cannot be set for the server-side upload handler which is required for cross-site [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.

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
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
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
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    var json = handler.parseResponse(xhr);
    /* ... */
}
```

### onAbort
A callback function that is called when the file upload has been cancelled.  
**Note:** The file upload can be canceled by calling the method *abort()* on the xhr parameter.

* Type: *function*
* Arguments:
    1. event: XHR abort event object / Custom abort event object (legacy browsers).
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    handler.removeNode(handler.uploadRow);
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
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow`, `handler.progressbar` and `handler.originalEvent` with references to the uploadRow and progressbar as well as the original onLoad event for the JSON parsing error.
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

### initUpload
This callback function is called as soon as files have been selected or dropped.  
If not set, the upload starts automatically.  
If set, the upload starts when the callBack parameter is called.  
The jQuery File Upload UI plugin makes use of this callback and provides an equivalent with the beforeSend option.  
**Note:** This function is called for each selected/dropped file.

* Type: *function*
* Arguments:
    1. event: drop or input change event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. Set to null for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and allows to override the current upload settings.
    6. callBack: The function to be called to start the file upload.
* Example:
```js
function (event, files, index, xhr, handler, callBack) {
    handler.url = '/path/to/upload/handler.json';
    callBack();
}
```

### onDocumentDragEnter
This callback function is called when files are dragged onto a document node.  
Used by the advanced user interface version to enlarge the dropZone.

* Type: *function*
* Arguments:
    1. event: dragenter event object.

### onDocumentDragLeave
This callback function is called when files are dragged outside of a document node.

* Type: *function*
* Arguments:
    1. event: dragleave event object.

### onDocumentDragOver
This callback function is repeatedly called during a drag operation inside of the document window.  
Used by the advanced user interface version to reduce the dropZone by using a [setTimeout](https://developer.mozilla.org/en/DOM/window.setTimeout) call to check if the drag operation has completed.

* Type: *function*
* Arguments:
    1. event: dragover event object.

### onDocumentDrop
This callback function is called when files are dropped anywhere on the document window.

* Type: *function*
* Arguments:
    1. event: drop event object.

### onDragEnter
This callback function is called when files are dragged into the dropZone area.  
Used by the advanced user interface version to toggle the highlight class of the dropZone.

* Type: *function*
* Arguments:
    1. event: dragenter event object.

### onDragLeave
This callback function is called when files are dragged outside the dropZone area.  
Used by the advanced user interface version to toggle the highlight class of the dropZone.

* Type: *function*
* Arguments:
    1. event: dragleave event object.

### onDragOver
This callback function is repeatedly called during a drag operation inside of the dropZone area.  

* Type: *function*
* Arguments:
    1. event: dragover event object.

### onDrop
This callback function is called when files are dropped onto the dropZone area.  
Used by the advanced user interface version to apply a drop effect and reduce the dropZone.

* Type: *function*
* Arguments:
    1. event: drop event object.

### onChange
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

### uploadTable
The jQuery object for the upload table.  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Upload rows are appended to this table.

* Type: *Object*
* Example:
```js
$('.upload_files')
```

### downloadTable
The jQuery object for the download table (can be the same node as the upload table).  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Download rows are appended to this table.

* Type: *Object*
* Example:
```js
$('.download_files')
```

### buildUploadRow
This function is supposed to return the HTML content for the upload row as a jQuery object.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the uploadTable.  
**Note:** The returned jQuery object must have a length of 1, containing only one top level element.

* Type: *function*
* Arguments:
    1. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    2. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
* Example:
```js
function (files, index) {
    var file = files[index];
    return $(
        '<tr>' +
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
This function is supposed to return the HTML content for the download row as a jQuery object.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the downloadTable.  
**Note:** The returned jQuery object must have a length of 1, containing only one top level element.

* Type: *function*
* Arguments:
    1. file: JSON response with information for the uploaded file.
* Example:
```js
function (file) {
    return $(
        '<tr><td>' + file.name + '<\/td><\/tr>'
    );
}
```

### addNode
Allows to override the way the upload/download rows are added to the upload/download tables.

* Type: *function*
* Arguments:
    1. parentNode: The parent jQuery DOM element where the new node should be added.
    2. node: The jQuery DOM element to add.
    3. callBack: An optional method to execute after the node has been added.
* Example:
```js
function (parentNode, node, callBack) {
    parentNode.append(node);
    if (typeof callBack === 'function') {
        callBack();
    }
}
```

### removeNode
Allows to override the way the upload row is removed after the upload has completed or has been canceled.

* Type: *function*
* Arguments:
    1. node: The jQuery DOM element to add.
    2. callBack: An optional method to execute after the node has been removed.
* Example:
```js
function (node, callBack) {
    if (node) {
        node.remove();
    }
    if (typeof callBack === 'function') {
        callBack();
    }
}
```

### cancelUpload
Allows to override the method to cancel uploads.

* Type: *function*
* Arguments:
    1. event: A click event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       Provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Default:
```js
function (event, files, index, xhr, handler) {
    var readyState = xhr.readyState;
    xhr.abort();
    // If readyState is below 2, abort() has no effect:
    if (isNaN(readyState) || readyState < 2) {
        handler.onAbort(event, files, index, xhr, handler);
    }
}
```

### initProgressBar
Allows to override the progressbar visualization.  
Must return an object providing a [progressbar value method](http://jqueryui.com/demos/progressbar/#method-value).

* Type: *function*
* Arguments:
    1. node: The jQuery DOM element where the progress bar should be displayed.
    2. value: initial progress value (0-100).
* Default:
```js
function (node, value) {
    if (typeof node.progressbar === 'function') {
        return node.progressbar({
            value: value
        });
    } else {
        var progressbar = $('<progress value="' + value + '" max="100"/>').appendTo(node);
        progressbar.progressbar = function (key, value) {
            progressbar.attr('value', value);
        };
        return progressbar;
    }
}
```

### beforeSend
This callback function is called as soon as files have been selected or dropped and the uploadRow has been added to the uploadTable.  
If not set, the upload starts automatically.  
If set, the upload starts when the callBack parameter is called.  
**Note:** This is the equivalent to the basic file upload's *initUpload* callBack option and the function is therefore called for each selected/dropped file.

* Type: *function*
* Arguments:
    1. event: drop or input change event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and allows to override the current upload settings.
       Provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
    6. callBack: The function to be called to start the file upload.
* Example:
```js
function (event, files, index, xhr, handler, callBack) {
    handler.url = '/path/to/upload/handler.json';
    callBack();
}
```

### parseResponse
Allows to override the way the File Upload response is parsed.  
The return value is used as parameter for the buildDownloadRow method call.

* Type: *function*
* Arguments:
    1. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
* Default:
```js
function (xhr) {
    if (typeof xhr.responseText !== 'undefined') {
        return $.parseJSON(xhr.responseText);
    } else {
        // Instead of an XHR object, an iframe is used for legacy browsers:
        return $.parseJSON(xhr.contents().text());
    }
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
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       Provides the attributes `handler.response` and `handler.downloadRow` with references to the JSON response and the downloadRow.
* Example:
```js
function (event, files, index, xhr, handler) {
    var json = handler.response;
    /* ... */
}
```

### dropZoneEnlarge
Allows to define a custom method to enlarge the dropZone.

* Type: *function*
* Arguments: none

### dropZoneReduce
Allows to define a custom method to reduce the dropZone.

* Type: *function*
* Arguments: none

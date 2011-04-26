The jQuery File Upload Plugin consists of a basic version ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js)) providing the basic File Upload API and an additional plugin providing a user interface API ([jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).
 
This page lists the various options that can be set for the plugin and examples on how to use them.

## Setting options on plugin initialization

The default way to set options is on plugin initialization - an example for the basic version:
```js
$('#file_upload').fileUpload({
    url: '/path/to/upload/handler.json',
    sequentialUploads: true
});
```

And an example for the advanced user interface version:
```js
$('#file_upload').fileUploadUI({
    url: '/path/to/upload/handler.json'
    uploadTable: $('#upload_files'),
    downloadTable: $('#download_files')
});
```

**Note:** The advanced user interface version requires three options, as described in the [[Setup]] Guide:

* uploadTable
* buildUploadRow
* buildDownloadRow

## Setting options after plugin initialization

It is also possible to set options after plugin initialization, similar to jQuery UI widgets:
```js
$('#file_upload').fileUploadUI(
    'option',
    'url',
    '/path/to/upload/handler.json'
);
```

Or multiple options at once:
```js
$('#file_upload').fileUploadUI(
    'option',
    {
        url: '/path/to/upload/handler.json',
        fieldName: 'file_parameter_name'
    }
);
```

## Options for the basic jQuery File Upload Plugin

### namespace
Allows to use multiple instances of the File Upload Plugin on the same upload form.  
Mutliple instances of the plugin can be used on the same page without having to set different namespaces.

* Type: *String*
* Default: `'file_upload'`

### init
Allows to define a function that is run after plugin initialization.  
**Note:** The File Upload UI plugin makes use of this option and provides an alternative with the initExtended option.

* Type: *function*
* Default: `undefined`

### destroy
Allows to define a function that is run before plugin de-initialization.  
**Note:** The File Upload UI plugin makes use of this option and provides an alternative with the destroyExtended option.

* Type: *function*
* Default: `undefined`

### uploadFormFilter
This option allows to filter the selected forms if the plugin is called on a container with multiple forms.  
Accepts any parameter that is suitable for the [jQuery filter](http://api.jquery.com/filter) method.

* Type: *String* or *function* or *object*
* Default: A function returning true so no forms are filtered out:
```js
function (index) {
    return true;
}
```
* Example: `'.upload_form_1'`

### fileInputFilter
This option allows to filter the selected file input fields if the upload form contains multiple.  
Accepts any parameter that is suitable for the [jQuery filter](http://api.jquery.com/filter) method.

* Type: *String* or *function* or *object*
* Default: A function returning true so no file input fields are filtered out:
```js
function (index) {
    return true;
}
```
* Example: `'.file_2'`

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
Accepts a String or a function returning a String.

* Type: *String* or *function*
* Default: A function returning the action attribute of the file upload form:
```js
function (form) {
    return form.attr('action');
}
```
* Example: `'/path/to/upload/handler.json'`

### method
The method of the HTTP request used to send the file(s) to the server.  
Can be *POST* (multipart/formdata file upload) or *PUT* (streaming file upload).  
Accepts a String or a function returning a String.

* Type: *String* or *function*
* Default: A function returning the method attribute of the file upload form (*POST*):
```js
function (form) {
    return form.attr('method');
}
```
* Example: `'PUT'`

### fieldName
The parameter name used to submit the file(s) to the server.  
Accepts a String or a function returning a String.

* Type: *String* or *function*
* Default: A function returning the name attribute of the file input field:
```js
function (input) {
    return input.attr('name');
}
```
* Example: `'file'`

### formData
Allows to define additional parameters, that are send with the file(s) to the server url.  
Accepts an Array of Objects with name and value attributes, a Function returning such an Array or a simple Object.  
**Note:** Additional form data is ignored when the multipart option is set to *false*.

* Type: *Array*, *Object* or *function*
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

### requestHeaders
Allows to define additional request headers, that are send with each file upload request to the server url.  
Accepts an Array of Objects with name and value attributes, or a simple Object.  
**Note:** Additional requestHeaders can only be added to [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) uploads, not to iframe based uploads.  
If you are making up your own HTTP header, you MUST put a X- in front of the name.

* Type: *Array* or *Object*
* Default: The basic file upload doesn't set this option (*undefined*), but the advanced user interface version uses the following default to make sure the server sends JSON as response type: 
```js
{'Accept': 'application/json, text/javascript, */*; q=0.01'}
```
* Example:
```js
[
  {
    name: 'Accept',
    value: 'application/json, text/javascript, */*; q=0.01'
  },
  {
    name: 'X-CSRF-Token',
    value: 'token'
  }
]
```

### multipart
If set to *false*, streams the file content to the server url instead of sending a [RFC 2388](http://www.ietf.org/rfc/rfc2388.txt) multipart message.  
Non-multipart uploads are also referred to as [HTTP PUT file upload](http://de.php.net/manual/en/features.file-upload.put-method.php).  
**Notes:**  
Setting the *multipart* option to *false* has only an effect on browsers supporting XHR file uploads.  
Form data is ignored when the multipart option is set to *false*.

* Type: *boolean*
* Default: *true*

### multiFileRequest
If set to *true*, uploads multiple selected files with one request instead of using one request per file.  
If this option is *true*, the *index* parameter used for the callBacks is *undefined* for browsers which support multiple file selection.   
**Note:** Uploading multiple files with one request requires the multipart option to be set to *true* (the default).

* Type: *boolean*
* Default: *false*

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

### sequentialUploads
By default, the plugin uploads multiple files simultaneously and asynchronously.  
However it is possible to force a sequential upload, that is starting the upload of the second selected file after the upload of the first one has completed, starting the upload of the third file after the second file has been uploaded and so on, by setting this option to *true*.

* Type: *boolean*
* Default: *false*

### maxChunkSize
To upload large files in smaller chunks, set this option to a preferred maximum chunk size.  
If set to *0* or *null*, files will be uploaded as a whole. See [[Chunked Uploads]].  
**Notes**:  
Only browsers supporting the [Blob API](https://developer.mozilla.org/en/DOM/Blob) will respect this setting, other browsers will always upload complete files.  
This setting is ignored if the option *multiFileRequest* is set to *true*.

* Type: *integer*
* Default: *null*

### uploadedBytes
When a non-multipart upload or a chunked multipart upload has been aborted, this option can be used to resume the upload by setting it to the size of the already uploaded bytes. See [[Chunked Uploads]].  

* Type: *integer*
* Default: *undefined*

### maxFileReaderSize
This option limits the size of files that are transferred as *multipart/form-data* via XHR using the [FileReader](https://developer.mozilla.org/en/DOM/FileReader) interface, when the [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) interface is not available.  
If the file size is bigger than *maxFileReaderSize*, the file is submitted using the iframe method.
The default *maxFileReaderSize* setting is 50 MB to address lockup problems in Firefox 3.6.

* Type: *integer*
* Default: *50000000*

### resumeUpload
This callback function is called before uploading the next chunk when *maxChunkSize* has been set.  
If defined, the upload resumes when the callBack parameter is called.

* Type: *function*
* Arguments:
    1. event: XHR onload event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and allows to override the current upload settings.
    6. callBack: The function to be called to start the next chunk upload.
* Example:
```js
function (event, files, index, xhr, handler, callBack) {
    handler.url = '/path/to/upload/handler.json';
    callBack();
}
```

### onSend
A callback function that is called on upload start.  
The jQuery File Upload UI Plugin makes use of this callback to set the progress bar to a full animated view for browsers which don't support progress events.  
**Note:** If this callback returns *false* the upload will not start.

* Type: *function*
* Arguments:
    1. event: drop or input change event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Example:
```js
function (event, files, index, xhr, handler) {
    if (!xhr.upload && handler.progressbar) {
        handler.progressbar.progressbar(
            'value',
            100 // indeterminate progress displayed by a full animated progress bar
        );
    }
}
```

### onProgress
A callback function that is called on upload progress.  
**Note:** This is only called for browsers which support the [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object as well as XMLHttpRequestUpload. Also, the browser must support either the [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) or [FileReader](https://developer.mozilla.org/en/DOM/FileReader) interfaces or the multipart option has to be set to *false*.  
The jQuery File Upload UI Plugin makes use of this callback to update the progress bar. If you override this setting, you need to update the progress bar yourself.

* Type: *function*
* Arguments:
    1. event: XHR onprogress event object.
    2. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects.
    3. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    4. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload.
    5. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.  
       The jQuery File Upload UI Plugin provides the attributes `handler.uploadRow` and `handler.progressbar` with references to the uploadRow and progressbar.
* Default:
```js
function (event, files, index, xhr, handler) {
    if (handler.progressbar) {
        handler.progressbar.progressbar(
            'value',
            parseInt(event.loaded / event.total * 100, 10)
        );
    }
}
```

### onProgressAll
A callback function that is called on overall upload progress.  
The jQuery File Upload UI Plugin makes use of this callback to update the overall progress bar. If you override this setting, you need to update the progress bar yourself.

* Type: *function*
* Arguments:
    1. event: progress event object, or a similar object providing the attributes lengthComputable/loaded/total).
    2. list: Array of argument lists of all current uploads.
* Default:
```js
function (event, list) {
    if (uploadHandler.progressbarAll && event.lengthComputable) {
        uploadHandler.progressbarAll.progressbar(
            'value',
            parseInt(event.loaded / event.total * 100, 10)
        );
    }
};
```

### onLoad
A callback function that is called when the client receives the server response for the file upload.    
The jQuery File Upload UI Plugin makes use of this callback to remove the uploadRow, parse the JSON response and add the downloadRow.  
Customize this method if you don't want to show a downloadRow depending on the server response. See also parseResponse for more information on displaying errors.

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
    var json;
    if (typeof xhr.responseText !== undef) {
        json = $.parseJSON(xhr.responseText);
    } else {
        // Instead of an XHR object, an iframe is used for legacy browsers:
        json = $.parseJSON(xhr.contents().text());
    }
    /* ... */
}
```

### onLoadAll
A callback function that is called when the client received the server responses for all current file uploads. 
The jQuery File Upload UI Plugin makes use of this callback to hide and reset the overall progressbar.

* Type: *function*
* Arguments:
    1. list: Array of arguments arrays (without the event argument) of all onLoad callBacks.

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

### initExtended
Allows to define a function that is run after plugin initialization.

* Type: *function*
* Default: `undefined`

### destroyExtended
Allows to define a function that is run before plugin de-initialization.

* Type: *function*
* Default: `undefined`

### imageTypes
A [regular expression](https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/regexp) limiting the file types for previews of local files to be uploaded to browser displayable image types.

* Type: *RegExp*
* Default: ```/^image\/(gif|jpeg|png)$/```

### previewMaxWidth
A maximum width for the preview images.

* Type: *integer*
* Default: ```80```

### previewMaxHeight
A maximum height for the preview images.

* Type: *integer*
* Default: ```80```

### previewLoadDelay
A delay in milliseconds, until the preview images are loaded.  
This setting can be useful for performance optimizations.

* Type: *integer*
* Default: ```100```

### previewAsCanvas
By default, preview images are rendered as [canvas](https://developer.mozilla.org/en/html/canvas) element, if possible. Set this option to *false* to force preview images into *img* elements.

* Type: *boolean*
* Default: ```true```

### previewSelector
The [jQuery selector](http://api.jquery.com/category/selectors/) used to select the container for a preview image of the file to be uploaded.  
Preview images can be loaded and displayed for local image files on browsers supporting the [URL](https://developer.mozilla.org/en/DOM/window.URL), webkitURL or [FileReader](https://developer.mozilla.org/en/DOM/FileReader) interfaces.  
If the previewSelector does not match any container elements or the file is no image, no preview image is loaded.

* Type: *String*
* Default: `'.file_upload_preview'`

### progressSelector
The [jQuery selector](http://api.jquery.com/category/selectors/) used to select the progress element inside the current upload row.

* Type: *String*
* Default: `'.file_upload_progress div'`

### cancelSelector
The [jQuery selector](http://api.jquery.com/category/selectors/) used to select the cancel element inside the current upload row.

* Type: *String*
* Default: `'.file_upload_cancel button'`

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
The jQuery object for the upload table or a function returning such an object.  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Upload rows are appended to this table.  
If no download table is specified, also serves as uploadTable, by replacing uploadRows with downloadRows.

* Type: *Object* or *Function*
* Example:
```js
$('#upload_files')
```

### downloadTable
The jQuery object for the download table (can be the same node as the upload table) or a function returning such an object.  
This does not strictly have to be a HTML table element, but can also be a list or any other element that allows to append nodes to it.  
Download rows are appended to this table.  
If no download table is specified, the uploadTable also serves as downloadTable, by replacing uploadRows with downloadRows.

* Type: *Object* or *Function*
* Example:
```js
$('#download_files')
```

### buildUploadRow
This function is supposed to return the HTML content for the upload row as a jQuery object.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the uploadTable.  
**Note:** The returned jQuery object must have a length of 1, containing only one top level element.

* Type: *function*
* Arguments:
    1. files: Array of all [File](https://developer.mozilla.org/en/DOM/File) objects. For legacy browsers, only the file name is populated.
    2. index: The index of the current [File](https://developer.mozilla.org/en/DOM/File) object.
    3. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.
* Example:
```js
function (files, index) {
    return $('<tr><td>' + files[index].name + '<\/td>' +
            '<td class="file_upload_progress"><div><\/div><\/td>' +
            '<td class="file_upload_cancel">' +
            '<button class="ui-state-default ui-corner-all" title="Cancel">' +
            '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
            '<\/button><\/td><\/tr>');
}
```

### buildDownloadRow
This function is supposed to return the HTML content for the download row as a jQuery object.  
The content does not strictly have to be a HTML table row, but can be any element that can be appended to the downloadTable.  
**Note:** The returned jQuery object must have a length of 1, containing only one top level element.

* Type: *function*
* Arguments:
    1. file: JSON response with information for the uploaded file.
    2. handler: A reference to the uploadHandler, gives access to all handler methods and upload settings.
* Example:
```js
function (file) {
    return $('<tr><td>' + file.name + '<\/td><\/tr>');
}
```

### progressAllNode
The jQuery object for the overall progress bar.

* Type: *Object*
* Example:
```js
$('#file_upload_progress div')
```

### addNode
Allows to override how elements (e.g. the download rows) are added to the page by the plugin.  
By default this method makes use of jQuery's [fadeIn](http://api.jquery.com/fadeIn) method.

* Type: *function*
* Arguments:
    1. parentNode: The parent jQuery DOM element where the new node should be added.
    2. node: The jQuery DOM element to add.
    3. callBack: An optional method to execute after the node has been added and faded in.
* Example:
```js
function (parentNode, node, callBack) {
    if (parentNode) {
        parentNode.append(node);
    }
    if (typeof callBack === 'function') {
        callBack();
    }
}
```

### removeNode
Allows to override the way elements (e.g. the upload rows) are removed from the page by the plugin.  
By default this method makes use of jQuery's [fadeOut](http://api.jquery.com/fadeOut) method.

* Type: *function*
* Arguments:
    1. node: The jQuery DOM element to add.
    2. callBack: An optional method to execute after the node has been faded out and removed.
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

### replaceNode
Allows to override the way elements on the page are replaced by the plugin.  
If *uploadTable* and *downloadTable* are referring to the same table, download rows replace upload rows.  
This method makes use of jQuery's [fadeIn](http://api.jquery.com/fadeIn) and [fadeOut](http://api.jquery.com/fadeOut) methods.

* Type: *function*
* Arguments:
    1. oldNode: The jQuery DOM element to remove.
    2. newNode: The jQuery DOM element to add.
    3. callBack: An optional method to execute after the replacement has been done.
* Example:
```js
function (oldNode, newNode, callBack) {
    if (oldNode && newNode) {
        oldNode.replaceWith(newNode);
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
    if (typeof readyState !== 'number' || readyState < 2) {
        handler.onAbort(event, files, index, xhr, handler);
    }
}
```

### initProgressBar
Allows to override the progressbar visualization.  
Must return an object providing a [progressbar value method](http://jqueryui.com/demos/progressbar/#methods).

* Type: *function*
* Arguments:
    1. node: The jQuery DOM element where the progress bar should be displayed.
    2. value: initial progress value (0-100).
* Default:
```js
function (node, value) {
    if (!node || !node.length) {
        return null;
    }
    if (typeof node.progressbar === func) {
        return node.progressbar({
            value: value
        });
    } else {
        node.addClass('progressbar')
            .append($('<div/>').css('width', value + '%'))
            .progressbar = function (key, value) {
                return this.each(function () {
                    if (key === 'destroy') {
                        $(this).removeClass('progressbar').empty();
                    } else {
                        $(this).children().css('width', value + '%');
                    }
                });
            };
        return node;
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
Should an error occur in parsing the json (it could be an html error page was returned from a runtime error on the server), the File Upload UI still removes the uploadRow and calls onError, without adding a downloadRow. If you would like to show errors against the item in the table, simply override this method in a try/catch block and return custom json to be handled in the buildDownloadRow code.

* Type: *function*
* Arguments:
    1. xhr: The [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) object for the current file upload. A jQuery iframe node for legacy browsers.
    2. handler: A reference to the uploadHandler, gives access to all handler methods and allows to override the current upload settings.
* Default:
```js
function (xhr, handler) {
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

### onCompleteAll
A callback function that is called when the client received the server responses for all current file uploads and after all rows have been added, removed and faded in and out.

* Type: *function*
* Arguments:
    1. list: Array of arguments arrays (without the event argument) of all onLoad callBacks.

### dropZoneEnlarge
Allows to define a custom method to enlarge the dropZone.

* Type: *function*
* Arguments: none

### dropZoneReduce
Allows to define a custom method to reduce the dropZone.

* Type: *function*
* Arguments: none

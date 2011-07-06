# Chunked Uploads (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

Chunked Uploads are supported "out-of-the-box" by all browsers supporting [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads and the [Blob API](https://developer.mozilla.org/en/DOM/Blob).  
Chunked Uploads are enabled if the *maxChunkSize* option is set to a value greater than 0, e.g. 10000000 bytes (10 MB):
```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    buildUploadRow: function (files, index, handler) {
        return $('<tr><td>' + files[index].name + '<\/td>' +
                '<td class="file_upload_progress"><div><\/div><\/td>' +
                '<td class="file_upload_cancel">' +
                '<button class="ui-state-default ui-corner-all" title="Cancel">' +
                '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                '<\/button><\/td><\/tr>');
    },
    buildDownloadRow: function (file, handler) {
        return $('<tr><td>' + file.name + '<\/td><\/tr>');
    },
    multipart: false,
    maxChunkSize: 10000000
});
```
In the example above, the *multipart* option is also set to *false*. This is due to Gecko 2.0 (Firefox 4 etc.) adding blobs with an empty filename when building a multipart upload request using the FormData interface - see [Bugzilla entry #649150](https://bugzilla.mozilla.org/show_bug.cgi?id=649150).  
Several server-side frameworks (including PHP and Django) cannot handle multipart file uploads with empty filenames.

## How to resume aborted file uploads
File uploads can be resumed by making use of the *beforeSend* and *uploadedBytes* options.  
In the following example, the uploaded filesize for a given file is retrieved with an AJAX call and then used to set the *uploadedBytes* option on the *handler* object before starting the upload process by invoking the *callBack* method:
```js
$('#file_upload').fileUploadUI({
    /* ... */
    multipart: false,
    maxChunkSize: 10000000,
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        if (typeof index !== 'undefined') {
            $.getJSON(
                handler.url,
                {file: files[index].name},
                function (file) {
                    if (file && file.size !== files[index].size) {
                        handler.uploadedBytes = file.size;
                    }
                    callBack();
                }
            );
        } else {
            callBack();
        }
    }
});
```
In the above example, the *handler.url* (usually the upload form *action* attribute) is supposed to return a JSON object holding file information when called via a GET request and a given file parameter with the name of the previously aborted file upload (see the [PHP Upload Handler](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/upload.php) for an implementation example).
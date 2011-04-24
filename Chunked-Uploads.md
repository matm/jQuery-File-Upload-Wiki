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
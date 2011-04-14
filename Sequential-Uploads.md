# How to force sequential uploads for multiple selected files
By default, the plugin uploads multiple files simultaneously.  
However it is possible to force a sequential upload, that is starting the upload of the second selected file after the upload of the first one has completed, starting the upload of the third file after the second file has been uploaded and so on.

Since version 4.1, sequential uploads are supported out of the box by setting the *sequentialUploads* option to *true* (see [[Options]]):
```js
/*global $ */
$(function () {
    $('#file_upload').fileUploadUI({
        uploadTable: $('#files'),
        buildUploadRow: function (files, index) {
            return $('<tr><td>' + files[index].name + '<\/td>' +
                    '<td class="file_upload_progress"><div><\/div><\/td>' +
                    '<td class="file_upload_cancel">' +
                    '<button class="ui-state-default ui-corner-all" title="Cancel">' +
                    '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                    '<\/button><\/td><\/tr>');
        },
        buildDownloadRow: function (file) {
            return $('<tr><td>' + file.name + '<\/td><\/tr>');
        },
        sequentialUploads: true
    });
});
```

See also [[How to queue files and start uploads with a button click]].
By default, the plugin uploads multiple selected (or dropped) files simultaneously and asynchronously.  
If you don't want to use [[Sequential Uploads]] or send multiple files with one request (see [[How to upload multiple files with one request]]) but still want to call a method after all selected files have been uploaded, you can use make use of the following code:
```js
/*global $ */
$(function () {
    $('#file_upload').fileUploadUI({
        uploadTable: $('#files'),
        downloadTable: $('#files'),
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
        onComplete: function (event, files, index, xhr, handler) {
            handler.onCompleteAll(files);
        },
        onAbort: function (event, files, index, xhr, handler) {
            handler.removeNode(handler.uploadRow);
            handler.onCompleteAll(files);
        },
        onCompleteAll: function (files) {
            // The files array is a shared object between the instances of an upload selection.
            // We extend it with a uploadCounter to calculate when all uploads have completed:
            if (!files.uploadCounter) {
                files.uploadCounter = 1;  
            } else {
                files.uploadCounter = files.uploadCounter + 1;
            }
            if (files.uploadCounter === files.length) {
                /* your code after all uplaods have completed */
            }
        }
    });
});
```
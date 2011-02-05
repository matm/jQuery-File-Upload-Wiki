By default, the plugin uploads multiple selected (or dropped) files simultaneously and asynchronously.  
If you don't want to use [[Sequential Uploads]] or send multiple files with one request (see [[How to upload multiple files with one request]]) but still want to call a method after all selected files have been uploaded, you can use make use of the following code:
```js
/*global $ */
$(function () {
    var uploadCounter = 0,
        onCompleteAll = function () {
            /* your code after all uplaods have completed */
        };
    $('.upload').fileUploadUI({
        uploadTable: $('.upload_files'),
        downloadTable: $('.download_files'),
        buildUploadRow: function (files, index) {/**/},
        buildDownloadRow: function (file) {/**/},
        onComplete: function (event, files, index, xhr, handler) {
            uploadCounter = uploadCounter + 1;
            if (uploadCounter === files.length) {
                uploadCounter = 0;
                onCompleteAll();
            }
        },
        onAbort: function (event, files, index, xhr, handler) {
            handler.removeNode(handler.uploadRow);
            uploadCounter = uploadCounter + 1;
            if (uploadCounter === files.length) {
                uploadCounter = 0;
                onCompleteAll();
            }
        }
    });
});
```

**Note:** A user might start another upload which the code above doesn't take into account.
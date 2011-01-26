# How to force sequential uploads for multiple selected files
By default, the plugin uploads multiple files simultaneously.  
However it is possible to force a sequential upload, that is starting the upload of the second selected file after the upload of the first one has completed, starting the upload of the third file after the second file has been uploaded and so on.

The following is an example implementation, making use of the *beforeSend*, *onComplete* and *onAbort* [[Options]] and a JavaScript array with a custom *start* method to sequence the uploads:
```js
/*global $ */
$(function () {
    var uploadSequence = [];
    uploadSequence.start = function (index) {
        var next = this[index];
        if (next) {
            next();
            this[index] = null;
        }
    };
    $('.upload').fileUploadUI({
        uploadTable: $('.upload_files'),
        downloadTable: $('.download_files'),
        buildUploadRow: function (files, index) {/**/},
        buildDownloadRow: function (file) {/**/},
        beforeSend: function (event, files, index, xhr, handler, callBack) {
            uploadSequence.push(callBack);
            if (index === 0) {
                uploadSequence.splice(0, uploadSequence.length - 1);
            } else if (index + 1 === files.length) {
                uploadSequence.start(0);
            }
        },
        onComplete: function (event, files, index, xhr, handler) {
            uploadSequence.start(index + 1);
        },
        onAbort: function (event, files, index, xhr, handler) {
            handler.removeNode(handler.uploadRow);
            uploadSequence[index] = null;
            uploadSequence.start(index + 1);
        }
    });
});
```
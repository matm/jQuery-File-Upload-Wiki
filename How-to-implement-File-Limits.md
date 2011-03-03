This page explains how to implement client-side limits for the files being uploaded, e.g. to restrict **file sizes** and **file types** or the number of selected files.  
To enforce these constraints, you will still have to implement the same limitations within your server-side handler, as any client-side restrictions can be bypassed.

## Example on how to limit the file size to 5 MB:
To restrict the file size you can make use of the *beforeSend* option like this:
```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    downloadTable: $('#files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        if (files[index].size > 5000000) {
            handler.uploadRow.find('.file_upload_progress').html('FILE TOO BIG!');
            setTimeout(function () {
                handler.removeNode(handler.uploadRow);
            }, 10000);
            return;
        }
        callBack();
    }
});
```

The equivalent for the basic file upload makes use of the *initUpload* option:
```js
$('.upload').fileUpload({
    initUpload: function (event, files, index, xhr, handler, callBack) {
        if (files[index].size > 5000000) {
            /* Show 'FILE TOO BIG!' message to user ... */
            return;
        }
        callBack();
    }
});
```

**Note:** For legacy browsers, the files[index].size value is always set to null, so the above checks will always pass.

## Example on how to limit the file types to web suitable images:
To restrict the file types you can make use of the *beforeSend* option like this:
```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    downloadTable: $('#files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        var regexp = /\.(png)|(jpg)|(gif)$/i;
        // Using the filename extension for our test,
        // as legacy browsers don't report the mime type
        if (!regexp.test(files[index].name)) {
            handler.uploadRow.find('.file_upload_progress').html('ONLY IMAGES ALLOWED!');
            setTimeout(function () {
                handler.removeNode(handler.uploadRow);
            }, 10000);
            return;
        }
        callBack();
    }
});
```

The equivalent for the basic file upload makes use of the *init* option:
```js
$('#file_upload').fileUpload({
    initUpload: function (event, files, index, xhr, handler, callBack) {
        var regexp = /\.(png)|(jpg)|(gif)$/i;
        // Using the filename extension for our test,
        // as legacy browsers don't report the mime type
        if (!regexp.test(files[index].name)) {
            /* Show 'ONLY IMAGES ALLOWED!' message to user ... */
            return;
        }
        callBack();
    }
});
```

## Example on how to limit the number of selected files:
```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    downloadTable: $('#files'),
    buildUploadRow: function (files, index) {
        if (index > 9) {
            return null;
        }
        /* ... */
    },
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        if (index > 9) {
            /* Show 'PLEASE ONLY SELECT UP TO 10 FILES!' message to user ... */
            return;
        }
        callBack();
    }
});
```
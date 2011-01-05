This page explains how to implement client-side limits for the files being uploaded, e.g. to restrict **file sizes** and **file types**.  
To enforce these constraints, you will still have to implement the same limitations within your server-side handler, as any client-side restrictions can be bypassed.

## Example on how to limit the file size to 5 MB:
To restrict the file size you can make use of the *initCallBack* option like this:
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    initCallBack: function (files, index, xhr, callBack, settings) {
        if (files[index].size > 5000000) {
            settings.uploadRow.find('.file_upload_progress').html('FILE TOO BIG!');
            setTimeout(function () {
                settings.uploadRow.fadeOut(function () {
                    $(this).remove();
                });
            }, 10000);
            return;
        }
        callBack();
    }
});
```

The equivalent for the basic file upload makes use of the *init* option:
```js
$('.upload').fileUpload({
    init: function (files, index, xhr, callBack, settings) {
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
To restrict the file types you can make use of the *initCallBack* option like this:
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    initCallBack: function (files, index, xhr, callBack, settings) {
        var regexp = /\.(png)|(jpg)|(gif)$/i;
        // Using the filename extension for our test,
        // as legacy browsers don't report the mime type
        if (!regexp.test(files[index].name)) {
            settings.uploadRow.find('.file_upload_progress').html('ONLY IMAGES ALLOWED!');
            setTimeout(function () {
                settings.uploadRow.fadeOut(function () {
                    $(this).remove();
                });
            }, 10000);
            return;
        }
        callBack();
    }
});
```

The equivalent for the basic file upload makes use of the *init* option:
```js
$('.upload').fileUpload({
    init: function (files, index, xhr, callBack, settings) {
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
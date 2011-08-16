# How to implement file limits (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

This page explains how to implement client-side limits for the files being uploaded, e.g. to restrict **file sizes** and **file types** or the number of selected files.  
**Note**: To enforce these constraints, you will still have to implement the same limitations within your server-side handler, as any client-side restrictions can be bypassed.  
**Note2:** If you want to allow only 1 file just remove the "multiple" attribute from the file input element in your HTML markup.

## File limits example for the jQuery File Upload UIX version:

```js
$('#file_upload').fileUploadUIX({
    maxFileSize: 5000000, // Size in Bytes - 5 MB
    minFileSize: 100000, // Size in Bytes - 100 KB
    maxNumberOfFiles: 5,
    // German localization:
    locale: {
        'File is too big': 'Datei ist zu groß',
        'File is too small': 'Datei ist zu klein',
        'Filetype not allowed': 'Dateityp nicht erlaubt',
        'Max number exceeded': 'Maximalanzahl überschritten'
    },
    // Accept only image file types:
    acceptFileTypes: /(png)|(jpe?g)|(gif)$/i
});
```

## Example on how to limit the file size to 5 MB:
To restrict the file size you can make use of the *beforeSend* option like this:

```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
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

### How to restrict the file selection dialog to show only specific file types?
You can use the accept attribute of the file input field to limit the file type selection, though this seems to be supported only on Google Chrome and Opera.  
An example limiting files to PNG images:

```html
<input type="file" name="file" accept="image/png" multiple>
```

Note that this will not limit files added by drag&drop and is not supported across all browsers.

## Example on how to avoid uploading empty files and folders:

```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        if (files[index].size === 0) {
            handler.uploadRow.find('.file_upload_progress').html('FILE IS EMPTY!');
            setTimeout(function () {
                handler.removeNode(handler.uploadRow);
            }, 10000);
            return;
        }
        callBack();
    }
});
```

## Example on how to limit the number of uploadable files:

```js
var maxFiles = 10,
    filesCounter = 0;
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    buildUploadRow: function (files, index) {
        if (filesCounter + index + 1 > maxFiles) {
            return null;
        }
        /* ... */
    },
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        if (filesCounter + index + 1 > maxFiles) {
            /* Show 'ONLY' + maxFiles + 'FILES ALLOWED!' message to user ... */
            return;
        }
        filesCounter = filesCounter + 1;
        callBack();
    }
});
```

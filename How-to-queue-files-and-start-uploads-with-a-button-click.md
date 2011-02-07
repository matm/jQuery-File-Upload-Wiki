By the default, the plugin uploads files automatically, after they have been selected or dropped on the dropZone.  
It is also possible to queue the selected files and only start the uploads after a button click, using the *beforeSend* option (see [[Options]]):
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
    buildUploadRow: function (files, index) {
        var file = files[index];
        return $('<tr><td class="file_upload_start">' +
                '<div class="ui-state-default ui-corner-all ui-state-hover" title="Start Upload">' +
                '<span class="ui-icon ui-icon-circle-arrow-e">Start Upload<\/span>' +
                '<\/div><\/td>' +
                '<td>' + file.name + '<\/td>' +
                '<td class="file_upload_progress"><div><\/div><\/td>' +
                '<td class="file_upload_cancel">' +
                '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
                '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                '<\/div><\/td><\/tr>');
    },
    buildDownloadRow: function (file) {
        return $('<tr><td>' + file.name + '<\/td><\/tr>');
    },
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        handler.uploadRow.find('.file_upload_start div').click(callBack);
    }
});
```

A button to start the upload of all queued files could be added with the following code:
```html
<button id="start_uploads">Start uploads</button>
```
```js
$('#start_uploads').click(function () {
    $('.file_upload_start div').click();
});
```

## How to add thumbnail pictures of image files queued for upload
The following code builds on the previous example and adds a thumbnail picture to the upload row of image files.  
It makes use of a custom *LocalImage* JavaScript function that returns an *Image* object with its *src* attribute pointing to the given *File* object if the browser supports the required APIs.
```js
/*global $, Image, FileReader, URL */
$(function () {
    var LocalImage = function (file, width, height) {
        var img,
            fileReader;
        if (!/^image\/\w+/.test(file.type)) {
            return null;
        }
        img = new Image();
        if (width) {
            img.width = width;
        }
        if (height) {
            img.height = height;
        }
        if (typeof URL !== 'undefined' && typeof URL.createObjectURL === 'function') {
            img.src = URL.createObjectURL(file);
            img.onload = function () {
                URL.revokeObjectURL(this.src);
            };
            return img;
        }
        if (typeof FileReader !== 'undefined') {
            fileReader = new FileReader();
            if (typeof fileReader.readAsDataURL === 'function') {
                fileReader.onload = function (e) {
                    img.src = e.target.result;
                };
                fileReader.readAsDataURL(file);
                return img;
            }
        }
        return null;
    };
    $('.upload').fileUploadUI({
        uploadTable: $('.upload_files'),
        downloadTable: $('.download_files'),
        buildUploadRow: function (files, index) {
            var file = files[index];
            return $('<tr><td class="file_upload_start">' +
                    '<div class="ui-state-default ui-corner-all ui-state-hover" title="Start Upload">' +
                    '<span class="ui-icon ui-icon-circle-arrow-e">Start Upload<\/span>' +
                    '<\/div><\/td>' +
                    '<td class="file_upload_preview"><\/td>' +
                    '<td>' + file.name + '<\/td>' +
                    '<td class="file_upload_progress"><div><\/div><\/td>' +
                    '<td class="file_upload_cancel">' +
                    '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
                    '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                    '<\/div><\/td><\/tr>');
        },
        buildDownloadRow: function (file) {
            return $('<tr><td>' + file.name + '<\/td><\/tr>');
        },
        beforeSend: function (event, files, index, xhr, handler, callBack) {
            handler.uploadRow.find('.file_upload_start div').click(callBack);
            handler.uploadRow.find('.file_upload_preview').append(new LocalImage(files[index], 100));
        }
    });
});
```

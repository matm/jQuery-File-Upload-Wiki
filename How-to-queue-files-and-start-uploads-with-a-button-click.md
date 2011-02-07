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
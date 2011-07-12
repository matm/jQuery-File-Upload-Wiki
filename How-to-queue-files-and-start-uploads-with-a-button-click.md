# How to queue files and start uploads with a button click (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

By the default, the plugin uploads files automatically, after they have been selected or dropped on the dropZone.  
But it is also possible to queue the selected files and only start the uploads after a button click, using the *beforeSend* option (see [[Options]]):

```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    buildUploadRow: function (files, index) {
        return $('<tr><td class="file_upload_preview"><\/td>' +
                '<td>' + files[index].name + '<\/td>' +
                '<td class="file_upload_progress"><div><\/div><\/td>' +
                '<td class="file_upload_start">' +
                '<button class="ui-state-default ui-corner-all" title="Start Upload">' +
                '<span class="ui-icon ui-icon-circle-arrow-e">Start Upload<\/span>' +
                '<\/button><\/td>' +
                '<td class="file_upload_cancel">' +
                '<button class="ui-state-default ui-corner-all" title="Cancel">' +
                '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                '<\/button><\/td><\/tr>');
    },
    buildDownloadRow: function (file) {
        return $('<tr><td>' + file.name + '<\/td><\/tr>');
    },
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        handler.uploadRow.find('.file_upload_start button').click(function () {
            callBack();
            return false;
        });
    }
});
```

A button to start the upload of all queued files could be added with the following code:
```html
<button id="start_uploads">Start uploads</button>
```
```js
$('#start_uploads').click(function () {
    $('.file_upload_start button').click();
    return false;
});
```
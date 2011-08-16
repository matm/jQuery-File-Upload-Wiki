# How to invoke a method after all uploads have completed (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

By default, the plugin uploads multiple selected (or dropped) files simultaneously and asynchronously.  
If you don't want to use [[Sequential Uploads]] or send multiple files with one request (see [[How to upload multiple files with one request]]) but still want to call a method after all selected files have been uploaded, you can use make use of the *onCompleteAll* callBack option:

```js
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
        onCompleteAll: function (list) {
            /* your code after all uploads have completed */
        }
    });
});
```

**Note**:  
The equivalent for the basic file upload is the *onLoadAll* callBack option.
# How to upload multiple files with one request (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

By default, the plugin uploads each selected file with an individual request. The [multiFileRequest option](https://github.com/blueimp/jQuery-File-Upload/wiki/Options) allows to upload multiple files with one request:

```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    buildUploadRow: function (files, index) {
        var fileNames;
        if (typeof index == 'number') {
            fileNames = files[index].name;
        } else {
            fileNames = '';
            for (i = 0; i < files.length; i += 1) {
                fileNames = fileNames + files[i].name + '<br>';
            }
        }
        return $(
            '<tr>' +
            '<td>' + fileNames + '<\/td>' +
            '<td class="file_upload_progress"><div><\/div><\/td>' +
            '<td class="file_upload_cancel">' +
            '<button class="ui-state-default ui-corner-all" title="Cancel">' +
            '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
            '<\/button>' +
            '<\/td>' +
            '<\/tr>'
        );
    },
    buildDownloadRow: function (file) {
        var fileNames;
        if ($.isArray(file)) {
            fileNames = '';
            for (i = 0; i < file.length; i += 1) {
                fileNames = fileNames + file[i].name + '<br>';
            }
        } else {
            fileNames = file.name;
        }
        return $(
            '<tr><td>' + fileNames + '<\/td><\/tr>'
        );
    },
    multiFileRequest: true
});
```
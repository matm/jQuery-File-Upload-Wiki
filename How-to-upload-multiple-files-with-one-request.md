By default, the plugin uploads each selected file with an individual request.  
The [multiFileRequest option](https://github.com/blueimp/jQuery-File-Upload/wiki/Options) allows to upload multiple files with one request:
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
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
            '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
            '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
            '<\/div>' +
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
This page explains how to submit additional Form Data (POST variables) with individual file uploads.  
This can be done using the beforeSend and formData [[Options]], similar to the following example:
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
    buildUploadRow: function (files, index) {
        var file = files[index];
        return $(
            '<tr>' +
            '<td class="file_upload_start">' +
            '<div class="ui-state-default ui-corner-all ui-state-hover" title="Start Upload">' +
            '<span class="ui-icon ui-icon-circle-arrow-n">Start Upload<\/span>' +
            '<\/div>' +
            '<\/td>' +
            '<td>' + file.name + '<\/td>' +
            '<td class="file_upload_desc"><input type="text" title="File description"><\/td>' +
            '<td class="file_upload_progress"><div><\/div><\/td>' +
            '<td class="file_upload_cancel">' +
            '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
            '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
            '<\/div>' +
            '<\/td>' +
            '<\/tr>'
        );
    },
    buildDownloadRow: function (file) {/**/},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        handler.uploadRow.find('.file_upload_start').click(function () {
            handler.formData = {
                description: handler.uploadRow.find('.file_upload_desc input').val()
            };
            callBack();
        });
    }
});
```

In the above example, the uploadRow content returned by buildUploadRow is extended with a button to start the file upload and an input field to add a file description.  
The beforeSend option allows to queue the selected files and add the content of the input field as formData before the upload is started by invoking the callBack method.

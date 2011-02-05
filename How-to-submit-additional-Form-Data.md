This page explains how to submit additional Form Data (POST variables) with your file uploads.  
This can be done either programmatically, or by adding additional input fields to the form.

## Adding additional Form Data for individual file uploads programmatically
Adding additional Form Data programmatically can be done using the beforeSend and formData [[Options]], similar to the following example:
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

## Sending additional Form Data by adding input fields to the upload form
If the formData option (see [[Options]] or the previous example) is not set, the plugin automatically includes all input fields with the upload request.  
The following example will add `example=banana` along with each file upload:
```html
<form class="upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="hidden" name="example" value="banana">
    <input type="file" name="file" multiple>
    <button>Upload</button>
    <div>Upload files</div>
</form>
```

It is also possible to add visible form fields that allow user customizable input:
```html
<form class="upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="text" name="example_text">
    <input type="checkbox" name="example_option">
    <div id="file_upload_container">
        <input type="file" name="file" multiple>
        <button>Upload</button>
        <div>Upload files</div>
    </div>
</form>
```

By default, the whole form is used as dropZone and file selection button, effectively hiding any other input fields.
Therefore, the file input field is wrapped in another container and has to be defined as dropZone:
```js
$('.upload').fileUploadUI({
    uploadTable: $('.upload_files'),
    downloadTable: $('.download_files'),
    buildUploadRow: function (files, index) {/**/},
    buildDownloadRow: function (file) {/**/},
    dropZone: $('#file_upload_container')
});
```
Lastly, some CSS adjustments have to be made, adding "div" in front of some of the selectors:
```css
div.file_upload {
  /* unchanged attributes */
}

div.file_upload_small {
  /* unchanged attributes */
}

div.file_upload_large {
  /* unchanged attributes */
}

div.file_upload_highlight {
  /* unchanged attributes */
}

div.file_upload input {
  /* unchanged attributes */
}

div.file_upload iframe, div.file_upload button {
  /* unchanged attributes */
}
```
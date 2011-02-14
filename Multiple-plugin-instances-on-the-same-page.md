Multiple File Upload Forms on the same webpage are supported out of the box and don't need any additional [[Options]].  

Here is a sample HTML code with two file upload forms on the same page:
```html
<form class="file_upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file" multiple>
    <button>Upload</button>
    <div>Upload files</div>
</form>
<form class="file_upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file" multiple>
    <button>Upload</button>
    <div>Upload files</div>
</form>
<table id="files"></table>
```

And the associated plugin initialization:
```js
/*global $ */
$(function () {
    $('.file_upload').fileUploadUI({
        uploadTable: $('#files'),
        downloadTable: $('#files'),
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
        }
    });
});
```

Note that the plugin initialization does not differ from the basic [[Setup]].  
The only difference is the jQuery selector, applying the plugin call to both forms.
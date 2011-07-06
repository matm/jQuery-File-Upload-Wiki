# Multiple file input fields in one form (v. 4 branch)

**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.

To handle multiple file input fields in one form, you can make use of the *namespace*, *fileInputFilter* and *dropZone* [[Options]] and **multiple plugin instances on the same form**.  
Here is the example HTML code for a form based on the example [[Setup]], but with three different file input fields (and different upload/download tables):
```html
<form id="file_upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <div id="drop_zone_1">
        <input id="file_1" type="file" name="file_1" multiple>
        <div>Upload files</div>
    </div>
    <div id="drop_zone_2">
        <input id="file_2" type="file" name="file_2" multiple>
        <div>Upload files</div>
    </div>
    <div id="drop_zone_3">
        <input id="file_3" type="file" name="file_3" multiple>
        <div>Upload files</div>
    </div>
    <button>Upload</button>
</form>
<table id="files_1" style="background:yellow;"></table>
<table id="files_2" style="background:gold;"></table>
<table id="files_3" style="background:orange;"></table>
```

And the associated plugin initializations:
```js
/*global $ */
$(function () {
    var initFileUpload = function (suffix) {
        $('#file_upload').fileUploadUI({
            namespace: 'file_upload_' + suffix,
            fileInputFilter: '#file_' + suffix,
            dropZone: $('#drop_zone_' + suffix),
            uploadTable: $('#files_' + suffix),
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
    };
    initFileUpload(1);
    initFileUpload(2);
    initFileUpload(3);
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
```
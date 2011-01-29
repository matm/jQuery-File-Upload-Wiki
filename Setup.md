# How to setup the plugin on your website

1. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) the plugin archive, extract it and upload the contents (without the "example" directory) to your server. 
2. Add the following two lines to the head of your page (adjust the path to the *jquery.fileupload-ui.css* file):
```html
<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.6/themes/base/jquery-ui.css" id="theme">
<link rel="stylesheet" href="../jquery.fileupload-ui.css">
```
3. Add the following form to the body of your page (replace *upload.php* with the path to your upload handler):
```html
<form class="upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file" multiple>
    <button>Upload</button>
    <div>Upload files</div>
</form> 
```
4. Add the following two lines to the body of your page, where you want the upload/download tables to appear:
```html
<table class="upload_files"></table>
<table class="download_files"></table>
```
5. Add the following lines to the bottom of your page, before the closing body tag (adjust the paths to the *jquery.fileupload.js* and *jquery.fileupload-ui.js* files):
```html
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.6/jquery-ui.min.js"></script>
<script src="../jquery.fileupload.js"></script>
<script src="../jquery.fileupload-ui.js"></script>
<script>
$(function () {
    $('.upload').fileUploadUI({
        uploadTable: $('.upload_files'),
        downloadTable: $('.download_files'),
        buildUploadRow: function (files, index) {
            var file = files[index];
            return $(
                '<tr>' +
                '<td>' + file.name + '<\/td>' +
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
            return $(
                '<tr><td>' + file.name + '<\/td><\/tr>'
            );
        }
    });
});
</script> 
```
6. Implement your server-side file upload handler to store the uploaded files and return a JSON response with the file information, e.g.:
```js
{"name":"picture.jpg","type":"image/jpeg","size":"123456789"}
```
**Note:** The file upload plugin makes use of iframes for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) uploads.  
They will only register a load event if the [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of the response is set to *text/plain* or *text/html*, **not** if it is set to *application/json*.
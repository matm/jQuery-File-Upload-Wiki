# Setup instructions v4

## Simple version using the provided example for websites running PHP

The provided example works out of the box and only needs one step for you to add it to your PHP based website:

1. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) the plugin archive, extract it and upload the extracted folder (you may rename it) to your server.

Visit the uploaded folder's "example" directory - you should see a file upload interface similar to the demo, allowing you to upload files to your website.

If uploading files doesn't work, make sure that the *files* and *thumbnails* directories permissions allow your server write access.

**Note:**  
The provided PHP upload handler allows anyone to upload files. The uploaded files are also accessible for anyone to download.  
The easiest way to add some kind of authentication system is to protect the example directory by adding [.htaccess based password protection](http://httpd.apache.org/docs/2.2/howto/auth.html#gettingitworking).

## Simple version using the provided example for websites without PHP

1. Implement a file upload handler on your platform (Ruby, Python, Java, etc.) that handles normal form based file uploads and upload it to your server. See also the Server-side specific tutorials on the [Documentation Homepage](https://github.com/blueimp/jQuery-File-Upload/wiki).
2. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) and extract the plugin archive.
3. Edit *example/index.html* and adjust the *action* attribute of the HTML form element to the URL of your custom file upload handler. Adjust the file input *name* attribute if your upload handler requires another parameter name for the file uploads.
4. Upload the jQuery-File-Upload folder to your website.
5. Extend your custom server-side upload handler to return a [JSON](http://en.wikipedia.org/wiki/JSON) response akin to the following output:
```js
{"name":"picture.jpg","size":902604,"url":"\/jQuery-File-Upload\/example\/files\/picture.jpg","thumbnail":"\/jQuery-File-Upload\/example\/thumbnails\/picture.jpg"}
```
Adjust it to respond with a JSON array if your upload component handles multiple files.

Visit the uploaded folder's "example" directory - you should see a file upload interface similar to the demo, allowing you to upload files to your website.

**Note:**  
The file upload plugin makes use of iframes for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) uploads.  
Iframe based uploads require a [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of *text/plain* or *text/html* for the JSON response - they will offer a download dialog if the iframe response is set to *application/json*.

## Advanced version without using the provided example code

1. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) the plugin archive, extract it and upload the contents without the "example" directory to your server.
2. Add the following two lines to the head of your page (adjust the path to the *jquery.fileupload-ui.css* file):
```html
<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.12/themes/base/jquery-ui.css" id="theme">
<link rel="stylesheet" href="css/jquery.fileupload-ui.css">
```
3. Add the following form to the body of your page (replace *upload.php* with the path to your upload handler):
```html
<form id="file_upload" action="upload.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="file[]" multiple>
    <button>Upload</button>
    <div class="file_upload_label">Upload files</div>
</form>
```
4. Add the following line to the body of your page, where you want the upload/download table to appear:
```html
<table id="files"></table>
```
5. Add the following lines to the bottom of your page, before the closing body tag (adjust the paths to the *jquery.fileupload.js* and *jquery.fileupload-ui.js* files):
```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.5.2/jquery.min.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.12/jquery-ui.min.js"></script>
<script src="js/jquery.fileupload.js"></script>
<script src="js/jquery.fileupload-ui.js"></script>
<script src="js/application.js"></script>
```
6. Create the file application.js with the following code - feel free to adjust the content returned by *buildUploadRow* and *buildDownloadRow*, as long as one *tr* or *tbody* element is returned:
```js
$(function () {
    $('#file_upload').fileUploadUI({
        uploadTable: $('#files'),
        buildUploadRow: function (files, index, handler) {
            return $('<tr><td>' + files[index].name + '<\/td>' +
                    '<td class="file_upload_progress"><div><\/div><\/td>' +
                    '<td class="file_upload_cancel">' +
                    '<button class="ui-state-default ui-corner-all" title="Cancel">' +
                    '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                    '<\/button><\/td><\/tr>');
        },
        buildDownloadRow: function (file, handler) {
            return $('<tr><td>' + file.name + '<\/td><\/tr>');
        }
    });
});
```
7. Implement your server-side file upload handler to store the uploaded files and return a JSON response with the file information, e.g.:
```js
{"name":"picture.jpg","type":"image/jpeg","size":"123456789"}
```

## How to use only the Basic plugin (minimal setup guide)

The plugin [download package](https://github.com/blueimp/jQuery-File-Upload/archives/master) comes with a complete user interface based on [Bootstrap](http://twitter.github.com/bootstrap/) and an example PHP file upload handler that is easy to [[Setup]].

There is also a version available for [[jQuery UI]].

However, if you want to build your own user interface, it is possible to use only the [basic plugin](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload.js) version and a minimal setup.  
The following is an alternative to [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/index.html) with only the minimal requirements and a simple *done* callback handler (see [[API]] and [[Options]] on how to use the different options and callbacks):

```html
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8">
<title>jQuery File Upload Example</title>
</head>
<body>
<input id="fileupload" type="file" name="files[]" multiple>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
<script src="js/vendor/jquery.ui.widget.js"></script>
<script src="js/jquery.iframe-transport.js"></script>
<script src="js/jquery.fileupload.js"></script>
<script>
$(function () {
    $('#fileupload').fileupload({
        dataType: 'json',
        url: 'server/php/',
        done: function (e, data) {
            $.each(data.result, function (index, file) {
                $('<p/>').text(file.name).appendTo(document.body);
            });
        }
    });
});
</script>
</body> 
</html>
```

## How to use image resizing functionality with the basic plugin

Include the following additional scripts (include jquery.fileupload-ip.js after jquery.fileupload.js):

```html
<script src="http://blueimp.github.com/JavaScript-Load-Image/load-image.min.js"></script>
<script src="http://blueimp.github.com/JavaScript-Canvas-to-Blob/canvas-to-blob.min.js"></script>
<script src="js/jquery.fileupload-ip.js"></script>
```

If you don't override the **add** callback option, you'll have image resizing functionality automatically. Else, you can make use of the **resize** API like this:

```js
$('#fileupload').fileupload({
    /* ... */
    add: function (e, data) {
        $(this).fileupload('resize', data).done(function () {
            data.submit();
        });
    }
});
```

Please have a look at the [[API]] and [[Options]] documentations.

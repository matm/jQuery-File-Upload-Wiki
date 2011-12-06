The plugin [download package](https://github.com/blueimp/jQuery-File-Upload/archives/master) comes with a complete user interface based on jQuery UI and an example PHP file upload handler that is easy to [[Setup]].

However, if you want to build your own user interface, it is possible to use only the [basic plugin](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js) version and a minimal setup.  
The following is an alternative to *example/index.html* with only the minimal requirements and a simple *done* callback handler (see [[API]] and [[Options]] on how to use the different options and callbacks):

```html
<!DOCTYPE HTML>
<html>
<head>
<meta charset="utf-8">
<title>jQuery File Upload Example</title>
</head>
<body>
<input id="fileupload" type="file" name="files[]" multiple>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.4/jquery.min.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.16/jquery-ui.min.js"></script>
<script src="../jquery.iframe-transport.js"></script>
<script src="../jquery.fileupload.js"></script>
<script>
$(function () {
    $('#fileupload').fileupload({
        dataType: 'json',
        url: 'php/index.php',
        done: function (e, data) {
            $.each(data.result, function (index, file) {
                $('<p/>').text(file.name).appendTo('body');
            });
        }
    });
});
</script>
</body> 
</html>
```

Note that instead of including the complete jQuery UI library, only the jQuery UI Widget component is required and can be downloaded using [jQuery UI's custom download builder](http://jqueryui.com/download).
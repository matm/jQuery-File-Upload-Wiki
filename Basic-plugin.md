## How to use only the Basic plugin (minimal setup guide)

The plugin [download package](https://github.com/blueimp/jQuery-File-Upload/archives/master) comes with a complete user interface based on [Bootstrap](http://twitter.github.com/bootstrap/). There is also a version available for [[jQuery UI]].

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
<input id="fileupload" type="file" name="files[]" data-url="server/php/" multiple>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
<script src="js/vendor/jquery.ui.widget.js"></script>
<script src="js/jquery.iframe-transport.js"></script>
<script src="js/jquery.fileupload.js"></script>
<script>
$(function () {
    $('#fileupload').fileupload({
        dataType: 'json',
        done: function (e, data) {
            $.each(data.result.files, function (index, file) {
                $('<p/>').text(file.name).appendTo(document.body);
            });
        }
    });
});
</script>
</body> 
</html>
```

### Response and data type
The example above sets the *dataType* option to **json**, expecting the server to return a JSON response for each uploaded file. However it's also possible to handle HTML content types as response or any other dataType that you can handle in your *done* handler.

## How to display upload progress with the basic plugin
The fileupload plugin triggers progress events for both individual uploads (**progress**) and all running uploads combined (**progressall**). Event handlers can be set via the event binding mechanism or as widget options (see [[API]] and [[Options]] documentation):

```js
$('#fileupload').fileupload({
    /* ... */
    progressall: function (e, data) {
        var progress = parseInt(data.loaded / data.total * 100, 10);
        $('#progress .bar').css(
            'width',
            progress + '%'
        );
    }
});
```

The previous code assumes a progress node with an inner element that displays the progress status via its width percentage:

```html
<div id="progress">
    <div class="bar" style="width: 0%;"></div>
</div>
```

The inner element should have a different background color than the container node, set via CSS and needs a height applied:

```css
.bar {
    height: 18px;
    background: green;
}
```

## How to tie a file to an element node during the life cycle of an upload
Often, you will display a file to upload in an element node. This can be done in the **add** callback.  
To be able to refer to the same element node in other callbacks related to the upload, you can make use of the **context** option (which is actually an option of [jquery.ajax](http://api.jquery.com/jQuery.ajax/)):

```js
$(function () {
    $('#fileupload').fileupload({
        dataType: 'json',
        add: function (e, data) {
            data.context = $('<p/>').text('Uploading...').appendTo(document.body);
            data.submit();
        },
        done: function (e, data) {
            data.context.text('Upload finished.');
        }
    });
});
```

## How to start uploads with a button click

Based on the previous example, it's possible to start uploads on the click of a button instead of automatically:

```js
$(function () {
    $('#fileupload').fileupload({
        dataType: 'json',
        add: function (e, data) {
            data.context = $('<button/>').text('Upload')
                .appendTo(document.body)
                .click(function () {
                    data.context = $('<p/>').text('Uploading...').replaceAll($(this));
                    data.submit();
                });
        },
        done: function (e, data) {
            data.context.text('Upload finished.');
        }
    });
});
```

## How to use image resizing functionality with the basic plugin
Include the following additional scripts (include jquery.fileupload-fp.js after jquery.fileupload.js):

```html
<script src="http://blueimp.github.com/JavaScript-Load-Image/load-image.min.js"></script>
<script src="http://blueimp.github.com/JavaScript-Canvas-to-Blob/canvas-to-blob.min.js"></script>
<script src="js/jquery.fileupload-fp.js"></script>
```

If you don't override the **add** callback option, you'll have image resizing functionality automatically. Else, you can make use of the **process** API like this:

```js
$('#fileupload').fileupload({
    /* ... */
    add: function (e, data) {
        $(this).fileupload('process', data).done(function () {
            data.submit();
        });
    }
});
```

Please have a look at the [[API]] and [[Options]] documentations.
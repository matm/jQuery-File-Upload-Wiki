First, [download](http://requirejs.org/docs/download.html) and add require.js to the [js](https://github.com/blueimp/jQuery-File-Upload/tree/master/js) directory.

Next, edit [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/index.html) and replace all the script sources starting with jQuery:

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js"></script>
...
<script src="js/main.js"></script>
```

Replace them with the following single script source:

```html
<script data-main="js/main" src="js/require.js"></script>
```

Next, edit [js/main.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/main.js) and replace its content with the following:

```js
require(
    [
        'http://ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js',
        'https://blueimp.github.io/JavaScript-Templates/js/tmpl.min.js',
        'https://blueimp.github.io/JavaScript-Load-Image/js/load-image.all.min.js',
        'https://blueimp.github.io/JavaScript-Canvas-to-Blob/js/canvas-to-blob.min.js',
        'js/jquery.iframe-transport',
        'js/jquery.fileupload-ui'
    ],
    function() {
        $('#fileupload').fileupload({
            url: 'server/php/'
        });
    }
);
```
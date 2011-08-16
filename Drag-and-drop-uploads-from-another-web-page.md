The following code snippet allows to upload images by drag&drop from another webpage, though this is currently only supported on Firefox:

```js
$('#fileupload').fileupload().bind('drop', function (e) {
    var url = $(e.originalEvent.dataTransfer.getData('text/html')).filter('img').attr('src');
    if (url) {
        $.getImageData({
            url: url,
            success: function (img) {
                var canvas = document.createElement('canvas'),
                    file;
                canvas.getContext('2d').drawImage(img, 0, 0);
                if ($.type(canvas.mozGetAsFile) === 'function') {
                    file = canvas.mozGetAsFile('file.png');
                }
                if (file) {
                    $('#fileupload').fileupload('add', {files: [file]});
                }
            }
        });
    }
});
```

Due to the [Same Origin policy](http://en.wikipedia.org/wiki/Same_origin_policy), which also applies to [the canvas element](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html#security-with-canvas-elements), it is not possible to load an image directly from another domain.  
Therefore one of the requirements for the code snippet above is a server-side proxy to retrieve the image data: The [$.getImageData](http://www.maxnov.com/getimagedata/) library has to be included along with the jQuery File Upload libraries.

When the [BlobBuilder interface](http://dev.w3.org/2009/dap/file-system/file-writer.html#the-blobbuilder-interface) becomes available, this might work for for Google Chrome and other browsers as well, see [Convert Data URI to File then append to FormData](http://stackoverflow.com/questions/4998908/convert-data-uri-to-file-then-append-to-formdata).
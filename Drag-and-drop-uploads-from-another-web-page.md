The following code snippet allows to upload images by drag&drop from another webpage:

```html
<script src="https://raw.github.com/betamax/getImageData/master/jquery.getimagedata.min.js"></script>
<script>
$(document).bind('drop', function (e) {
    var url = $(e.originalEvent.dataTransfer.getData('text/html')).filter('img').attr('src');
    if (url) {
        $.getImageData({
            url: url,
            success: function (img) {
                var canvas = document.createElement('canvas');
                if (canvas.getContext) {
                    canvas.getContext('2d').drawImage(img, 0, 0);
                    canvasToBlob(canvas, function (blob) {
                        $('#fileupload').fileupload('add', {files: [blob]});
                    });
                }
            }
        });
    }
});
</script>
```

Due to the [Same Origin policy](http://en.wikipedia.org/wiki/Same_origin_policy), which also applies to [the canvas element](http://www.whatwg.org/specs/web-apps/current-work/multipage/the-canvas-element.html#security-with-canvas-elements), it is not possible to load an image directly from another domain.  
Therefore one of the requirements for the code snippet above is a server-side proxy to retrieve the image data: The [$.getImageData](http://www.maxnov.com/getimagedata/) library has to be included along with the jQuery File Upload libraries.
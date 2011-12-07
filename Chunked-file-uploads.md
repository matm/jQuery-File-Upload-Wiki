Chunked file uploads are only supported by browsers with support for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads and the [Blob API](https://developer.mozilla.org/en/DOM/Blob), which includes Google Chrome and Mozilla Firefox 4+. 

## Client-side setup
To upload large files in smaller chunks, set the *maxChunkSize* option (see [[Options]]) to a preferred maximum chunk size in Bytes:

```js
$('#fileupload').fileupload({
    maxChunkSize: 10000000 // 10 MB
});
```

For chunked uploads to work in Mozilla Firefox 4-6 (XHR upload capable Firefox versions prior to Firefox 7), the *multipart* option also has to be set to *false* - see the [[Options]] documentation on *maxChunkSize* for an explanation.

## Server-side setup
The [example PHP upload handler](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/upload.php) supports chunked uploads out of the box.
You only need to set the *discard_aborted_uploads* option to *false*:

```php
<?php
// ...
'discard_aborted_uploads' => false
// ...
?>
```

To still be able to discard partial uploads if the user aborts the upload process, a DELETE request for the uploaded file has to be issued after every cancel action, e.g. with the following code:

```js
$('#fileupload').bind('fileuploadfail', function (e, data) {
    if (data.errorThrown === 'abort') {
        $.ajax({
            url: 'php/index.php?' + $.param({
                file: data.files[0].name
            }),
            type: 'DELETE'
        });
    }
});
```

To support chunked uploads, the upload handler compares the given file name and file size (transmitted via *X-File-Name* and *X-File-Size* headers) with already uploaded files to determine if the current blob has to be appended to an existing file.  
If your setup requires other information than the file name for chunked uploads (e.g. a file ID), this information could be provided via session parameters.

## How do chunked uploads work?
If *maxChunkSize* is set to an integer value greater than 0, the File Upload plugin splits up files with a file size bigger than *maxChunkSize* into multiple [blobs](https://developer.mozilla.org/en/DOM/Blob) and submits each of these blobs to the upload url in sequential order.

Chunked uploads (as well as all non-multipart uploads) set the following custom headers:

```js
{
    'X-File-Name': file.name,
    'X-File-Type': file.type,
    'X-File-Size': file.size
}
```

These headers transmit the original attributes of the chunked file and allow the server-side application to combine the uploaded blobs into one file.

## Callbacks
Chunked file uploads trigger the same callbacks as normal file uploads, e.g. the *done* callback (see [[API]]) will only be triggered after the last blob has been successfully uploaded.
However, callbacks set as part of the [$.ajax](http://api.jquery.com/jQuery.ajax/) [[Options]] (e.g. *success* or *complete*) will be called for each AJAX request.

## Cross-site chunked uploads
By default, browsers don't allow custom headers on cross-site file uploads, if they are not explicitly defined as allowed with the following server-side headers:

```
Access-Control-Allow-Headers X-File-Name,X-File-Type,X-File-Size
```

## Resuming file uploads
Using the *uploadedBytes* option (see [[Options]]), it is possible to resume aborted uploads:

```js
$('#fileupload').fileupload({
    maxChunkSize: 10000000, // 10 MB
    add: function (e, data) {
        var that = this;
        $.getJSON('php/index.php', {file: data.files[0].name}, function (file) {
            data.uploadedBytes = file && file.size;
            $.blueimpUI.fileupload.prototype
                .options.add.call(that, e, data);
        });
    }
});
```

The above code overrides the add callback and sends a JSON request with the current file name to the server. If a file with the given name exists, the server responds with the file information including the file size, which is set as *uploadedBytes* option.  
If *uploadedBytes* is set, the plugin only uploads the remaining parts of the file as blob upload.

### Automatic resume

The following code snippet implements an automatic resume functionality, based on the previous code:

```js
$('#fileupload').fileupload({
    /* ... settings as above plus the following ... */
    maxRetries: 100,
    retryTimeout: 500,
    fail: function (e, data) {
        var fu = $(this).data('fileupload'),
            retries = data.context.data('retries') || 0,
            retry = function () {
                $.getJSON('php/index.php', {file: data.files[0].name})
                    .done(function (file) {
                        data.uploadedBytes = file && file.size;
                        data.submit();
                    })
                    .fail(function () {
                        fu._trigger('fail', e, data);
                    });
            };
        if (data.errorThrown !== 'abort' &&
                data.errorThrown !== 'uploadedBytes' &&
                retries < fu.options.maxRetries) {
            retries += 1;
            data.context.data('retries', retries);
            window.setTimeout(retry, retries * fu.options.retryTimeout);
            return;
        }
        data.context.removeData('retries');
        $.blueimpUI.fileupload.prototype
            .options.fail.call(this, e, data);
    }
});
```

If the upload fails, the code above will automatically resume the file upload after retrieving the uploaded bytes.  
To prevent endless loops, the number of retries can be limited with the *maxRetries* setting.  
The *retryTimeout* setting defines a timeout in milliseconds, before the file upload is resumed. It is increased for every subsequent retry to extend the waiting time.
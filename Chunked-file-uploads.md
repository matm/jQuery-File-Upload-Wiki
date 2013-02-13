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
The [example PHP upload handler](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/UploadHandler.php) supports chunked uploads out of the box.

To support chunked uploads, the upload handler makes use of the [Content-Range header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.16), which is transmitted by the plugin for each chunk.

## How do chunked uploads work?
If *maxChunkSize* is set to an integer value greater than 0, the File Upload plugin splits up files with a file size bigger than *maxChunkSize* into multiple [blobs](https://developer.mozilla.org/en/DOM/Blob) and submits each of these blobs to the upload url in sequential order.

The byte range of the blob is transmitted via the [Content-Range header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.16).  
The file name of the blob is transmitted via the **Content-Disposition** header.

## Callbacks
Chunked file uploads trigger the same callbacks as normal file uploads, e.g. the *done* callback (see [[API]]) will only be triggered after the last blob has been successfully uploaded.

Chunked uploads trigger additional callbacks, that can be used to track the events of individual chunked uploads:

```js
$('#fileupload').fileupload({maxChunkSize: 100000})
    .on('fileuploadchunksend', function (e, data) {})
    .on('fileuploadchunkdone', function (e, data) {})
    .on('fileuploadchunkfail', function (e, data) {})
    .on('fileuploadchunkalways', function (e, data) {});
```

**Note:**
Callbacks set as part of the [$.ajax](http://api.jquery.com/jQuery.ajax/) [[Options]] (e.g. *success*, *error* or *complete*) will be called for each AJAX request, including uploads of individual chunks.

## Cross-site chunked uploads
By default, browsers don't allow all headers used for cross-site file uploads, if they are not explicitly defined as allowed with the following server-side headers:

```
Access-Control-Allow-Headers Content-Type, Content-Range, Content-Disposition
```

## Resuming file uploads
Using the *uploadedBytes* option (see [[Options]]), it is possible to resume aborted uploads:

```js
$('#fileupload').fileupload({
    maxChunkSize: 10000000, // 10 MB
    add: function (e, data) {
        var that = this;
        $.getJSON('server/php/', {file: data.files[0].name}, function (result) {
            var file = result.file;
            data.uploadedBytes = file && file.size;
            $.blueimp.fileupload.prototype
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
        // jQuery Widget Factory uses "namespace-widgetname" since version 1.10.0:
        var fu = $(this).data('blueimp-fileupload') || $(this).data('fileupload'),
            retries = data.context.data('retries') || 0,
            retry = function () {
                $.getJSON('server/php/', {file: data.files[0].name})
                    .done(function (result) {
                        var file = result.file;
                        data.uploadedBytes = file && file.size;
                        // clear the previous data:
                        data.data = null;
                        data.submit();
                    })
                    .fail(function () {
                        fu._trigger('fail', e, data);
                    });
            };
        if (data.errorThrown !== 'abort' &&
                data.uploadedBytes < data.files[0].size &&
                retries < fu.options.maxRetries) {
            retries += 1;
            data.context.data('retries', retries);
            window.setTimeout(retry, retries * fu.options.retryTimeout);
            return;
        }
        data.context.removeData('retries');
        $.blueimp.fileupload.prototype
            .options.fail.call(this, e, data);
    }
});
```

If the upload fails, the code above will automatically resume the file upload after retrieving the uploaded bytes.  
To prevent endless loops, the number of retries can be limited with the *maxRetries* setting.  
The *retryTimeout* setting defines a timeout in milliseconds, before the file upload is resumed. It is increased for every subsequent retry to extend the waiting time.

## Force re-upload of last chunk
In case you would like to enforce the chunked upload to re-upload the last chunk, you could achieve this by applying the following logic.

You should replace your calls to "new UploadHandler(...)" with "new ChunkedUploadHelper(...)", which will trim the file if it exists. The rest of the code and functionality is untouched. _**Note: this code is based and tested upon jQuery File Upload Plugin 5.21**_

```php
/**
 * Helper method for trimming chunked uploads.
 */
class ChunkedUploadHelper extends UploadHandler {
    /**
     * Custom "get" method to call our custom function.
     */
    public function get($print_response = true) {
        if ($print_response && isset($_GET['download'])) {
            return $this->download();
        }
        $file_name = $this->get_file_name_param();
        if ($file_name) {
            $response = array(
                substr($this->options['param_name'], 0, -1) => $this->get_file_object_trimmed($file_name)
            );
        } else {
            $response = array(
                $this->options['param_name'] => $this->get_file_objects()
            );
        }
        return $this->generate_response($response, $print_response);
    }

    /**
     * Gets a trimmed file object if the maxChunkSize has been given.
     */
    protected function get_file_object_trimmed($file_name) {
        $file_object = $this->get_file_object($file_name);
        if ($file_object != null) {
            // In case an upload was started and we have a max chunk size, trim it to the last full size.
            $max_chunk_size = $_GET['maxChunkSize'] || 0;
            if ($file_object->size > 0) && ($max_chunk_size > 0) {
                $truncated_bytes = floor($file_object->size / $max_chunk_size) * $max_chunk_size;
                if ($truncated_bytes < $file_object->size) {
                    $file_path = $this->get_upload_path($file_name, $this->get_version_param());
                    if (ftruncate(fopen($file_path, 'r+'), $truncated_bytes)) {
                        // Update file size to reflect truncated state
                        $file_object->size = filesize($file_path);
                    } else {
                        // Something went wrong while truncating, redo entire file
                        @unlink($file_path);
                        $file_object = null;
                    }
                }
            }
        }
        return $file_object;
    }
}
```

Then all you need to do is to pass the maxChunkSize value to the backend script, and the UploadHandler will take care of the rest.
```js
$('#fileupload').fileupload({
    /* ... settings as above with add function replaced with ... */
    add: function (e, data) {
        // jQuery Widget Factory uses "namespace-widgetname" since version 1.10.0:
        var fu = $(this).data('blueimp-fileupload') || $(this).data('fileupload');
        var that = this;
        $.getJSON('server/php/', {file: data.files[0].name, maxChunkSize: fu.options.maxChunkSize}, function (result) {
            var file = result.file;
            data.uploadedBytes = file && file.size;
            $.blueimp.fileupload.prototype
                .options.add.call(that, e, data);
        });
    }
});
```
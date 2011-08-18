Chunked Uploads are supported "out-of-the-box" by all browsers supporting [XMLHttpRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads and the [Blob API](https://developer.mozilla.org/en/DOM/Blob).  
Chunked Uploads are enabled if the *maxChunkSize* option (see [[Options]]) is set to a value greater than 0, e.g. 10000000 bytes (10 MB):

```js
$('#fileupload').fileupload({
    multipart: false,
    maxChunkSize: 10000000
});
```

In the example above, the *multipart* option is also set to *false*. This is due to Gecko 2.0 (Firefox 4 etc.) adding blobs with an empty filename when building a multipart upload request using the FormData interface - see [Bugzilla entry #649150](https://bugzilla.mozilla.org/show_bug.cgi?id=649150).  
Several server-side frameworks (including PHP and Django) cannot handle multipart file uploads with empty filenames.

**Note**:
The example PHP upload handler discards partial uploads automatically.  
For chunked uploads to work, the PHP upload handler option *discard_aborted_uploads* has to be set to *false*.

## How to resume aborted file uploads
File uploads can be resumed by overriding the *add* callback and setting the *uploadedBytes* option.  
In the following example, the uploaded filesize for a given file is retrieved with an AJAX call and then used to set the *uploadedBytes* option inside of the *add* callback:

```js
$('#fileupload').fileupload({
    multipart: false,
    maxChunkSize: 10000000,
    add: function (e, data) {
        var that = this;
        $.getJSON(
            'upload.php',
            {file: data.files[0].name},
            function (file) {
                data.uploadedBytes = file && file.size;
                $.blueimpUI.fileupload.prototype
                    .options.add.call(that, e, data);
            }
        );
    }
});
```

In the above example, the *upload.php* script is supposed to return a JSON object holding file information when called via a GET request with a given file parameter with the name of the previously aborted file upload.
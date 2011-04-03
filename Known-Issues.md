# Known Issues

This page lists known issues that cannot be fixed by the plugin code but rather depend on browser or operating system specific issues or the design of HTTP/HTML/JavaScript/CSS.

## Content-type of the JSON response for iframe uploads
The file upload plugin makes use of iframes for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) uploads.  
They will only register a load event if the [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of the response is set to *text/plain* or *text/html*, **not** if it is set to *application/json*.  
If browsers like IE offer to download files as result of the response, try setting the content-type of your JSON response to `text/plain`.

## Opera file input with multiple option
The plugin submits multiple files if more than one has been selected in Opera, using the iframe method (which is basically the same as a non-JavaScript form submit).
Due to the Opera not supporting the File API, only the filename of one of the files is accessible via JavaScript (via the file input value), not the complete file list and it's only possible to submit all the selected files with one request.

There also seems to be a bug in PHP related to the way the multiple file upload is implemented in Opera: [PHP Bug 47789](http://bugs.php.net/bug.php?id=47789).

Since it's still possible to select and submit multiple files with Opera I'm reluctant to disable it in the plugin code itself, although the support is kind of limited and requires additional work on server-side to accept multi-file requests.

However, you can remove (or add) the *multiple* option via JavaScript depending on the implementation of the File API without having to adjust the plugin code itself:

```js
$('#file_upload').each(function () {
    // Fix for browsers which support multiple file selection but not the File API:
    // https://github.com/blueimp/jQuery-File-Upload/issues#issue/36
    if (typeof File === 'undefined') {
        $(this).find('input:file').each(function () {
            $(this).removeAttr('multiple')
                // Fix for Opera, which ignores just removing the multiple attribute:
                .replaceWith($(this).clone(true));
        });
    }
}).fileUploadUI(fileUploadOptions);
```

See also [issue #36](https://github.com/blueimp/jQuery-File-Upload/issues/36).

## Bad performance with Firefox 3.6 and very large (e.g. 2GB) files
On Firefox 3.6 File Uploads are handled differently than on FF 4 and Chrome.  
On FF 3.6 the files have to be loaded in memory via readAsBinaryString and concatenated into a multipart message string for the upload, which is the reason for the bad performance and may lead to a slowdown or even a crash of Firefox 3.6.  
See also [issue #79](https://github.com/blueimp/jQuery-File-Upload/issues/79).

## Mozilla Firefox + GNU/Linux file managers
**Reported by [jni-](https://github.com/jni-):**  
If you are using linux and firefox, file drag&drop upload won't work with some file managers.  
Tested with thunar, pcmanfm and ROX. Nautilus seems to work.  
If you want all the details, [[have a look here|https://bugzilla.mozilla.org/show_bug.cgi?id=609284]]  
Chrome works fine with these file managers.
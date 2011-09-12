The plugin supports two methods of doing cross-site (cross-domain) file uploads:

* Cross-site XMLHttpRequest file uploads
* Cross-site iframe transport uploads

## Cross-site XMLHttpRequest file uploads
Cross-site XHR file uploads don't require any work on client side, but are only supported by browsers supporting XHR File Uploads - see [[Browser support]].

To allow cross-site XHR file uploads, the receiving server must set the appropriate [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_access_control#Access-Control-Allow-Origin) headers, e.g.:

```
Access-Control-Allow-Origin http://example.org
```

**Note:**  
For cross-browser compatibility, the header must be set as response to both the file upload (POST) request as well as response to OPTIONS requests. See [Preflighted requests](https://developer.mozilla.org/En/HTTP_access_control#Preflighted_requests) for more information.

If the *multipart* option is set to *false* or [[Chunked file uploads]] are enabled, you also need to set [Access-Control-Allow-Headers](https://developer.mozilla.org/En/HTTP_access_control#Access-Control-Allow-Headers) to allow custom headers used by the plugin to transmit file meta information:

```
Access-Control-Allow-Headers X-File-Name,X-File-Type,X-File-Size
```

With the appropriate headers set on server-side, cross-domain XHR file uploads work just like file uploads to the same domain.

## Cross-site iframe transport uploads

To force all browser to make use of the iframe transport module for file uploads, the *forceIframeTransport* option can be set to *true*:

```js
$('#fileupload').fileupload({
    forceIframeTransport: true
});
```

Cross-site iframe transport uploads don't require any additional server response headers.  
Unfortunately, it is not possible to access the response body of iframes on a different domain.

However if both servers - the server hosting the upload form and the target server for the file uploads - are just on different subdomains (e.g. source.example.org and target.example.org), it is possible to access the iframe content by adding the following line of Javascript to both webpages (the upload form page and the upload server response page):

```
document.domain = 'example.com';
```

Note that this requires the server response to be a HTML document (and not JSON as is the default for the UI version of the plugin).
The plugin supports two methods of doing cross-site (cross-domain) file uploads:

* Cross-site XMLHttpRequest file uploads
* Cross-site iframe transport uploads

**Node:**
The PHP and Google App Engine implementations come with full cross-domain support out of the box.

## Cross-site XMLHttpRequest file uploads
Cross-site XHR file uploads don't require any work on client side, but are only supported by browsers supporting XHR File Uploads - see [[Browser support]].

To allow cross-site XHR file uploads, the receiving server must set the appropriate [Access-Control-Allow-Origin](https://developer.mozilla.org/En/HTTP_access_control#Access-Control-Allow-Origin) headers, e.g.:

```
Access-Control-Allow-Origin: http://example.org
```

**Note:**  
For cross-browser compatibility, the header must be set as response to both the file upload (POST) request as well as response to OPTIONS requests. See [Preflighted requests](https://developer.mozilla.org/En/HTTP_access_control#Preflighted_requests) for more information.

If the *multipart* option is set to *false* or [[Chunked file uploads]] are enabled, you also need to set [Access-Control-Allow-Headers](https://developer.mozilla.org/En/HTTP_access_control#Access-Control-Allow-Headers) to allow custom headers used by the plugin to transmit file meta information:

```
Access-Control-Allow-Headers: X-File-Name,X-File-Type,X-File-Size
```

If you need to send along cookies (e.g. for authentication), set the *withCredentials* [$.ajax()](http://api.jquery.com/jQuery.ajax/) setting as fileupload widget option:

```js
$('#fileupload').fileupload('option', {
    xhrFields: {
        withCredentials: true
    }
});
```

On server-side, you need to set the header [Access-Control-Allow-Credentials](https://developer.mozilla.org/en/http_access_control#Requests_with_credentials) to *true*:

```
Access-Control-Allow-Origin: http://example.org
Access-Control-Allow-Credentials: true
```

**Note:** when responding to a credentialed request, the server must specify a domain, and cannot use wild carding (*).

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

Therefore Cross-site iframe transport uploads require a redirect back to the origin server to retrieve the upload results. Set the **redirect** option to the absolute url of your [result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/result.html) file, which must reside on the origin server:

```js
$('#fileupload').fileupload(
    'option',
    'redirect',
    'http://example.org/result.html?%s'
);
```

The plugin will transmit the absolute URL set as **redirect** option as part of the formData (with the parameter name **redirect** if the option **redirectParamName** is not set as well) if the file upload is a cross-domain iframe  transport upload. Else (for XHR file uploads or same-domain iframe uploads), the option is ignored.  

On server-side, you need to check if a request parameter **redirect** has been transmitted with the file upload. In this case, the server response to the upload has to be a redirect to this parameter, with the urlencoded result contents appended to the redirect URL.
Note that the redirect URL is supposed to have a placeholder (e.g. **%s**) as part of the URL, where the upload server will append the urlencoded upload results.

### Cross-site uploads to different subdomains

If both servers - the server hosting the upload form and the target server for the file uploads - are just on different subdomains (e.g. source.example.org and target.example.org), it is possible to access the iframe content on the subdomain by adding the following line of Javascript to both webpages (the upload form page and the upload server response page):

```
document.domain = 'example.com';
```

Note that this requires the server response to be a HTML document (and not JSON as is the default for the UI version of the plugin).
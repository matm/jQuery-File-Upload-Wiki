The plugin supports two methods of doing cross-site (cross-domain) file uploads:

* Cross-site XMLHttpRequest file uploads
* Cross-site iframe transport uploads

**Note:**
All provided server-side implementations come with full cross-domain support out of the box.

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
Access-Control-Allow-Headers: Content-Type, Content-Range, Content-Disposition, Content-Description
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

The [result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/result.html) page adds the decoded result content as body content. This allows the plugin to access the response without cross-domain access issues.

Example redirect URL sent back to the client:

```
http://example.org/result.html?%7B%22files%22%3A%5B%7B%22name%22%3A%22cat.jpg%22%2C%22size%22%3A464885%2C%22type%22%3A%22image%2Fjpeg%22%2C%22url%22%3A%22http%3A%2F%2Fexample.org%2Ffiles%2Fcat.jpg%22%2C%22thumbnail_url%22%3A%22http%3A%2F%2Fexample.org%2Ffiles%2Fthumbnail%2Fcat.jpg%22%2C%22delete_url%22%3A%22http%3A%2F%2Fexample.org%2F%3Ffile%3Dcat.jpg%22%7D%5D%7D
```

Example body content decoded by the [result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/result.html) page:

```json
{"files":[{"name":"cat.jpg","size":464885,"type":"image/jpeg","url":"http://example.org/files/cat.jpg","thumbnail_url":"http://example.org/files/thumbnail/cat.jpg","delete_url":"http://example.org/?file=cat.jpg"}]}
```

**Note:**
The response should not exceed a certain length, as the redirect URL is limited by the [maximum URL length](What is the maximum length of a URL?) that browsers will process.

### Cross-site iframe transport uploads with HTML responses on different subdomains

If both servers - the server hosting the upload form and the target server for the file uploads - are just on different subdomains (e.g. source.example.org and target.example.org), it is possible to access the iframe content on the subdomain by adding the following line of Javascript to both webpages (the upload form page and the upload server response page):

```
document.domain = 'example.com';
```

Note that this requires the server response to be a HTML document and not JSON as is the default for the UI version of the plugin.

## Additional cross-domain resources

The jQuery File Upload repository also provides two additional scripts for Cross-Origin-Resource-Sharing.

### XDomainRequest Transport
The [XDomainRequest Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.xdr-transport.js) is included for cross-domain file deletion for IE8+:

```html
<!--[if gte IE 8]><script src="js/cors/jquery.xdr-transport.js"></script><![endif]-->
```

The XDomainRequest Transport enables cross-domain GET and POST AJAX requests for IE8+.
However, since IE doesn't support AJAX file uploads, it doesn't allow cross-domain file uploads.

### PostMessage Transport
The [PostMessage Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) allows an alternative means of XHR cross-domain uploads, which doesn't require any cross-domain headers, but requires a [postMessage API](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) on the target server.

The [PostMessage Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) and the [postMessage API](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) make use of [window.postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) to transfer files between two different domains.

To use it, include the [PostMessage Transport script](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js):

```html
<script src="js/cors/jquery.postmessage-transport.js"></script>
```

And set the postMessage option to the location of the [postMessage API](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) on the target server:

```js
$('#fileupload').fileupload(
    'option',
    'postMessage',
    'http://example.org/postmessage.html'
);
```

The PostMessage API has [a configuration setting for which origin hosts to allow to upload files](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html#L25). Edit this setting to a regular expression for which domains you want to enable.

**Note:**
PostMessage transport uploads are currently only fully supported by Google Chrome.
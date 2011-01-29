# Known Issues

This page lists known issues that cannot be fixed by the plugin code but rather depend on browser or operating system specific issues or the design of HTTP/HTML/JavaScript/CSS.

## Content-type of the JSON response for iframe uploads
The file upload plugin makes use of iframes for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) uploads.  
They will only register a load event if the [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of the response is set to *text/plain* or *text/html*, **not** if it is set to *application/json*.

## Mozilla Firefox + GNU/Linux file managers
**Reported by [jni-](https://github.com/jni-):**  
If you are using linux and firefox, file drag&drop upload won't work with some file managers.  
Tested with thunar, pcmanfm and ROX. Nautilus seems to work.  
If you want all the details, [[have a look here|https://bugzilla.mozilla.org/show_bug.cgi?id=609284]]  
Chrome works fine with these file managers.
## JavaScript
* [jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js) is the basic plugin - it enhances the file upload process, but doesn't make any assumptions about the user interface or the content-type of the response.
* [jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js) is an extension to *jquery.fileupload.js*. The UI version expects JSON as the response content and adds a complete user interface.
* [jquery.iframe-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.iframe-transport.js) adds iframe transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [jquery.postmessage-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.postmessage-transport.js) adds [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [jquery.xdr-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.xdr-transport.js) adds XDomainRequest transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [application.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/application.js) is an example how to initialize and use the File Upload plugin.
* [test/test.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/test.js) contains the JS code for the unit tests.

## CSS
* [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.css) adds styling for the file input field, progress bars and upload buttons. See [[Style Guide]].
* [style.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/style.css) adds some basic styling for the example file upload form page.

## HTML
* [result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/result.html) is a helper page which allows to access cross-domain iframe contents via redirects.
* [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/index.html) is a HTML markup example for the file upload form and the upload/download templates and serves as demo page.
* [postmessage.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/postmessage.html) serves as an API endpoint for cross-domain [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) based file uploads.
* [test/index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/index.html) contains the HTML markup for the unit tests.

## Google App Engine
* [gae/main.py](https://github.com/blueimp/jQuery-File-Upload/blob/master/gae/main.py) is an example for a Python based server-side file upload handler component.
* [gae/app.yaml](https://github.com/blueimp/jQuery-File-Upload/blob/master/gae/app.yaml) is the configuration file for the App Engine application.

## PHP
* [php/index.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/php/index.php) is an example for a PHP based server-side file upload handler component.
* [php/files/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/php/files/.htaccess) contains instructions for [Apache](http://httpd.apache.org/) to serve all uploaded files with a content-type of *application/octet-stream*, except image files. This prevents executing any uploaded script files and makes sure non-image files produce a download dialog.
* [php/thumbnails/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/php/thumbnails/.htaccess) doesn't serve any purpose other than making sure the thumbnails folder shows up in the Git source code repository.

## Other
* [README.md](https://github.com/blueimp/jQuery-File-Upload/blob/master/README.md) contains basic plugin information in [markdown](http://daringfireball.net/projects/markdown/) format.
* [progressbar.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/progressbar.gif) is an animated GIf used for the animation of the upload progress bars.

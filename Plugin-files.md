## JavaScript
* [js/jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload.js) is the basic plugin - it enhances the file upload process, but doesn't make any assumptions about the user interface or the content-type of the response.
* [js/jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-ui.js) is an extension to *jquery.fileupload.js*. The UI version expects JSON as the response content and adds a complete user interface.
* [js/jquery.fileupload-fp.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-fp.js) extends the basic or UI version of the fileupload plugin and adds image processing (image resizing) functionality.
* [js/jquery.iframe-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js) adds iframe transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [js/main.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/main.js) is an example how to initialize and use the File Upload plugin.
* [js/cors/jquery.postmessage-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) adds [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [js/cors/jquery.xdr-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.xdr-transport.js) adds XDomainRequest transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [js/vendor/jquery.ui.widget.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/vendor/jquery.ui.widget.js) provides the [jQuery UI widget factory](http://wiki.jqueryui.com/w/page/12138135/Widget-factory).
* [test/test.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/test.js) contains the JS code for the unit tests.

## CSS
* [css/jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/jquery.fileupload-ui.css) adds styling for the file input field, progress bars and upload buttons. See [[Style Guide]].
* [css/style.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/style.css) is an example generic page style.

## HTML
* [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/index.html) is a HTML markup example for the file upload form and the upload/download templates and serves as demo page.
* [cors/result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/result.html) is a helper page which allows to access cross-domain iframe contents via redirects.
* [cors/postmessage.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) serves as an API endpoint for cross-domain [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) based file uploads.
* [test/index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/index.html) contains the HTML markup for the unit tests.

## Google App Engine Go
* [server/gae-go/app/main.go](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/app/main.go) is an example for a file upload handler component implemented in Go.
* [server/gae-go/app.yaml](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/app.yaml) is the configuration file for the Go App Engine application.

## Google App Engine Python
* [server/gae-python/main.py](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/main.py) is an example for a Python based server-side file upload handler component.
* [server/gae-python/app.yaml](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/app.yaml) is the configuration file for the Python App Engine application.

## Node.js
* [server/node/.gitignore](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/.gitignore) is a [Git configuration file](http://help.github.com/ignore-files/).
* [server/node/package.json](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/package.json) contains machine readable information about the node package in [JSON](http://www.json.org/) format.
* [server/node/server.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/server.js) is an example for a [Node.js](http://nodejs.org/) file upload handler implementation.

## PHP
* [server/php/index.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/index.php) is an example for a PHP upload handler.
* [server/php/upload.class.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/upload.class.php) is an example PHP upload handler class used by index.php.
* [server/php/files/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/files/.htaccess) contains instructions for [Apache](http://httpd.apache.org/) to serve all uploaded files with a content-type of *application/octet-stream*, except image files. This prevents executing any uploaded script files and makes sure non-image files produce a download dialog.
* [server/php/thumbnails/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/thumbnails/.htaccess) doesn't serve any purpose other than making sure the thumbnails folder shows up in the Git source code repository.

## Other
* [.gitignore](https://github.com/blueimp/jQuery-File-Upload/blob/master/.gitignore) is a [Git configuration file](http://help.github.com/ignore-files/).
* [README.md](https://github.com/blueimp/jQuery-File-Upload/blob/master/README.md) contains basic plugin information in [markdown](http://daringfireball.net/projects/markdown/) format.
* [img/progressbar.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/img/progressbar.gif) is an animated GIF image file used for the animation of the upload progress bars for browsers without support for CSS animations.
* [package.json](https://github.com/blueimp/jQuery-File-Upload/blob/master/package.json) contains machine readable information about the plugin in [JSON](http://www.json.org/) format.
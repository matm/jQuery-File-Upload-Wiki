* [cors](https://github.com/blueimp/jQuery-File-Upload/tree/master/cors)
    * [result.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/result.html) is a helper page which allows to access cross-domain iframe contents via redirects.
    * [postmessage.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/cors/postmessage.html) serves as an API endpoint for cross-domain [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) based file uploads.
* [css](https://github.com/blueimp/jQuery-File-Upload/tree/master/css)
    * [demo.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/demo.css]) is an example generic page style for the demos which don't rely on [Bootstrap](http://getbootstrap.com/), currently only used by the jQuery UI version.
    * [jquery.fileupload-ui-noscript.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/jquery.fileupload-ui-noscript.css) adjusts styling for browsers with JavaScript disabled.
    * [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/jquery.fileupload-ui.css) adds styling for the file input field, progress bars and upload buttons. See [[Style Guide]].
    * [style.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/style.css) is an example generic page style, accommodating the [Bootstrap](http://getbootstrap.com/) style which is used by all demos except the jQuery UI version.
* [img](https://github.com/blueimp/jQuery-File-Upload/tree/master/img)
    * [loading.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/img/loading.gif) is an animated GIF image file used for the file processing indication.
    * [progressbar.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/img/progressbar.gif) is an animated GIF image file used for the animation of the upload progress bars for browsers without support for CSS animations.
* [js](https://github.com/blueimp/jQuery-File-Upload/tree/master/js)
    * [app.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/app.js) is an example application for the AngularJS module.
    * [jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload.js) is the basic plugin - it enhances the file upload process, but doesn't make any assumptions about the user interface or the content-type of the response.
    * [jquery.fileupload-angular.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-angular.js) is an AngularJS file upload module depending on the File Upload (and processing) plugin.
    * [jquery.fileupload-audio.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-audio.js) extends the file processing plugin and adds audio preview functionality.
    * [jquery.fileupload-image.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-image.js) extends the file processing plugin and adds image preview & resize functionality.
    * [jquery.fileupload-jquery-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-jquery-ui.js) extends the UI version of the fileupload plugin to use it with jQuery UI.
    * [jquery.fileupload-process.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-process.js) extends the basic version of the fileupload plugin and adds file processing functionality.
    * [jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-ui.js) extends the file processing plugin and adds a complete user interface.
    * [jquery.fileupload-validate.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-validate.js) extends the file processing plugin and adds file validation functionality.
    * [jquery.fileupload-video.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.fileupload-video.js) extends the file processing plugin and adds video preview functionality.
    * [jquery.iframe-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js) adds iframe transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
    * [main.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/main.js) is an example how to initialize and use the File Upload plugin.
    * [cors](https://github.com/blueimp/jQuery-File-Upload/tree/master/js/cors)
        * [jquery.postmessage-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.postmessage-transport.js) adds [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
        * [jquery.xdr-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/cors/jquery.xdr-transport.js) adds XDomainRequest transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
    * [vendor](https://github.com/blueimp/jQuery-File-Upload/tree/master/js/vendor)
        * [jquery.ui.widget.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/vendor/jquery.ui.widget.js) provides the [jQuery UI widget factory](http://wiki.jqueryui.com/w/page/12138135/Widget-factory).
* [test](https://github.com/blueimp/jQuery-File-Upload/tree/master/test)
    * [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/index.html) contains the HTML markup for the unit tests.
    * [test.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/test/test.js) contains the JS code for the unit tests.
* [server](https://github.com/blueimp/jQuery-File-Upload/tree/master/server)
    * [gae-go](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-go)
        * [app](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-go/app)
            * [main.go](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/app/main.go) is an example for a file upload handler component implemented in Go.
        * [static](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-go/static)
            * [favicon.ico](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/static/favicon.ico) is the [favicon](http://en.wikipedia.org/wiki/Favicon) for the Go GAE upload handler. 
            * [robots.txt](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/static/robots.txt) is the directive for the [Robots exclusion standard](http://en.wikipedia.org/wiki/Robots_exclusion_standard) for the Go GAE upload handler.
        * [app.yaml](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-go/app.yaml) is the configuration file for the Go App Engine application.
    * [gae-python](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-python)
        * [static](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-python/static)
            * [favicon.ico](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/static/favicon.ico) is the [favicon](http://en.wikipedia.org/wiki/Favicon) for the Python GAE upload handler. 
            * [robots.txt](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/static/robots.txt) is the directive for the [Robots exclusion standard](http://en.wikipedia.org/wiki/Robots_exclusion_standard) for the Python GAE upload handler.
        * [main.py](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/main.py) is an example for a Python based server-side file upload handler component.
        * [app.yaml](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/gae-python/app.yaml) is the configuration file for the Python App Engine application.
    * [node](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/node)
        * [.gitignore](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/.gitignore) is a [Git configuration file](http://help.github.com/ignore-files/).
        * [package.json](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/package.json) contains machine readable information about the node package in [JSON](http://www.json.org/) format.
        * [server.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/node/server.js) is an example for a [Node.js](http://nodejs.org/) file upload handler implementation.
    * [php](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/php)
        * [files](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/php/files)
            * [.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/files/.htaccess) contains instructions for [Apache](http://httpd.apache.org/) to serve all uploaded files with a content-type of *application/octet-stream*, except image files. This prevents executing any uploaded script files and makes sure non-image files produce a download dialog.
        * [index.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/index.php) is an example for a PHP upload handler.
        * [UploadHandler.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/server/php/UploadHandler.php) is an example PHP upload handler class used by index.php.
* [.gitignore](https://github.com/blueimp/jQuery-File-Upload/blob/master/.gitignore) is a [Git configuration file](http://help.github.com/ignore-files/).
* [angularjs.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/angularjs.html) is a HTML markup example for the AngularJS file upload module.
* [basic.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/basic.html) is a HTML markup example for the basic file upload plugin.
* [basic-plus.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/basic-plus.html) is a HTML markup example for the basic file upload plugin extended with the processing, image resizing and validation plugins.
* [blueimp-file-upload.jquery.json](https://github.com/blueimp/jQuery-File-Upload/blob/master/blueimp-file-upload.jquery.json) is a [JSON](http://www.json.org/) file for the [jQuery Plugin Registry](http://plugins.jquery.com/).
* [CONTRIBUTING.md](https://github.com/blueimp/jQuery-File-Upload/blob/master/CONTRIBUTING.md) contains contribution information in [markdown](http://daringfireball.net/projects/markdown/) format.
* [index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/index.html) is a HTML markup example for the file upload form and the upload/download templates and serves as demo page.
* [jquery-ui.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery-ui.html) is a HTML markup example for the jQuery UI version of the file upload plugin.
* [package.json](https://github.com/blueimp/jQuery-File-Upload/blob/master/package.json) contains machine readable information about the plugin in [JSON](http://www.json.org/) format.
* [README.md](https://github.com/blueimp/jQuery-File-Upload/blob/master/README.md) contains basic plugin information in [markdown](http://daringfireball.net/projects/markdown/) format.
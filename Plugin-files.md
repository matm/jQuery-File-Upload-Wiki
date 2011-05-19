## JavaScript
* [jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js) is the basic plugin - it enhances the file upload process, but doesn't make any assumptions about the user interface or the content-type of the response.
* [jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js) is an extension to *jquery.fileupload.js*. The UI version expects JSON as the response content and adds a complete user interface.
* [jquery.iframe-transport.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.iframe-transport.js) adds iframe transport support to [jQuery.ajax()](http://api.jquery.com/jQuery.ajax/).
* [jquery.image-gallery.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.image-gallery.js) adds gallery functionality for uploaded images and exposes a *scale* method that is used for the preview images.
* [example/application.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/application.js) is an example how to initialize and use the File Upload plugin.
* [tests/tests.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/tests/tests.js) contains the JS code for the unit tests.

## CSS
* [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.css) adds styling for the file input field, progress bars and upload buttons. See [[Style Guide]].
* [example/style.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/style.css) adds some basic styling for the example file upload form page.

## HTML
* [example/index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/index.html) is a HTML markup example for the file upload form and the upload/download templates.
* [tests/index.html](https://github.com/blueimp/jQuery-File-Upload/blob/master/tests/index.html) contains the HTML markup for the unit tests.

## PHP
* [example/upload.php](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/upload.php) is an example for a server-side file upload handler component.

## Other
* [.gitignore](https://github.com/blueimp/jQuery-File-Upload/blob/master/.gitignore) contains instructions which [files to ignore for the Git source code repository](http://help.github.com/git-ignore/).
* [README.md](https://github.com/blueimp/jQuery-File-Upload/blob/master/README.md) contains basic plugin information in [markdown](http://daringfireball.net/projects/markdown/) format.
* [README.txt](https://github.com/blueimp/jQuery-File-Upload/blob/master/README.txt) contains basic plugin information in plain text format.
* [loading.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/* [pbar-ani.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/pbar-ani.gif) is an animated GIf used for the loading animation of the image gallery dialogs.
* [pbar-ani.gif](https://github.com/blueimp/jQuery-File-Upload/blob/master/pbar-ani.gif) is an animated GIf used for the animation of the upload progress bars.
* [example/files/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/files/.htaccess) contains instructions for [Apache](http://httpd.apache.org/) to serve all uploaded files with a content-type of *application/octet-stream*, except image files. This prevents executing any uploaded script files and makes sure non-image files produce a download dialog.
* [example/thumbnails/.htaccess](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/thumbnails/.htaccess) doesn't serve any purpose other than making sure the thumbnails folder shows up in the Git source code repository.
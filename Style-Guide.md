## File input styling
Most browsers prevent submitting files when the input field didn't receive a direct click (or keyboard) event in one way (preventing the click event) or another (not actually submitting any files) as a security precaution.  
Firefox 4 (and later) is so far the only browser which supports invoking "click"-Events on a completely hidden (*display: none*) file input field. This will eventually allow to use any element as button for file selects when this feature is supported across browsers.  
The programmatic triggering of the click event itself is supported on IE, but no files will be submitted on subsequent form submits.

To achieve a cross-browser styling of the "Upload files" button, the file input field is made transparent and positioned on top of the *fileinput-button* label - see [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.css).

## Bootstrap UI
The current version of the plugin builds on Twitter's [Bootstrap](http://twitter.github.com/bootstrap/) toolkit for it's look&feel.

## jQuery UI
An older version of the plugin is available, which builds on [jQuery UI's theme framework](http://jqueryui.com/themeroller/) for it's look&feel.  
It is available via the [jquery-ui](https://github.com/blueimp/jQuery-File-Upload/tree/jquery-ui) branch and can be downloaded here:
https://github.com/blueimp/jQuery-File-Upload/archives/jquery-ui
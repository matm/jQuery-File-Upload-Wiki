The jQuery File Upload UI version of the plugin builds on [jQuery UI's theme framework](http://jqueryui.com/themeroller/) for it's look&feel.  
By including different themeroller themes, the design can be easily changed. Have a look at the theme switcher on the demo page.

To get the file input button to look like a standard jQuery UI button, some hacks have to be applied, as historically, browsers only allow a very limited styling of file input fields.

Firefox 4 (and later) is so far the only browser which supports invoking "click"-Events on a completely hidden (*display: none*) file input field. This will eventually allow to use any element as button for file selects when this feature is supported across browsers.  
The click event itself is supported on IE, but no files will be submitted on subsequent form submits.
Several browsers prevent submitting files when the input field didn't receive a direct click (or keyboard) event in one way (preventing the click event) or another (not actually submitting any files) as a security precaution.

Another solution would be to invoke the click event on an invisible (*visibility: hidden* and *width* and *height* set to *1px*) file input field, but this is not supported by Firefox v. 3.6 and Opera v. 11.01.

So far, to achieve a cross-browser styling of the "Upload files" button, the file input field is made transparent and positioned on top of the *fileinput-button* label - see [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.css).

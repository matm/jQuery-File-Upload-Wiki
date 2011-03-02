The upload button styling is a rather ugly hack.
Historically, browsers only allow a very limited styling of file input fields.

Firefox 4 is so far the only browser which supports invoking "click"-Events on a hidden (*display: none*) file input field. This will eventually allow to use any element as button for file selects when this feature is supported across browsers.

So far, to achieve a cross-browser styling of the "Upload files" button, the file input field is made transparent and positioned on top of the *file_upload* container - see [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.css).

However, the file input field looks different depending on the browser, the operating system and the language setting which requires some adjustments to get a clickable result across those versions.

## Making additional form fields visible
By default, the CSS code provided with the plugin covers the whole form with the file input field.  
To get additional form fields to show up, some CSS adjustments are needed - please refer to the section "Sending additional Form Data by adding input fields to the upload form" of [[How to submit additional Form Data]].
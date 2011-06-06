## Multiple File selection
The following browsers support multiple file selection:

* Firefox 3.6+
* Safari 5+
* Google Chrome
* Opera 11+

Internet Explorer has no support for multiple file selection, but allows to add multiple files to the upload queue by selecting files multiple times.

## Drag & Drop
The following browsers support drag & drop:

* Firefox 4+
* Safari 5+
* Google Chrome

IE and Opera have currently no support for drag & drop.

### Firefox 3.6
The outdated v4 branch of the plugin supported drag & drop for Firefox 3.6, but since version 5 of the plugin, drag&drop is not supported on versions below Firefox 4 anymore (for multipart uploads).

The reason for this is that Firefox 3.6 does not support the FormData interface for multipart form uploads and required extra code using the FileReader interface to built a multipart/form-data upload. This code was rather inefficient for large files and since version 4 of Firefox is stable and widespread and selecting files still works on Firefox 3.6, I (the developer) decided to remove the extra code for the rewritten version 5 of the plugin.

It is still possible to enable drag & drop support on Firefox 3.6 with version 5 of the plugin by setting the multipart option to *false* (see [[Options]]).

## Upload progress
The following browsers have complete support for upload progress indication:

* Firefox 4+
* Safari 5+
* Google Chrome

Firefox 3.6 and Opera 11.1 have partial upload progress support via the global progress bar, which will update after each iframe based upload with the percentage of the uplaoded files compared to all file selections in the upload queue. This is possible as those browsers support the File API and report the file size of the uploaded files, although they lack the interfaces for XMLHttpRequest uploads.

All versions of Internet Explorer will also update the global progress bar after each iframe based upload. However since IE does not report the file size, the global progress bar will measure each uploaded file the same without regard to the size of the uploaded file.

## Image previews
The following browsers have support for image previews prior to uploading files:

* Firefox 4+
* Google Chrome
* Opera 11+ (some images seem to display incorrectly on Opera)

## XMLHttpRequest File Uploads
The following browsers support [XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads, which allows advanced usage of the file upload [[API]]:

* Firefox 4+
* Safari 5+
* Google Chrome

If the *multipart* option is set to *false* (see [[Options]]), Firefox 3.6 also supports [XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads.

The [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.iframe-transport.js) is used for other browsers. The Iframe Transport requires a file input selection to upload files.
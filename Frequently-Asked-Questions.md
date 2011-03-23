## Does the plugin have to be called on a form tag?
Technically, the form is only required for iframe uploads.
If you define the *url*, *method* and *fieldName* [[Options]], you can call the plugin on any element - no form or file input field required - and the drag and drop functionality will still work.
The file input button functionality would technically also work without a form, but the plugin code currently requires the file input to be part of a form.
But without support for iframe uploads, you will leave users of IE and Opera out in the dark.

However, what you can do is using the *dropZone* option to define a container inside of the form as dropZone target.
You can also adjust the CSS code so the file input field does not cover the whole form.

Please have a look at [[How to submit additional Form Data]], specifically the second section about "Sending additional Form Data by adding input fields to the upload form".

## Internet Explorer prompts to download a file after the upload completes - why?
The file upload plugin makes use of iframes for browsers like Microsoft Internet Explorer and Opera, which do not yet support XMLHTTPRequest uploads.
They will only register a load event if the Content-type of the response is set to text/plain or text/html, not if it is set to application/json. Setting the content-type of the JSON response to "text/html" should usually fix problems where IE shows an unwanted download dialog.  
Please have a look at the [[Setup]] instructions.

## Why do I get a JSON parsing error?
Your JSON response is probably not valid JSON.  
You can test your JSON response for validity on [jslint.com](http://www.jsonlint.com/).

## How to restrict the file selection dialog to show only specific file types?
You can use the accept attribute of the file input field to limit the file type selection, though this seems to be supported only on Google Chrome and Opera.  
An example limiting files to PNG images:
```html
<input type="file" name="file" accept="image/png" multiple>
```

Note that this will not limit files added by drag&drop and is not supported across all browsers.

## What is the maximum file size limitation?
It is possible to upload large files (> 1 GB) with the jQuery File Upload plugin, but with some reservations.

HTTP might not be the ideal protocol for uploading large files.
Until the Blob API get's better support across browsers and allows chunked file uploads, there is also no support to resume aborted uploads.

Apart from the implementation used for Firefox 3.6 (and other browsers which support the File API and XHR file uploads, but not the FormData API), the jQuery File Upload plugin doesn't put anything on top of the HTTP upload implementation of the browser.
So any files that can be uploaded with a simple HTML form upload can be uploaded with the jQuery File Upload plugin as well.

See also [issue #103](https://github.com/blueimp/jQuery-File-Upload/issues/103).

## Is it possible to trigger the file selection dialog programmatically?
Invoking the click event on a file input field is **not** supported on the following browsers:

* Firefox 3.6 (tested on OSX and Windows XP)
* Opera 11.01 (tested on OSX)

It works on the other major browsers including Firefox 4, so it might be a feasible solution in the future.

See also [issue #92](https://github.com/blueimp/jQuery-File-Upload/issues/92).

## How to implement the server-side file handling or file deletion functionality?
This plugin requires you to build the server-side file handling (including the deletion of files) yourself as it focuses solely on enhancing a standard form-based file upload.  
It is meant to be used by developers who can easily customize it and adapt it to work with their server-side web application framework.  
However, if you need inspiration you can have a look at the [[Demo implementation]].  
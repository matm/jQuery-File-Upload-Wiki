## Does the plugin require a form or file input field?
If you define the *url* (and probably *paramName*) [[Options]], you can call the plugin on any element - no form or file input field required - and the drag&drop functionality will still work.  
To support browsers without [XHR](https://developer.mozilla.org/en/xmlhttprequest) file upload capabilities, a file input field has to be part of the widget, or defined using the *fileInput* option.

## Why does Internet Explorer prompt to download a file after the upload completes?
The file upload plugin makes use of an Iframe Transport module for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
Iframe based uploads require a [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of *text/plain* or *text/html* for the JSON response - they will show an undesired download dialog if the iframe response is set to *application/json*.   
Please have a look at the Content-Type Negotiation section of the [[Setup]] instructions.

## Why do I get a JSON parsing error?
Your JSON response is probably not valid JSON.  
You can test your JSON response for validity on [jsonlint.com](http://www.jsonlint.com/).

## How to restrict the file selection dialog to show only specific file types?
You can use the *accept* attribute of the file input field to limit the file type selection, though this seems to be supported only on Google Chrome and Opera.  
An example limiting files to PNG images:

```html
<input type="file" name="files[]" accept="image/png" multiple>
```

Note that this will not limit files added by drag&drop and is not supported across all browsers.

## What is the maximum file size limitation?
It is possible to upload large files (> 1 GB) with the jQuery File Upload plugin, but with some reservations.  
HTTP might not be the ideal protocol for uploading large files, but the jQuery File Upload plugin doesn't put any layer on top of the HTTP upload implementation of the browser.  
So any files that can be uploaded with a simple HTML form can be uploaded with the jQuery File Upload plugin as well.

## Is it possible to trigger the file selection dialog programmatically?
Invoking the click event on a file input field is **not** supported on the following browsers:

* Firefox 3.6 (tested on OSX and Windows XP)
* Opera 11.01 (tested on OSX)

It works on the other major browsers including Firefox 4, so it might be a feasible solution in the future.  
See also: [[Style Guide]].

## How to use the *this* keyword inside of the plugin initialization options?
Just make use of [jQuery's each method](http://api.jquery.com/each/) to set the *this* keyword to the element node:

```js
$('#fileupload').each(function () {
    $(this).fileupload({
        fileInput: $(this).find('input:file')
    });
});
```

## How to limit the file selection so users can only select one file?
Just remove the *multiple* attribute from the file input:

```html
<input type="file" name="files[]">
```

Note that users can still drag&drop multiple files. To enforce a one file upload limit, you can make use of the *maxNumberOfFiles* option (see [[Options]]).

## Does the plugin support HTTP status codes?
The File Upload plugin will properly handle HTTP response codes when the browser supports [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.
It even displays the correct error message, e.g. "Error: Service Unavailable" for the following HTTP header :

    HTTP/1.0 503 Service Unavailable

However, for browsers without support for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads - which includes Internet Explorer and Opera - the [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.iframe-transport.js) is used and there is no way to retrieve the HTTP status code from an iframe load event.

## Is it possible to resize images prior to uploading?
It's technically possible (via the canvas element and replacing the File objects with Blobs), but currently only supported on Mozilla Firefox (via [canvas.mozGetAsFile](https://developer.mozilla.org/en/DOM/HTMLCanvasElement)). As soon as Google Chrome and other browsers support the [BlobBuilder](http://dev.w3.org/2009/dap/file-system/file-writer.html) interface, it's also possible on those browsers.

## Ist is possible to drag&drop a folder of files?
The current browser APIs only support drag&drop of (multiple) files, not of folders.  
See also [issue #573](https://github.com/blueimp/jQuery-File-Upload/issues/573).
## Does the plugin require a form or file input field?
If you define the *url* (and probably *paramName*) [[Options]], you can call the plugin on any element - no form or file input field required - and the drag&drop functionality will still work.  
To support browsers without XHR file upload capabilities, a file input field has to be part of the widget, or defined using the *fileInput* option.

## Internet Explorer prompting to download a file after the upload completes
The file upload plugin makes use of an Iframe Transport module for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
Iframe based uploads require a [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of *text/plain* or *text/html* for the JSON response - they will show an undesired download dialog if the iframe response is set to *application/json*.   
Please have a look at the Content-Type Negotiation section of the [[Setup]] instructions.

## Why do I get a JSON parsing error?
Your JSON response is probably not valid JSON.  
You can test your JSON response for validity on [jsonlint.com](http://www.jsonlint.com/).

## How to restrict the file selection dialog to show only specific file types?
You can use the *accept* attribute of the file input field to limit the file type selection, though this seems to be supported only on Google Chrome and Opera.  
An example limiting files to PNG images:

```html
<input type="file" name="file" accept="image/png" multiple>
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
See also [[Style Guide]].

## How to use the *this* keyword inside of the plugin initialization options?
Just make use of [jQuery's each method](http://api.jquery.com/each/) to set the *this* keyword to the element node:
```js
$('#fileupload').each(function () {
    $(this).fileupload({
        fileInput: $(this).find('input:file')
    });
});
```
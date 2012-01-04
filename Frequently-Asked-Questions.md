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

## Why do I get an empty file upload result when uploading large files?
You probably have a server-side setting preventing you to upload larger files.  
Try adding the following to a [.htaccess](http://httpd.apache.org/docs/current/howto/htaccess.html) file in the php directory:

```
php_value upload_max_filesize 99999M
php_value post_max_size 99999M
```

If this doesn't work, try creating a [php.ini](http://www.php.net/manual/en/ini.php) file in the php directory and add the following two lines:

```
upload_max_filesize = 99999
post_max_size = 99999
```

If this also doesn't work, contact your hosting provider.

## What is the maximum file size limitation?
It is possible to upload large files (> 1 GB) with the jQuery File Upload plugin, but with some reservations.  
HTTP might not be the ideal protocol for uploading large files, but the jQuery File Upload plugin doesn't put any layer on top of the HTTP upload implementation of the browser.  
So any files that can be uploaded with a simple HTML form can be uploaded with the jQuery File Upload plugin as well.

## How to prevent the page from becoming unresponsive, when adding a large number of image files?
Remove the "preview" class from the upload template to avoid rendering the preview images, which have the potential to block the main JS thread.

## Is it possible to trigger the file selection dialog programmatically?
Invoking a click event on the file input field programmatically is not supported across browsers - see [[Style Guide]].

However, another file input button can be used to trigger the file selection and passed as parameter to the fileupload **add** or **send** [[API]]:

```js
$('#some-file-input-field').bind('change', function (e) {
    $('#fileupload').fileupload('add', {
        files: e.target.files || [{name: this.value}],
        fileInput: $(this)
    });
});
```

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

## Is it possible to drag&drop a folder of files?
The current browser APIs only supports drag&drop of (multiple) files, not of folders.  
See also [issue #573](https://github.com/blueimp/jQuery-File-Upload/issues/573).  
However, it is possible to allow selecting a folder (instead of files) via the file input element by adding browser-vendor specific "directory" attributes:

```html
<input type="file" name="files[]" multiple directory webkitdirectory mozdirectory>
```

Note that this currently only works in the latest versions of Google Chrome and Firefox.

## How to clear the list of uploaded files?
The example in the download package comes with code that retrieves and displays a list of existing files.  
Remove the **$.getJSON** call from [application.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/example/application.js) if you don't need that functionality.

## Why is the protocol ("http:") missing from the script references in the HTML source code?
This is called a [protocol relative url](//www.google.com/search?q=protocol+relative+URL) and a perfectly valid way to define a resource, relative to the current URL protocol.  
This ensures that the referenced scripts are loaded via the same protocol as the current page, which avoids security notifications when loading resources via unencrypted HTTP on a page loaded via HTTPS.

However, it also requires that the current protocol is either "http:" or "https:" and will not work on a "file:" url.

See also issue [#514](https://github.com/blueimp/jQuery-File-Upload/issues/514) as well as pull requests [#722](https://github.com/blueimp/jQuery-File-Upload/pull/772) and [#833](https://github.com/blueimp/jQuery-File-Upload/pull/833).

## Why can't I print the files index in the template for loop?
The template will be rendered for each **add** call.  
As long as the option **singleFileUploads** is set to *true* (which is the default), multiple selects/drops get split up into single **add** calls, so the index will always be **0**.

Please see the comments for [Issue #893](https://github.com/blueimp/jQuery-File-Upload/issues/893).

## Why does Firefox never show 100% upload progress?
See Firefox Bug [#642463](https://bugzilla.mozilla.org/show_bug.cgi?id=642463).  
If desired, you can workaround this bug by manually setting the progress bar to 100% when the load event (done event) fires.
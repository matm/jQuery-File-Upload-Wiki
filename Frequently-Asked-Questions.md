## Client-side

### What is the maximum file size limitation?
It is possible to upload files up to **4 GB** with the jQuery File Upload plugin.  
By making use of [[Chunked file uploads]] (with chunks smaller than 4GB), the potential file size is unlimited.  
The restriction of 4 GB is due to some browser limitations, which might be fixed in future updates to those browsers:

Firefox bug references:  
https://bugzilla.mozilla.org/show_bug.cgi?id=215450  
https://bugzilla.mozilla.org/show_bug.cgi?id=660159

Chrome bug references:  
http://code.google.com/p/chromium/issues/detail?id=139815

### Does the plugin require a form or file input field?
If you define the *url* (and probably *paramName*) [[Options]], you can call the plugin on any element - no form or file input field required - and the drag&drop functionality will still work.  
To support browsers without [XHR](https://developer.mozilla.org/en/xmlhttprequest) file upload capabilities, a file input field has to be part of the widget, or defined using the *fileInput* option.

### How to restrict the file selection dialog to show only specific file types?
You can use the *accept* attribute of the file input field to limit the file type selection, though this seems to be supported only on Google Chrome and Opera.  
An example limiting files to PNG images:

```html
<input type="file" name="files[]" accept="image/png" multiple>
```

Note that this will not limit files added by drag&drop and is not supported across all browsers.

### How to prevent the page from becoming unresponsive, when adding a large number of image files?
Lower the **loadImageMaxFileSize** setting or remove the "preview" class from the upload template to avoid rendering large preview images, which have the potential to block the main JS thread.

### Is it possible to trigger the file selection dialog programmatically?
Invoking a click event on the file input field programmatically is not supported across browsers - see [[Style Guide]].

However, another file input button can be used to trigger the file selection and passed as parameter to the fileupload **add** or **send** [[API]]:

```js
$('#some-file-input-field').bind('change', function (e) {
    $('#fileupload').fileupload('add', {
        files: e.target.files || [{name: this.value}],
        fileInput: $(this)
    });
});
```

### How to use the *this* keyword inside of the plugin initialization options?
Just make use of [jQuery's each method](http://api.jquery.com/each/) to set the *this* keyword to the element node:

```js
$('#fileupload').each(function () {
    $(this).fileupload({
        fileInput: $(this).find('input:file')
    });
});
```

### How to limit the file selection so users can only select one file?
Just remove the *multiple* attribute from the file input:

```html
<input type="file" name="files[]">
```

Note that users can still drag&drop multiple files. To enforce a one file upload limit, you can make use of the *maxNumberOfFiles* option (see [[Options]]).

### Is it possible to resize images prior to uploading?
This has been built-in and is currently supported by the latest versions of Mozilla Firefox and Google Chrome. Please have a look at the **process** [[Options]].

### Is it possible to drag&drop a folder of files?
Yes, with the latest version of Google Chrome it is possible.  
It is also possible to allow selecting a folder (instead of files) via the file input element by adding browser-vendor specific "directory" attributes, though this seems to be only supported in Google Chrome so far:

```html
<input type="file" name="files[]" multiple directory webkitdirectory mozdirectory>
```

See also [issue #573](https://github.com/blueimp/jQuery-File-Upload/issues/573).

### Why is the protocol ("http:") missing from the script references in the HTML source code?
This is called a [protocol relative url](//www.google.com/search?q=protocol+relative+URL) and a perfectly valid way to define a resource, relative to the current URL protocol.  
This ensures that the referenced scripts are loaded via the same protocol as the current page, which avoids security notifications when loading resources via unencrypted HTTP on a page loaded via HTTPS.

However, it also requires that the current protocol is either "http:" or "https:" and will not work on a "file:" url.

See also issue [#514](https://github.com/blueimp/jQuery-File-Upload/issues/514) as well as pull requests [#722](https://github.com/blueimp/jQuery-File-Upload/pull/772), [#833](https://github.com/blueimp/jQuery-File-Upload/pull/833) and [#1789](https://github.com/blueimp/jQuery-File-Upload/pull/1789).

### Why can't I print the files index in the template for loop?
The template will be rendered for each **add** call.  
As long as the option **singleFileUploads** is set to *true* (which is the default), multiple selects/drops get split up into single **add** calls, so the index will always be **0**.

Please see the comments for [Issue #893](https://github.com/blueimp/jQuery-File-Upload/issues/893).

### Why does Firefox never show 100% upload progress?
See Firefox Bug [#642463](https://bugzilla.mozilla.org/show_bug.cgi?id=642463).  
This bug has been addressed with commit [f60bbfb2546bc08fe9538d3af56be8d07e634675](https://github.com/blueimp/jQuery-File-Upload/commit/f60bbfb2546bc08fe9538d3af56be8d07e634675).

### Why does the plugin display 1000 Bytes as 1 KB (and 1000000 Bytes as 1 MB)?
The plugin makes use of [metric prefixes](http://en.wikipedia.org/wiki/SI_prefix) in conformance of the [International System of Units](http://en.wikipedia.org/wiki/International_System_of_Units).
This is the same unit system that is used by hard drive manufacturers and e.g. the Mac OSX operating system to report hard drive capacities.
Unfortunately, the terms "kilobytes", "megabytes", etc. have historically been used in ambiguous meanings. Please have a look at the [Binary Prefix](http://en.wikipedia.org/wiki/Binary_prefix) article on Wikipedia for background information.

### Why doesn't the file input field show the path of a selected file?
The plugin provides CSS classes for a custom file input button, which doesn't display the selected file name. However they can be removed to display the native browser file input button - see [[Style Guide]].  
By default, the plugin also replaces the file input button after each file(s) selection. This behaviour can be disabled by setting the option [replaceFileInput](https://github.com/blueimp/jQuery-File-Upload/wiki/Options#replacefileinput) to *false*.

### Why is the file input field cloned and replaced after each selection?
The cloning is done for two reasons:

* First to make sure a change event is fired even if the same file (or filename) is selected subsequently.
* Second to allow upload queues for the iframe transport.

If you have one file input and select e.g. the file "example.jpg" and then you abort the upload for some reason, there will be no change event fired if you select the same file to retry the upload.
Replacing the original file input with a clone (and resetting the clone's value property) fixes this problem.

Uploads using the iframe transport rely on the file input field, as iframe transport uploads are simple HTML form uploads with an iframe as POST target.
If you select a file with a file input field, the file reference is tied to the input field. Modern browsers also allow to access the selected files via the File API, but for the iframe transport, the input field itself is the only usable reference.
You can't select another file with this file input field until the associated form has been submitted, else you will loose the original selection.
To allow queuing files with the iframe transport, you have to keep the original input field and add a new input field for the next user selection.
So, replacing the original file input with a clone (and keeping the original until it has been used for a form submission) also fixes the queuing problem.

The *fileInput* option is supposed to be a reference to the collection of file input fields the plugin is listening to for change events. So when the original file input fields are replaced with their clones, the clones have to take their place and the *fileInput* reference needs to be updated.

### Why are files with a slash as part of the filename listed with a colon instead? (OSX)
When you create a file with a slash as part of the filename on OSX, it's actually represented with a colon instead if you list the file on the Terminal.  
You can test it by renaming an existing file in Finder to e.g. "test/example" and then list the contents of the file's directory using the OSX Terminal, where it will show up as "test:example".  
The Terminal version is the one that gets reported as file name if you select or drop this file on a website.
This is done because the slash is actually a directory separator on OSX:  
http://stackoverflow.com/a/13298479

The name property of File objects is actually readonly, so the plugin doesn't do anything here:  
https://developer.mozilla.org/en-US/docs/Web/API/File#Properties

The only thing the plugin does regarding file names is remove path information from files reported by older browsers (old IE versions) that don't support the File API.

### Why do multiple selected files only show one start/cancel button when the *forceIframeTransport* option is set to *true*?
If you force the use of the Iframe Transport, it is still possible to select multiple files on modern browsers.  
However, the Iframe Transport requires that all selections have to be uploaded in one request, simply because the file input is used for a standard HTML form submit to a hidden iframe and it's not possible to separate the selected files this way.

The UI reflects this by showing only one set of start/cancel buttons for each selection to be uploaded.
It might not be the ideal user interface for this kind of situation, but it allows to reuse the same upload templates with minimal code additions.

### Why aren't custom headers sent on some browsers (namely IE version below IE10)?
Only browsers with support for XHR file upload support setting custom headers.  
See [[Browser support]] for more information.

### Why does the plugin only provide a dragover event, not other drag related events?
The plugin only has its own dragover event handler to be able to apply the *copy* drop effect and to call the *preventDefault* method on the event object, which are both required to make cross-browser file drag&drop possible.  
The plugin's dragover callback trigger allows to prevent that behavior by returning *false* in the callback, which can be useful in some situations.

You can easily bind event handlers for any drag event independently of the plugin with jQuery alone, e.g.:

```js
$('#dropzone').bind('dragleave', function (e) {
    // dragleave event handler code
});
```

## Server-side

### Why does Internet Explorer prompt to download a file after the upload completes?
The file upload plugin makes use of an Iframe Transport module for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
Iframe based uploads require a [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of *text/plain* or *text/html* for the JSON response - they will show an undesired download dialog if the iframe response is set to *application/json*.   
Please have a look at the Content-Type Negotiation section of the [[Setup]] instructions.

### Why doesn't Internet Explorer show HTML snippets as part of the JSON response?
This is due to how the response content is parsed when using the iframe transport, which is required by Internet Explorer.  
Have a look at [an explanation and solution from user espeoneefi](https://github.com/blueimp/jQuery-File-Upload/issues/659#issuecomment-2999298).

### Why do I get a JSON parsing error?
Your JSON response is probably not valid JSON.  
You can test your JSON response for validity on [jsonlint.com](http://www.jsonlint.com/).  
To retrieve the JSON response, make use of the network tab of your browser development tools (e.g. Google Chrome's and Safari's developer console or Firebug).

### Why do I get an empty file upload result when uploading large files (PHP)?
You probably have a server-side setting preventing you to upload larger files.  
Try adding the following to a [.htaccess](http://httpd.apache.org/docs/current/howto/htaccess.html) file in the php directory:

```
php_value upload_max_filesize 9G
php_value post_max_size 9G
php_value max_execution_time 200
php_value max_input_time 200
php_value memory_limit 256M
```

If this doesn't work, try creating a [php.ini](http://www.php.net/manual/en/ini.php) file in the php directory and add the following lines:

```
upload_max_filesize 9G
post_max_size 9G
max_execution_time 200
max_input_time 200
memory_limit 256M
```

If this also doesn't work, contact your hosting provider.

### Is there a problem uploading files with non-ASCII characters (PHP, Windows server)?
If your non-ASCII file names get uploaded with strange characters like Ã¤, Ã¶ or Ã¼ you probably need to apply the [utf8-decode()](http://de.php.net/manual/en/function.utf8-decode.php) method on the file names of uploaded files, e.g. by overriding the *trim_file_name* method:

```php
<?php
require('upload.class.php');

class CustomUploadHandler extends UploadHandler {
    protected function trim_file_name($name, $type, $index) {
        $name = utf8_decode($name);
        return parent::trim_file_name($name, $type, $index);
    }
}

$upload_handler = new CustomUploadHandler();
```

### Why does string comparison fail with non-ASCII file names returned from the server?
Depending on your server-environment, you might have to do [Unicode normalization](http://unicode.org/faq/normalization.html), to achieve the same binary representation of strings with Unicode characters.  
See also issue [#1339](https://github.com/blueimp/jQuery-File-Upload/issues/1339).

### Does the plugin support HTTP status codes?
The File Upload plugin will properly handle HTTP response codes when the browser supports [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads.
It even displays the correct error message, e.g. "Error: Service Unavailable" for the following HTTP header :

    HTTP/1.0 503 Service Unavailable

However, for browsers without support for [XHR](https://developer.mozilla.org/en/xmlhttprequest) file uploads - which includes Internet Explorer before IE10 - the [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js) is used and there is no way to retrieve the HTTP status code from an iframe load event.
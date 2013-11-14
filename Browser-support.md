## File uploads
The following browsers support at least one form of [AJAX](https://en.wikipedia.org/wiki/Ajax_%28programming%29) style file uploads, either via [XHR](https://developer.mozilla.org/en/XmlHttpRequest) or via the [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js):

### Desktop browsers

* Google Chrome
* Apple Safari 4.0+
* Mozilla Firefox 3.0+
* Opera 11.0+
* Microsoft Internet Explorer 6.0+

### Mobile browsers

* Apple Safari on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+
* Default Browser on Android 2.3+
* Opera Mobile 12.0+

See also [File upload support on mobile](http://viljamis.com/blog/2012/file-upload-support-on-mobile/).

## Multiple File selection
The following browsers support multiple file selection:

### Desktop browsers

* Google Chrome
* Apple Safari 5.0+
* Mozilla Firefox 3.6+
* Opera 11.0+
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+

### Known bugs
Safari on Windows has [a bug](http://stackoverflow.com/questions/7231054/file-input-size-issue-in-safari-for-multiple-file-selection) and reports a file size of 0 bytes when selecting multiple files. Selecting single files works.

### Folder selection
It is possible to select a complete folder structure, though this is currently only supported by Google Chrome.  
To enable this feature, the following vendor-specific **directory** attributes have to be added to the file input field:

```js
<input type="file" name="files[]" multiple directory webkitdirectory mozdirectory>
```

## Drag & Drop
The following browsers support drag & drop:

* Google Chrome
* Apple Safari 5.0+
* Mozilla Firefox 4.0+
* Opera 12.0+ (broken, see Issue [#2080](https://github.com/blueimp/jQuery-File-Upload/issues/2080))
* Microsoft Internet Explorer 10.0+

### Known bugs
Safari on Windows has a bug that adds "￿" characters at the end of the file name or mangles the file name in another way and reports a file size of 0 bytes when dropping files. Trying to drop the same file(s) again seems to work.

### Folder Drag&Drop
It is possible to drag&drop a complete folder structure, though this is currently only supported by Google Chrome.

## Copy&Paste of images
The following browsers support pasting images from the clipboard:

* Google Chrome

## Upload progress
The following browsers have complete support for upload progress indication:

### Desktop browsers

* Google Chrome
* Apple Safari 5.0+
* Mozilla Firefox 4.0+
* Opera 12.0+
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on Android 4.0+
* Opera Mobile 12.0+

### Differences in upload progress events
Depending on the size of the file, the operating system platform and the network throughput, the number of progress events triggered might differ.  
e.g. Google Chrome might only trigger one progress event for the upload of a 100KB file on Windows XP, but trigger multiple progress events for the same file on Windows 7 and Mac OSX (tested with Google Chrome version 22).

Please also note that some programs that analyze network traffic are known to interfere with the number of progress events triggered, e.g. Web Debugging Proxies or Firewall/Antivirus software.

## Image previews
The following browsers have support for image previews prior to uploading files:

### Desktop browsers

* Google Chrome
* Apple Safari 6.0+
* Mozilla Firefox 3.6+
* Opera 11.0+
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+
* Default Browser on Android 3.2+

### Known bugs

* The default browser on Android 4.1.2 doesn't seem to support image previews.

## File meta data
The following browsers report complete file meta data prior to uploading files:

### Desktop browsers

* Google Chrome
* Apple Safari 4.0+
* Mozilla Firefox 3.6+
* Opera 10.0+
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+
* Default Browser on Android 3.2+
* Opera Mobile 12.0+

### File meta data properties
The reported file meta data consists of the following properties:

* **name**: The name of the file, e.g. *banana.jpg*.
* **size**: The size of the file in bytes, e.g. *130073* (131 KB).
* **type**: The type of the file, e.g. *image/jpg*.

### Known bugs
The default browser on Android 4.1.2 doesn't report the file type.

## XMLHttpRequest File Uploads
The following browsers support [XHR](https://developer.mozilla.org/en/XmlHttpRequest) file uploads, which allows advanced usage of the file upload [[API]]:

### Desktop browsers

* Google Chrome
* Apple Safari 5.0+
* Mozilla Firefox 4.0+
* Opera 12.0+
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+
* Default Browser on Android 3.2+
* Opera Mobile 12.0+

### Iframe Transport
The [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js) is used for other browsers. The Iframe Transport requires a file input selection to upload files.

## Chunked uploads
The following browsers support splitting up file uploads into multiple chunks:

### Desktop browsers

* Google Chrome
* Apple Safari 6.0+
* Mozilla Firefox 4.0+
* Opera 12.0+ (broken, Opera fails to properly upload Blob objects)
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+

## Image Resizing Functionality
The following browsers support client-side image resizing functionality:

### Desktop browsers

* Google Chrome
* Apple Safari 6.0+
* Mozilla Firefox 4.0+
* Opera 12.0+ (broken, Opera fails to properly upload Blob objects)
* Microsoft Internet Explorer 10.0+

### Mobile browsers

* Apple Safari Mobile on iOS 6.0+
* Google Chrome on iOS 6.0+
* Google Chrome on Android 4.0+

## Cross-domain file uploads
All browsers support cross-domain file uploads.  
Either via native [CORS (Cross Origin Resource Sharing)](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) features, or via the [Iframe Transport](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/jquery.iframe-transport.js).  
For more information and the requirements, please see [[Cross-domain-uploads]].

## Setting For Add Watermark
Use these settings.
* search folder files
* add your copyright.png or change name and change name in code 
* open file UploadHandler.php
* serach in file UploadHandler.php
* $new_file_path = $file_path;
* add after $new_file_path = $file_path;

## Code For Add Watermark 
Use these Code.
`$imagen_original = $file_path;
$imagen_logo = imagecreatefrompng("copyright.png");
$ancho_logo = imagesx($imagen_logo); //get width
$alto_logo = imagesy($imagen_logo); // get height
$imagen_dest = imagecreatefromjpeg($imagen_original);
$ancho_dest = imagesx($imagen_dest); //get new image width
$alto_dest = imagesy($imagen_dest); //get new image height
$ancho_muestra = ($ancho_dest - $ancho_logo) - 10; 
$alto_muestra = ($alto_dest - $alto_logo) - 10; 
imagecopyresized($imagen_dest,$imagen_logo,$ancho_muestra,$alto_muestra,0,0,$ancho_logo,$alto_logo,$ancho_logo,$alto_logo);
imagejpeg($imagen_dest,$file_path,100);`

Greetings...
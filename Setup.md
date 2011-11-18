# Setup instructions

## Using jQuery File Upload (UI version) on PHP websites

The provided example implementation works out of the box and only needs one step for you to add it to your PHP based website:

1. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) the plugin archive, extract it and upload the extracted folder (you may rename it) to your server.

Visit the uploaded directory - you should see the same file upload interface as the demo, allowing you to upload files to your website.

If uploading files doesn't work, make sure that the *files* and *thumbnails* directories permissions allow your server write access.

**Note:**  
The provided PHP upload handler allows anyone to upload files. The uploaded files are also accessible for anyone to download.  
The easiest way to add some kind of authentication system is to protect the example directory by adding [.htaccess based password protection](http://httpd.apache.org/docs/2.2/howto/auth.html#gettingitworking).

## Using jQuery File Upload (UI version) with Google App Engine

1. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) the plugin archive, extract it, adjust the App ID inside of *gae/app.yaml* and upload the **gae** folder as your App Engine instance.
2. Upload the jQuery-File-Upload folder (without the gae/php subfolders) to any server, after adjusting the form action target to point to the url of your App Engine instance.

Visit the uploaded directory - you should see the same file upload interface as the demo, allowing you to upload files to your App Engine instance.

## Using jQuery File Upload (UI version) with a custom server-side upload handler

1. Implement a file upload handler on your platform (Ruby, Python, Java, etc.) that handles normal form based file uploads and upload it to your server. See also the Server-side specific tutorials on the [Documentation Homepage](https://github.com/blueimp/jQuery-File-Upload/wiki).
2. [Download](https://github.com/blueimp/jQuery-File-Upload/archives/master) and extract the plugin archive.
3. Edit *index.html* and adjust the *action* attribute of the HTML form element to the URL of your custom file upload handler. Adjust the file input *name* attribute if your upload handler requires another parameter name for the file uploads.
4. Upload the jQuery-File-Upload folder to your website.
5. Extend your custom server-side upload handler to return a [JSON](http://en.wikipedia.org/wiki/JSON) response akin to the following output:

```js
[{"name":"picture1.jpg","size":902604,"url":"\/\/example.org\/files\/picture1.jpg","thumbnail_url":"\/\/example.org\/thumbnails\/picture1.jpg","delete_url":"\/\/example.org\/upload-handler?file=picture1.jpg","delete_type":"DELETE"},{"name":"picture2.jpg","size":841946,"url":"\/\/example.org\/files\/picture2.jpg","thumbnail_url":"\/\/example.org\/thumbnails\/picture2.jpg","delete_url":"\/\/example.org\/upload-handler?file=picture2.jpg","delete_type":"DELETE"}]
```

Note that the response should always be a JSON array even if only one file is uploaded.

Visit the uploaded directory - you should see the same file upload interface as in the demo, allowing you to upload files to your website.

### Content-Type Negotiation
The file upload plugin makes use of an Iframe Transport module for browsers like *Microsoft Internet Explorer* and *Opera*, which do not yet support [XMLHTTPRequest](https://developer.mozilla.org/en/xmlhttprequest) file uploads.  
Iframe based uploads require a [Content-type](http://en.wikipedia.org/wiki/MIME#Content-Type) of *text/plain* or *text/html* for the JSON response - they will show an undesired download dialog if the iframe response is set to *application/json*.

You can make use of the *Accept* header to offer different content types for the file upload response. Here is the (PHP) example code snippet for the *Accept* content-type variation:

```php
<?php
header('Vary: Accept');
if (isset($_SERVER['HTTP_ACCEPT']) &&
    (strpos($_SERVER['HTTP_ACCEPT'], 'application/json') !== false)) {
    header('Content-type: application/json');
} else {
    header('Content-type: text/plain');
}
?>
```

## Using only the basic version of the jQuery File Upload plugin
If you want to build your own interface, please refer to the [[Basic plugin]] guide (minimal setup guide).
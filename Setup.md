# Setup instructions

**Note:**  
Although the demo implementations contained in this repository include source files from remote servers, it is recommended to download all dependencies and upload them to your own server.  
This excludes script files hosted on Google's Content Delivery Network, which is a more reliable source than GitHub demo pages.

## Using jQuery File Upload (UI version) on [PHP](http://www.php.net/) websites

The provided example implementation works out of the box and only needs one step for you to add it to your PHP based website:

1. [Download](https://github.com/blueimp/jQuery-File-Upload/releases) the plugin archive, extract it and upload the extracted folder (you may rename it) to your server.

Visit the uploaded directory - you should see the same file upload interface as the demo, allowing you to upload files to your website.

If uploading files doesn't work, make sure that the *php/files* directory permissions allow your server write access.

**Note:**  
Please keep in mind some [[Security]] considerations when running a file upload handler on your server.

## Using jQuery File Upload (UI version) with [Google App Engine](http://code.google.com/appengine/)

1. [Download](https://github.com/blueimp/jQuery-File-Upload/releases) the plugin archive, extract it and upload the **server/gae-python** or **server/gae-go** folder (depending on which runtime environment you want to use) as your App Engine instance, after editing the *app.yaml* inside if the folder and replacing "jquery-file-upload" with your own App ID.
2. Upload the jQuery-File-Upload folder (without the **server** subfolder) to any server, after adjusting the **url** option in [main.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/main.js#L22) to the url of your App Engine instance.

Visit the uploaded directory - you should see the same file upload interface as the demo, allowing you to upload files to your App Engine instance.

## Using jQuery File Upload (UI version) with [Node.js](http://nodejs.org/)

You can install the sample [Node.js](http://nodejs.org/) upload service on your server via [npm](http://npmjs.org/):

```sh
npm install blueimp-file-upload-node
```

You can start the service by running the following command:

```sh
./node_modules/blueimp-file-upload-node/server.js
```

Next, [download](https://github.com/blueimp/jQuery-File-Upload/releases) the plugin archive, extract it, and adjust the **url** option in [main.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/js/main.js#L22) to the url of your [Node.js](http://nodejs.org/) service (e.g. "http://localhost:8080").  
You can then upload the project folder (without the unnecessary **server** subfolder) to any static file server and use it as interface to your [Node.js](http://nodejs.org/) upload service.

Make sure to have imagemagick CLI tools installed on the server running the node upload service.

## Using jQuery File Upload (UI version) with a custom server-side upload handler

1. Implement a file upload handler on your platform (Ruby, Python, Java, etc.) that handles normal form based file uploads and upload it to your server. See also the Server-side specific tutorials on the [Documentation Homepage](https://github.com/blueimp/jQuery-File-Upload/wiki).
2. [Download](https://github.com/blueimp/jQuery-File-Upload/releases) and extract the plugin archive.
3. Edit *main.js* and adjust the **url** option to the URL of your custom file upload handler. Alternatively you can remove the url option and edit *index.html* and adjust the *action* attribute of the HTML form element to the URL of your custom file upload handler. If your upload handler requires another parameter name for the file uploads than **files[]**, you also have to adjust the file input *name* attribute or set the **paramName** option (see [[Options]] documentation).
4. Upload the jQuery-File-Upload folder to your website.
5. Extend your custom server-side upload handler to return a [JSON](http://en.wikipedia.org/wiki/JSON) response akin to the following output:

```js
{"files": [
  {
    "name": "picture1.jpg",
    "size": 902604,
    "url": "http:\/\/example.org\/files\/picture1.jpg",
    "thumbnailUrl": "http:\/\/example.org\/files\/thumbnail\/picture1.jpg",
    "deleteUrl": "http:\/\/example.org\/files\/picture1.jpg",
    "deleteType": "DELETE"
  },
  {
    "name": "picture2.jpg",
    "size": 841946,
    "url": "http:\/\/example.org\/files\/picture2.jpg",
    "thumbnailUrl": "http:\/\/example.org\/files\/thumbnail\/picture2.jpg",
    "deleteUrl": "http:\/\/example.org\/files\/picture2.jpg",
    "deleteType": "DELETE"
  }
]}
```

To return errors to the UI, just add an error property to the individual file objects:

```js
{"files": [
  {
    "name": "picture1.jpg",
    "size": 902604,
    "error": "Filetype not allowed"
  },
  {
    "name": "picture2.jpg",
    "size": 841946,
    "error": "Filetype not allowed"
  }
]}
```

When removing files using the delete button, the response should be like this:

```js
{"files": [
  {
    "picture1.jpg": true
  },
  {
    "picture2.jpg": true
  }
]}
```
Note that the response should always be a JSON object containing a files array even if only one file is uploaded.

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

Here is a Ruby on Rails example to serve the proper Content-Type:

``` ruby
def update_attachment
  name  = params[:attachment_name]
  style = params[:attachment_style]
  image = params[:user][name]

  raise "No attachment #{name} for User!" unless User.attachment_definitions[name.to_sym]

  current_user.update("#{name}" => image)
  render(json: current_user.to_fileupload(name, style), content_type: request.format)
end
```

Thanks to the `content_type` option of render, the correct header is set for both IE and true browsers.

For the record, here is the `to_fileupload` method:

``` ruby
def to_fileupload(attachment_name, attachment_style)
  {
    files: [
      {
        id:   read_attribute(:id),
        name: read_attribute("#{attachment_name}_file_name"),
        type: read_attribute("#{attachment_name}_content_type"),
        size: read_attribute("#{attachment_name}_file_size"),
        url:  send(attachment_name).url(attachment_style)
      }
    ]
  }
end
```

## Using only the basic version of the jQuery File Upload plugin
If you want to build your own interface, please refer to the [[Basic plugin]] guide (minimal setup guide).
# Using the plugin with Google App Engine's Blobstore

To upload files to the [Blobstore](http://code.google.com/appengine/docs/python/blobstore/) of [Google App Engine](http://code.google.com/appengine/) with the File Upload plugin, you implement a normal form-based file upload on server-side and then adjust it to return a JSON response.

First, you can follow Google's documentation on [Blobstore File Uploads](http://code.google.com/appengine/docs/python/blobstore/overview.html#Uploading_a_Blob). Then, instead of redirecting the user to the uploaded blob, redirect to a page that returns a JSON response with an array of objects with information for each uploaded file (see [[Setup]] guide). The demo does this by appending the file IDs to the redirect url.

Since a new file upload url has to be created for every upload request to the blobstore, you need to create a handler on server-side, which creates these upload urls and returns them as response to a browser GET request:
* xx.py
```py
from google.appengine.ext import blobstore
from google.appengine.ext import webapp
class UploadUrlHandler(webapp.RequestHandler):
    def get(self):
        upload_url = blobstore.create_upload_url('/path/to/upload/handler')
        self.response.headers['Content-Type'] = 'application/json'
        self.response.out.write('"' + upload_url + '"')
```

On client-side, you can override the *add* callback to retrieve the upload url and override the *url* setting, before adding the file to the upload queue:
```js
`$('#fileupload').fileupload({`
`    add: function (e, data) {`
`        var that = this;`
`        $.getJSON('/upload-url-handler.json', function (url) {`
`            data.url = url;`
`            $.blueimpUI.fileupload.prototype`
`                .options.add.call(that, e, data);`
`        });`
`    }`
`});`
```
*$.blueimpUI.fileupload* is the widget class of [jQuery File Upload UI](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js). It extends the basic widget class of [jQuery File Upload](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js).

Documentation on how to create your own widget class based on the File Upload plugin can be found at the [[Plugin extensions]] page.
# Using the plugin with Google App Engine's Blobstore

To upload files to the [Blobstore](http://code.google.com/appengine/docs/python/blobstore/) of [Google App Engine](http://code.google.com/appengine/), a new file upload url has to be created for every upload request.

On server-side you need to create a handler, which creates these upload urls and returns them as response to the browser request:
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
$('#fileupload').fileupload({
    add: function (e, data) {
        var that = this;
        $.getJSON('/upload-url-handler.json', function (url) {
            data.url = url;
            $.blueimpUI.fileupload.prototype
                .options.add.call(that, e, data);
        });
    }
});
```
*$.blueimpUI.fileupload* is the widget class of [jQuery File Upload UI](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js). It extends the basic widget class of [jQuery File Upload](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js).

Documentation on how to create your own widget class based on the File Upload plugin can be found at the [[Plugin extensions]] page.
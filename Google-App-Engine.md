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

On client-side, you can override the *add* callback to retrieve the upload url and override the *url* setting:
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
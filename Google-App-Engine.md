# Using the plugin with Google App Engine's Blobstore

To upload files to the [Blobstore](http://code.google.com/appengine/docs/python/blobstore/) of [Google App Engine](http://code.google.com/appengine/), a new file upload url has to be created for every upload request.

On server-side you need to create a handler, which creates these upload urls and returns them as response to the browser request:
```py
from google.appengine.ext import blobstore
from google.appengine.ext import webapp
class UploadUrlHandler(webapp.RequestHandler):
    def get(self):
        upload_url = blobstore.create_upload_url('/path/to/upload/handler')
        self.response.out.write(upload_url)
```

On client-side, you can make use of the *beforeSend* option to retrieve the upload url and override the *url* setting:
```js
$('#file_upload').fileUploadUI({
    uploadTable: $('#files'),
    downloadTable: $('#files'),
    buildUploadRow: function (files, index) {/* ... */},
    buildDownloadRow: function (file) {/* ... */},
    beforeSend: function (event, files, index, xhr, handler, callBack) {
        $.get('/upload-url-handler', function (data) {
            handler.url = data;
            callBack();
        });
    }
});
```
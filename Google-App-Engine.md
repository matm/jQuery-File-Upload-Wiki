Two complete server-side Google App Engine implementation examples can be found in the *gae-python* and *gae-go* directories of the source code tree:
https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-python
https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-go

Those versions are the same that have been (Python) or are (Go) running as the demo implementation.
However, since the demo is relying on Cross-domain communication (from Github Pages to App Engine), it has to write Files to the Blobstore programmatically, instead of using the direct upload feature.
Uploading directly to the Blobstore improves upload performance and allows you to upload very large file sizes, but doesn't allow you to set any custom headers required for Cross-Domain access.
You can find more information regarding this issue at the end of this howto.

# Using the plugin with Google App Engine's Blobstore

To upload files to the [Blobstore](http://code.google.com/appengine/docs/python/blobstore/) of [Google App Engine](http://code.google.com/appengine/) with the File Upload plugin, you implement a normal form-based file upload on server-side and then adjust it to return a JSON response.

First, you can follow Google's documentation on [Blobstore File Uploads](http://code.google.com/appengine/docs/python/blobstore/overview.html#Uploading_a_Blob). Then, instead of redirecting the user to the uploaded blob, redirect to a page that returns a JSON response with an array of objects with information for each uploaded file (see [[Setup]] guide).

Since a new file upload url has to be created for every upload request to the blobstore, you need to create a handler on server-side, which creates these upload urls and returns them as response to a browser GET request:

```py
from google.appengine.ext import blobstore
from google.appengine.ext import webapp
class UploadUrlHandler(webapp.RequestHandler):
    def get(self):
        upload_url = blobstore.create_upload_url('/path/to/upload/handler')
        self.response.headers['Content-Type'] = 'application/json'
        self.response.out.write('"' + upload_url + '"')
```

On client-side, you can make use of the *submit* callback to retrieve the upload url and override the *url* setting, before sending the file to the upload handler:

```js
$('#fileupload').fileupload({
    submit: function (e, data) {
        var $this = $(this);
        $.getJSON('/upload-url-handler.json', function (result) {
            data.url = result;
            $this.fileupload('send', data);
        });
        return false;
    } 
});
```

See also how to [[submit files asynchronously]] and the [[Plugin extensions]] documentation.

# App Engine Blobstore and Cross-domain uploads
Since I've rewritten the demo to be hosted on Github pages while the uploads are still done to App Engine, I've had to tackle the Cross-Domain Blobstore issue.

First, you might want to star this issue, so the App Engine developers might give us the possibility to add custom headers to Blobstore upload urls:
http://code.google.com/p/googleappengine/issues/detail?id=5059
Though I think this might be more complicated, as the upload urls would also have to respond to preflighted OPTIONS requests.

The current implementation of the demo (which can be found in the project source code in the gae folder) accepts File uploads with a custom File Upload handler and writes the files to the blobstore programmatically:
http://code.google.com/appengine/docs/python/blobstore/overview.html#Writing_Files_to_the_Blobstore
Unfortunately, this is not the most performant solution - file uploads will be limited by the max request size and memory consumption limits of the App Engine instance.

Therefore I've also followed the [postMessage](https://developer.mozilla.org/en/DOM/window.postMessage) approach and also added support for it to the latest version of the plugin.
To use postMessage transport file uploads, add the plugin to the script section of your HTML page and initialize the fileupload plugin with the postMessage option, which must point to the URL of the postmessage.html file.
postMessage file uploads are currently only supported by Google Chrome and Firefox (8.0+), while Firefox doesn't allow cross-domain postMessage transports with File, Blob or FileList objects (see this issue: https://bugzilla.mozilla.org/show_bug.cgi?id=673742 ).
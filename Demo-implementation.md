**The** [jQuery File Upload Demo](http://blueimp.github.com/jQuery-File-Upload/) is hosted on GitHub, but the files are uploaded to [Google App Engine](http://code.google.com/appengine/).

The uploaded files are publicly accessible, but without a public file listing nobody will see your test uploads, if you don't forward someone the rather cryptic download links.  
Every file will be automatically deleted after 5 minutes, but might be longer accessible due to caching.

You can find the complete source code for the client ([GitHub Pages](http://pages.github.com/)) and server-side ([App Engine](http://code.google.com/appengine/)) implementations in the [master](https://github.com/blueimp/jQuery-File-Upload/tree/master) branch of this project (which is in sync with the [gh-pages](https://github.com/blueimp/jQuery-File-Upload/tree/gh-pages) branch).  
The App Engine source code is located in the [server/gae-python](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-python) and [server/gae-go](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/gae-go) folders.  
Originally, the demo had been built for the [Python runtime of Google App Engine](http://code.google.com/appengine/docs/python/), but it's since been rebuilt for the [Go Runtime Environment](http://code.google.com/appengine/docs/go/).

You can also find an example PHP based server-side implementation under the [server/php](https://github.com/blueimp/jQuery-File-Upload/tree/master/server/php) folder that can be run on any typical PHP webhosting platform.

All implementations can be run in [cross-site](https://github.com/blueimp/jQuery-File-Upload/wiki/Cross-domain-uploads) scenarios, which means the HTML pages can be hosted on a different domain than the upload handlers.  
e.g. the App Engine demo runs on *jquery-file-upload.appspot.com*, while the upload form is hosted on *blueimp.github.com*.

All implementations support every available File Upload plugin option (see [[Options]] documentation), with the exception of non-multipart and chunked uploads, which have not been implemented for the Google App Engine versions.
The [jQuery File Upload Demo](http://blueimp.github.com/jQuery-File-Upload/) is hosted on GitHub, but the files are uploaded to [Google App Engine](http://code.google.com/appengine/).

The uploaded files are publicly accessible, but without a public file listing nobody will see your test uploads, if you don't forward someone the rather cryptic download links.  
Every file will be automatically deleted after 5 minutes, but might be longer accessible due to caching.

You can find the complete source code for the client ([GitHub Pages](http://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0CCUQFjAA&url=http%3A%2F%2Fpages.github.com%2F&ei=VjPMTs7vGqX6mAW37J3aDQ&usg=AFQjCNHt2CUmUiQf3LKcuMYcV1wShuC0Pg&sig2=nVtYFr5Xx95qPL6dq0Pu6w)) and server-side ([App Engine](http://code.google.com/appengine/)) implementations in the [master](https://github.com/blueimp/jQuery-File-Upload/tree/master) and [gh-pages](https://github.com/blueimp/jQuery-File-Upload/tree/gh-pages) branches of this project.  
The App Engine source code is located in the [gae-python](https://github.com/blueimp/jQuery-File-Upload/tree/master/gae-python) and [gae-go](https://github.com/blueimp/jQuery-File-Upload/tree/master/gae-go) folders.  
Originally, the demo had been built for the [Python runtime of Google App Engine](http://code.google.com/appengine/docs/python/), but it's since been rebuilt for the [Go Runtime Environment](http://code.google.com/appengine/docs/go/).

You can also find an example PHP based server-side implementation under the [php](https://github.com/blueimp/jQuery-File-Upload/tree/master/php) folder that can be run on any typical PHP webhosting platform.

All implementations can be run in [cross-site](https://github.com/blueimp/jQuery-File-Upload/wiki/Cross-domain-uploads) scenarios, which means the HTML pages can be hosted on a different domain than the upload handlers.  
e.g. the App Engine demo runs on *jquery-file-upload.appspot.com*, while the upload form is hosted on *blueimp.github.com*.

All implementations support every available File Upload plugin option (see [[Options]] documentation), with the exception of non-multipart and chunked uploads, which have not been implemented for the Google App Engine versions yet.
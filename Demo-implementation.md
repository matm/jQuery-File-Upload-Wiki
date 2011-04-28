The [jQuery File Upload Demo](http://aquantum-demo.appspot.com/file-upload) is always equipped with the latest version of the source files ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js) and [jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).

In addition to the [jQuery](http://jquery.com/) and [jQuery UI](http://jqueryui.com/) libraries and the files distributed with the [jQuery File Upload Plugin](https://github.com/blueimp/jQuery-File-Upload), the demo utilizes the [jQuery Templates Plugin](https://github.com/jquery/jquery-tmpl), a simple [jQuery Cookie Plugin](https://github.com/blueimp/jQuery-Cookie) and a JavaScript file with localization strings.  
For [[Performance Optimizations]], the JavaScript files are merged and minified into one minified JavaScript file. 

Un-minified versions of the demo JavaScript files can be found here:

* [[http://aquantum-demo.appspot.com/static/js/jquery.cookie.js]]
* [[http://aquantum-demo.appspot.com/static/js/jquery.fileupload.js]]
* [[http://aquantum-demo.appspot.com/static/js/jquery.fileupload-ui.js]]
* [[http://aquantum-demo.appspot.com/static/js/jquery.tmpl.js]]
* [[http://aquantum-demo.appspot.com/static/js/localization/en.js]]
* [[http://aquantum-demo.appspot.com/static/js/application.js]]

Note that these files are sent with a [far future Expires header](http://developer.yahoo.com/performance/rules.html#expires), so you might need to refresh your browser cache to retrieve the latest versions (the demo itself uses IDs appended as query parameter to the files to make sure browsers get the latest versions).

The server-side of the demo is written in Python on [Google App Engine](http://code.google.com/appengine/).  
With App Engine, adding a thumbnail picture for uploaded files is very easy thanks to the [get_serving_url](http://code.google.com/appengine/docs/python/images/functions.html#Image_get_serving_url) method of the [Images API](http://code.google.com/appengine/docs/python/images/). You basically just create a special link to the uploaded file with the thumbnail size as parameter.

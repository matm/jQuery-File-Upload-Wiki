

Note that these files are sent with a [far future Expires header](http://developer.yahoo.com/performance/rules.html#expires), so you might need to refresh your browser cache to retrieve the latest versions (the demo itself uses IDs appended as query parameter to the files to make sure browsers get the latest versions).

The server-side of the demo is written in Python on [Google App Engine](http://code.google.com/appengine/).  
With App Engine, adding a thumbnail picture for uploaded files is very easy thanks to the [get_serving_url](http://code.google.com/appengine/docs/python/images/functions.html#Image_get_serving_url) method of the [Images API](http://code.google.com/appengine/docs/python/images/). You basically just create a special link to the uploaded file with the thumbnail size as parameter.

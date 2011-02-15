The [jQuery File Upload Demo](http://aquantum-demo.appspot.com/file-upload) is always equipped with the latest version of the source files ([jquery.fileupload.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload.js) and [jquery.fileupload-ui.js](https://github.com/blueimp/jQuery-File-Upload/blob/master/jquery.fileupload-ui.js)).

In addition to the the [jQuery](http://jquery.com/) and [jQuery UI](http://jqueryui.com/) libraries and the files distributed with the [jQuery File Upload Plugin](https://github.com/blueimp/jQuery-File-Upload), the demo utilizes the [jQuery Templates Plugin](https://github.com/jquery/jquery-tmpl) and a simple [jQuery Cookie Plugin](https://github.com/blueimp/jQuery-Cookie).
For [[Performance Optimizations]], the JavaScript files are merged and minified. 

An un-minified version of the demo JavaScript application can be found here:  
[[http://aquantum-demo.appspot.com/static/js/application.js]]  
Note that this file is sent with a [far future Expires header](http://developer.yahoo.com/performance/rules.html#expires), so you might need to refresh your browser cache to retrieve the latest version (the demo itself uses IDs appended as query parameter to the file to make sure browsers get the latest version).

The most reusable addition of the demo implementation is a TemplateHelper function used in conjunction with the [jQuery Templates Plugin](https://github.com/jquery/jquery-tmpl):
```js
var TemplateHelper = function (locale, settings) {
    var roundDecimal = function (num, dec) {
            return Math.round(num * Math.pow(10, dec)) / Math.pow(10, dec);
        };
    this.locale = locale;
    this.settings = settings;
    this.formatFileSize = function (bytes) {
        if (isNaN(bytes) || bytes === null) {
            return '';
        }
        if (bytes >= 1000000000) {
            return roundDecimal(bytes / 1000000000, 2) + ' GB';
        }
        if (bytes >= 1000000) {
            return roundDecimal(bytes / 1000000, 2) + ' MB';
        }
        return roundDecimal(bytes / 1000, 2) + ' KB';
    };
    this.formatFileName = function (fileName) {
        // Remove any path information:
        return fileName.replace(/^.*[\/\\]/, '');
    };
}
```

The server-side of the demo is written in Python on [Google App Engine](http://code.google.com/appengine/).  
With App Engine, adding a thumbnail picture for uploaded files is very easy thanks to the [get_serving_url](http://code.google.com/appengine/docs/python/images/functions.html#Image_get_serving_url) method of the [Images API](http://code.google.com/appengine/docs/python/images/). You basically just create a special link to the uploaded file with the thumbnail size as parameter.

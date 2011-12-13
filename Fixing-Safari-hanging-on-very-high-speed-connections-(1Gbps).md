We're using your plugin to support multiple file uploads, but when I was testing it using my ethernet connection, which gives me about 1gbit of upload throughput to our datacenter, Safari kept hanging, especially when uploading very large files (and yes, we will have internal users with the same ballpark of upload throughput, I'm not being picky, hehe). 

I discovered the cause for the problem: the xhr.upload.onprogress callback was firing too often, making the browser unusable while a big upload was running. 

I fixed it by changing this: 

```javascript
        _initProgressListener: function (options) {
            var that = this,
                xhr = options.xhr ? options.xhr() : $.ajaxSettings.xhr();
            // Accesss to the native XHR object is required to add event listeners
            // for the upload progress event:
            
            if (xhr.upload && xhr.upload.addEventListener) {
                xhr.upload.addEventListener('progress', function (e) {
                    that._onProgress(e, options);
                }, false);
                options.xhr = function () {
                    return xhr;
                };
            }
        },
```

into 

```javascript
        _initProgressListener: function (options) {
            var that = this,
                xhr = options.xhr ? options.xhr() : $.ajaxSettings.xhr();
            // Accesss to the native XHR object is required to add event listeners
            // for the upload progress event:
            if (xhr.upload && xhr.upload.addEventListener) {
                xhr.upload.addEventListener('progress',
                    jQuery.throttle(100, function (e) {
                        that._onProgress(e, options);
                    }), 
                    false);
                options.xhr = function () {
                    return xhr;
                };
            }
        },
```

It uses this plugin to throttle the number of times that _onProgress is called: http://benalman.com/projects/jquery-throttle-debounce-plugin/

I hope this is useful :)
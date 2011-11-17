The jQuery File Upload plugin triggers callbacks for progress events, that can be used to display extended progress information, including:

* Upload speed (formatted as GBps/MBps/KBps/Bps)
* Remaining upload time (formatted as hh:mm:ss)
* Uploaded and total bytes (formatted as GB/MB/KB/B)
* Percentage of uploaded bytes

The following code snippet displays all this information inside of a specific HTML element.
It works with both the basic file upload plugin and the UI version.  
Just add the following element to your HTML page:

```
<div id="fileupload-progress"></div>
```

Then add the following code to your JavaScript file:

```js
$('#fileupload').bind('fileuploadstart', function () {
    var widget = $(this),
        progressElement = $('#fileupload-progress').fadeIn(),
        interval = 500,
        total = 0,
        loaded = 0,
        loadedBefore = 0,
        progressTimer,
        progressHandler = function (e, data) {
            loaded = data.loaded;
            total = data.total;
        },
        stopHandler = function () {
            widget
                .unbind('fileuploadprogressall', progressHandler)
                .unbind('fileuploadstop', stopHandler);
            window.clearInterval(progressTimer);
            progressElement.fadeOut(function () {
                progressElement.html('');
            });
        },
        formatTime = function (seconds) {
            var date = new Date(seconds * 1000);
            return ('0' + date.getUTCHours()).slice(-2) + ':' +
                ('0' + date.getUTCMinutes()).slice(-2) + ':' +
                ('0' + date.getUTCSeconds()).slice(-2);
        },
        formatBytes = function (bytes) {
            if (bytes >= 1000000000) {
                return (bytes / 1000000000).toFixed(2) + ' GB';
            }
            if (bytes >= 1000000) {
                return (bytes / 1000000).toFixed(2) + ' MB';
            }
            if (bytes >= 1000) {
                return (bytes / 1000).toFixed(2) + ' KB';
            }
            return bytes + ' B';
        },
        formatPercentage = function (floatValue) {
            return (floatValue * 100).toFixed(2) + ' %';
        },
        updateProgressElement = function (loaded, total, bps) {
            progressElement.html(
                formatBytes(bps) + 'ps | ' +
                    formatTime((total - loaded) / bps) + ' | ' +
                    formatPercentage(loaded / total) + ' | ' +
                    formatBytes(loaded) + ' / ' + formatBytes(total)
            );
        },
        intervalHandler = function () {
            var diff = loaded - loadedBefore;
            if (!diff) {
                return;
            }
            loadedBefore = loaded;
            updateProgressElement(
                loaded,
                total,
                diff * (1000 / interval)
            );
        };
    widget
        .bind('fileuploadprogressall', progressHandler)
        .bind('fileuploadstop', stopHandler);
    progressTimer = window.setInterval(intervalHandler, interval);
});
```
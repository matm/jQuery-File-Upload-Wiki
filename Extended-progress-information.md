The jQuery File Upload plugin triggers callbacks for progress events, that can be used to display extended progress information, including:

* Upload speed (formatted as GBps/MBps/KBps/Bps).
* Remaining upload time (formatted as hh:mm:ss).
* Uploaded and total bytes (formatted as bit/s, kbit/s, Mbit/s, Gbit/s).
* Percentage of uploaded bytes.

The UI version of the plugin displays this extended progress information.  
However, it's also possible to access the progress bitrate with the basic plugin, e.g. the following way:

```js
$('#fileupload').bind('fileuploadprogress', function (e, data) {
    // Log the current bitrate for this upload:
    console.log(data.bitrate);
});
```
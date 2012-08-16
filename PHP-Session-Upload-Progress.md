PHP 5.4 will likely introduce built-in support for server-side upload progress status:  
http://php.net/manual/en/session.upload-progress.php

The following code snippets outline how to use this feature with the File Upload plugin:

## JavaScript (application.js)

```js
// This code assumes each file upload has a related DOM node
// set as data.context, which is true for the UI version:
$('#fileupload').bind('fileuploadsend', function (e, data) {
    // This feature is only useful for browsers which rely on the iframe transport:
    if (data.dataType.substr(0, 6) === 'iframe') {
        // Set PHP's session.upload_progress.name value:
        var progressObj = {
            name: 'PHP_SESSION_UPLOAD_PROGRESS',
            value: (new Date()).getTime()  // pseudo unique ID
        };
        data.formData.push(progressObj);
        // Start the progress polling:
        data.context.data('interval', setInterval(function () {
            $.get('progress.php', $.param([progressObj]), function (result) {
                // Trigger a fileupload progress event,
                // using the result as progress data:
                e = document.createEvent('Event');
                e.initEvent('progress', false, true);
                $.extend(e, result);
                $('#fileupload').data('fileupload')._onProgress(e, data);
            }, 'json');
        }, 1000)); // poll every second
    }
}).bind('fileuploadalways', function (e, data) {
    clearInterval(data.context.data('interval'));
});
```

## PHP (progress.php)

```
<?php
  // Assuming default values for session.upload_progress.prefix
  // and session.upload_progress.name:
  $s = $_SESSION['upload_progress_'.intval($_GET['PHP_SESSION_UPLOAD_PROGRESS'])];
  $progress = array(
    'lengthComputable' => true,
    'loaded' => $s['bytes_processed'],
    'total' => $s['content_length']
  );
  echo json_encode($progress);
?>
```

# How to force sequential uploads for multiple selected files
By default, the plugin uploads multiple files simultaneously.  
However it is possible to force a sequential upload, that is starting the upload of the second selected file after the upload of the first one has completed, starting the upload of the third file after the second file has been uploaded and so on.

The following is an example implementation, making use of the *beforeSend*, *onComplete* and *onAbort* [[Options]] and a JavaScript array with a custom *start* method to sequence the uploads:
```js
/*global $ */
$(function () {
    $('#file_upload').fileUploadUI({
        uploadTable: $('#files'),
        downloadTable: $('#files'),
        buildUploadRow: function (files, index) {
            return $('<tr><td>' + files[index].name + '<\/td>' +
                    '<td class="file_upload_progress"><div><\/div><\/td>' +
                    '<td class="file_upload_cancel">' +
                    '<button class="ui-state-default ui-corner-all" title="Cancel">' +
                    '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
                    '<\/button><\/td><\/tr>');
        },
        buildDownloadRow: function (file) {
            return $('<tr><td>' + file.name + '<\/td><\/tr>');
        },
        beforeSend: function (event, files, index, xhr, handler, callBack) {
            if (index === 0) {
                // The files array is a shared object between the instances of an upload selection.
                // We extend it with a custom array to coordinate the upload sequence:
                files.uploadSequence = [];
                files.uploadSequence.start = function (index) {
                    var next = this[index];
                    if (next) {
                        // Call the callback with any given additional arguments:
                        next.apply(null, Array.prototype.slice.call(arguments, 1));
                        this[index] = null;
                    }
                };
            }
            files.uploadSequence.push(callBack);
            if (index + 1 === files.length) {
                files.uploadSequence.start(0);
            }
        },
        onComplete: function (event, files, index, xhr, handler) {
            files.uploadSequence.start(index + 1);
        },
        onAbort: function (event, files, index, xhr, handler) {
            handler.removeNode(handler.uploadRow);
            files.uploadSequence[index] = null;
            files.uploadSequence.start(index + 1);
        }
    });
});
```

## How to pass parameters to the next upload handler in the sequence:
If you want to adjust the settings of the next upload depending on the response from the previous one, you can make the following adjustments:  
Change the line ```files.uploadSequence.push(callBack);``` to
```js
files.uploadSequence.push(function (setting1, setting2, ...) {
    handler.setting1 = setting1;
    handler.setting2 = setting2;
    ...
    callBack();
});
```

Next change the lines starting the next sequence with ```files.uploadSequence.start(index + 1);``` and add the desired parameters:
```js
files.uploadSequence.start(index + 1, setting1, setting2, ...);
```

## How to start the sequential uploads with a button click
Inside of the *beforeSend* method, replace
```js
files.uploadSequence.start(0);
```

with
```js
$('#start_uploads').click(function () {
    files.uploadSequence.start(0);
});
```

Then add a button to your upload form:
```html
<button id="start_uploads">Start uploads</button>
```

See also [[How to queue files and start uploads with a button click]].
```js
$.blueimp.fileupload.prototype.options.processQueue.push(
    {
        action: 'catTest'
    }
);

$.widget('blueimp.fileupload', $.blueimp.fileupload, {
    processActions: {
        catTest: function (data) {
            var dfd = $.Deferred(),
                file = data.files[data.index];
            if (file.name.indexOf('cat') !== -1) {
                dfd.resolveWith(this, [data]);
            } else {
                file.error = 'Not a cat picture';
                data.files.error = true;
                dfd.rejectWith(this, [data]);
            }
            return dfd.promise();
        }
    }
});

$('#fileupload')
    .on('fileuploadprocessstart', function () {
        console.log('processstart');
    })
    .on('fileuploadprocess', function (e, data) {
        console.log('process', data.files[data.index].name);
    })
    .on('fileuploadprocessfail', function (e, data) {
        console.log('processfail', data.files[data.index].name);
    })
    .on('fileuploadprocessdone', function (e, data) {
        console.log('processdone', data.files[data.index].name);
    })
    .on('fileuploadprocessstop', function () {
        console.log('processstop');
    });
```
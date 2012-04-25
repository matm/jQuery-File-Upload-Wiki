```js
$.blueimpFP.fileupload.prototype.processActions.duplicate = function (data, options) {
    data.files.push(data.files[data.index]);
    return data;
};
$('#fileupload').fileupload({
    process: [
        {
            action: 'load',
            fileTypes: /^image\/(gif|jpeg|png)$/,
            maxFileSize: 20000000 // 20MB
        },
        {
            action: 'resize',
            maxWidth: 1920,
            maxHeight: 1200
        },
        {action: 'save'},
        {action: 'duplicate'},
        {
            action: 'resize',
            maxWidth: 1280,
            maxHeight: 1024
        },
        {action: 'save'},
        {action: 'duplicate'},
        {
            action: 'resize',
            maxWidth: 1024,
            maxHeight: 768
        },
        {action: 'save'}
    ]
});
```
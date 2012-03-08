```js
$('#fileupload').fileupload({
    resizeOptions: [
        {resizeMaxWidth: 1280},
        {resizeMaxWidth: 1024},
        {resizeMaxWidth: 800}
    ],
    submit: function (e, data) {
        var $this = $(this),
            originalFile = data.files[0],
            filesToSubmit = [],
            deferred = $.Deferred(),
            promise = deferred.promise();
        $.each(
            $this.fileupload('option', 'resizeOptions') || [],
            function (index, settings) {
                promise = promise.pipe(function () {
                    filesToSubmit.push(data.files[0]);
                    data.files = [originalFile];
                    $.extend(data, settings);
                    return $this.fileupload('resize', data);
                });
            }
        );
        promise.done(function () {
            filesToSubmit.push(data.files[0]);
            data.files = filesToSubmit;
            $this.fileupload('send', data);
        });
        deferred.resolve();
        return false;
    } 
});
```
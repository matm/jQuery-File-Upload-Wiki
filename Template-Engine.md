jQuery File Upload makes use of the [JavaScript Templates](https://github.com/blueimp/JavaScript-Templates) engine to render the list of files.
You can find documentation for this template engine on the [project page](https://github.com/blueimp/JavaScript-Templates).

However, you are not required to use this specific template engine and can replace it with a different implementation. The following example makes use of [[Plugin extensions]] to override the **_initTemplates** and **_renderTemplate** methods to provide a simple template mechanism that doesn't require any additional plugin:

```js
$.widget('blueimpUIX.fileupload', $.blueimpUI.fileupload, {

    _renderTemplate: function (func, files) {
        return func({
            files: files,
            formatFileSize: this._formatFileSize,
            options: this.options
        });
    },

    _initTemplates: function () {
        this.options.uploadTemplate = function (o) {
            var rows = $();
            $.each(o.files, function (index, file) {
                var row = $('<tr class="template-upload">' +
                    '<td class="preview"><span class="fade"></span></td>' +
                    '<td class="name"></td>' +
                    '<td class="size"></td>' +
                    (file.error ? '<td class="error" colspan="2"></td>' :
                            '<td class="progress"><div class="progressbar">' +
                                '<div style="width:0%;"></div></div></td>' +
                                '<td class="start"><button class="btn primary">Start</button></td>'
                    ) + '<td class="cancel"><button class="btn info">Cancel</button></td></tr>');
                row.find('.name').text(file.name);
                row.find('.size').text(o.formatFileSize(file.size));
                if (file.error) {
                    row.addClass('ui-state-error');
                    row.find('.error').text(
                        fileUploadErrors[file.error] || file.error
                    );
                }
                rows = rows.add(row);
            });
            return rows;
        };
        this.options.downloadTemplate = function (o) {
            var rows = $();
            $.each(o.files, function (index, file) {
                var row = $('<tr class="template-download">' +
                    (file.error ? '<td></td><td class="name"></td>' +
                        '<td class="size"></td><td class="error" colspan="2"></td>' :
                            '<td class="preview"></td>' +
                                '<td class="name"><a></a></td>' +
                                '<td class="size"></td><td colspan="2"></td>'
                    ) + '<td class="delete"><button class="btn danger">Delete</button> ' +
                        '<input type="checkbox" name="delete" value="1"></td></tr>');
                row.find('.size').text(o.formatFileSize(file.size));
                if (file.error) {
                    row.find('.name').text(file.name);
                    row.addClass('ui-state-error');
                    row.find('.error').text(
                        fileUploadErrors[file.error] || file.error
                    );
                } else {
                    row.find('.name a').text(file.name);
                    if (file.thumbnail_url) {
                        row.find('.preview').append('<a><img></a>')
                            .find('img').prop('src', file.thumbnail_url);
                        row.find('a').prop('rel', 'gallery');
                    }
                    row.find('a').prop('href', file.url);
                    row.find('.delete button')
                        .attr('data-type', file.delete_type)
                        .attr('data-url', file.delete_url);
                }
                rows = rows.add(row);
            });
            return rows;
        };
    }

});
```

**Notes:**

* This code snippet assumes a global variable **fileUploadErrors** with a map of error codes to error messages. In a real application, this information would come from your localization system.
* The **_renderTemplate** is supposed to return a jQuery collection object of the rendered upload/download template.
* The **data** parameter passed via **add** callback is stored via [jQuery.data](http://api.jquery.com/data/) on this jQuery collection object and later reused when submitting or canceling the file upload.
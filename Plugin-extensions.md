The recommended way to extend the jQuery File Upload plugin is by using the extension mechanism of the [jQuery UI Widget Factory](http://docs.jquery.com/UI_Developer_Guide#The_widget_factory).

## Example on how to override callbacks and add additional options
The following is an example on how to create a custom File Upload widget that adjusts the deletion URL to include a request authenticity token to protect against [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks by overriding the *destroy* callback.  
It is a simplified version of the plugin extension that is used for the demo (see [[Demo Implementation]]).

```js
$.widget('blueimpUIX.fileupload', $.blueimpUI.fileupload, {

    options: {
        authenticityTokenName: 'request_authenticity_token',
        destroy: function (e, data) {
            var fu = $(this).data('fileupload');
            data.url = data.url &&
                fu._addUrlParams(data.url, fu._getAuthenticityToken());
            $.blueimpUI.fileupload.prototype
                .options.destroy.call(this, e, data);
        }
    },
    
    _addUrlParams: function (url, data) {
        return url + (/\?/.test(url) ? '&' : '?') + $.param(data);
    },
    
    _getAuthenticityToken: function () {
        var name = this.options.authenticityTokenName,
            parts = $.cookie(name).split('|'),
            obj = {};
        obj[name] = parts[0];
        return obj;
    }

});
```

## Example on how to override existing plugin methods
The following example overrides the *_renderUploadTemplate* and *_renderDownloadTemplate* methods and provides a simple template mechanism that doesn't require the jQuery Templates plugin:
```js
$.widget('blueimpUIX.fileupload', $.blueimpUI.fileupload, {

    options: {
        errorMessages: {
            maxFileSize: 'File is too big',
            minFileSize: 'File is too small',
            acceptFileTypes: 'Filetype not allowed',
            maxNumberOfFiles: 'Max number of files exceeded'
        }
    },

    _renderUploadTemplate: function (files) {
        var that = this,
            rows = $();
        $.each(files, function (index, file) {
            file = that._uploadTemplateHelper(file);
            var row = $('<tr class="template-upload">' + 
                '<td class="preview"></td>' +
                '<td class="name"></td>' +
                '<td class="size"></td>' +
                (file.error ?
                    '<td class="error" colspan="2"></td>'
                :
                    '<td class="progress"><div></div></td>' +
                    '<td class="start"><button>Start</button></td>'
                ) + 
                '<td class="cancel"><button>Cancel</button></td>' +
                '</tr>');
            row.find('.name').text(file.name);
            row.find('.size').text(file.sizef);
            if (file.error) {
                row.addClass('ui-state-error');
                row.find('.error').text(
                    that.options.errorMessages[file.error] || file.error
                );
            }
            rows = rows.add(row);
        });
        return rows;
    },

    _renderDownloadTemplate: function (files) {
        var that = this,
            rows = $();
        $.each(files, function (index, file) {
            file = that._downloadTemplateHelper(file);
            var row = $('<tr class="template-download">' + 
                (file.error ?
                    '<td></td>' +
                    '<td class="name"></td>' +
                    '<td class="size"></td>' +
                    '<td class="error" colspan="2"></td>'
                :
                    '<td class="preview"></td>' +
                    '<td class="name"><a></a></td>' +
                    '<td class="size"></td>' +
                    '<td colspan="2"></td>'
                ) + 
                '<td class="delete"><button>Delete</button></td>' +
                '</tr>');
            row.find('.size').text(file.sizef);
            if (file.error) {
                row.find('.name').text(file.name);
                row.addClass('ui-state-error');
                row.find('.error').text(
                    that.options.errorMessages[file.error] || file.error
                );
            } else {
                row.find('.name a').text(file.name);
                if (file.thumbnail_url) {
                    row.find('.preview').append('<a><img></a>')
                        .find('img').prop('src', file.thumbnail_url);
                    row.find('a').prop('target', '_blank');
                }
                row.find('a').prop('href', file.url);
                row.find('.delete button')
                    .attr('data-type', file.delete_type)
                    .attr('data-url', file.delete_url);
            }
            rows = rows.add(row);
        });
        return rows;
    },

});
```
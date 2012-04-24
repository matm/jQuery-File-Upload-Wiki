The following is an example on how to create a custom File Upload widget that adjusts the deletion URL to include a request authenticity token to protect against CSRF attacks by overriding the destroy callback.
It is a simplified version of the plugin extension that is used for the demo (see Demo Implementation).
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

Example on how to override existing plugin methods

See Template Engine.
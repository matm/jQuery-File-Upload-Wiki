The recommended way to extend the jQuery File Upload plugin is by using the extension mechanism of the [jQuery UI Widget Factory](http://api.jqueryui.com/jQuery.widget/).  
This allows to override default [[Options]] (including callback methods) as well as methods of the File Upload widget class.

## Example on how to override callbacks and add additional options
The following is an example on how to create a custom File Upload widget that adjusts the deletion URL to include a request authenticity token to protect against [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery) attacks by overriding the *destroy* callback.  
It is a simplified version of the plugin extension that is used for the demo (see [[Demo Implementation]]).

```js
$.widget('blueimp.fileupload', $.blueimp.fileupload, {

    options: {
        authenticityTokenName: 'request_authenticity_token',
        destroy: function (e, data) {
            // jQuery Widget Factory uses "namespace-widgetname" since version 1.10.0:
            var fu = $(this).data('blueimp-fileupload') || $(this).data('fileupload');
            data.url = data.url &&
                fu._addUrlParams(data.url, fu._getAuthenticityToken());
            $.blueimp.fileupload.prototype
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
See [[Template Engine]].
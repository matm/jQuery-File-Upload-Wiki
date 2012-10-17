# How to submit files asynchronously
Sometimes you will want to retrieve extra parameters asynchronously via AJAX request before submitting files to the upload server.

## Overriding the add callback
One way to do this is by overriding the default **add** callback of the basic file upload plugin and calling the **submit()** method on the **data** parameter after retrieving your parameters and setting them as formData object:

```js
$('#fileupload').fileupload({
    add: function (e, data) {
        $.getJSON('/example/url', function (result) {
            data.formData = result; // e.g. {id: 123}
            data.submit();
        });
    } 
});
```

The UI version of the File Upload plugin uses this approach to render the upload template and invokes **data.submit()** after clicking the start button.  
If you want to override the **add** callback, but still want to keep the features of the UI version of the **add** callback, you can call it the following way, using the prototype property of the UI widget:

```js
$('#fileupload').fileupload({
    add: function (e, data) {
        $.getJSON('/example/url', function (result) {
            data.formData = result; // e.g. {id: 123}
            $.blueimp.fileupload.prototype
                .options.add.call(this, e, data);
        });
    } 
});
```

However, this also means that the upload template is not rendered until your AJAX call returns.

## Using the submit callback option
Another approach is to make use of the submit callback option. This approach allows you to render the upload template and only retrieve your additional parameters when the file upload is actually submitted:

```js
$('#fileupload').fileupload({
    submit: function (e, data) {
        var $this = $(this);
        $.getJSON('/example/url', function (result) {
            data.formData = result; // e.g. {id: 123}
            $this.fileupload('send', data);
        });
        return false;
    } 
});
```

Returning false in the submit callback prevents the upload from being started (see [[Options]] documentation). The upload is started manually after retrieving the additional parameters by making use of the File Upload **send** [[API]] method.
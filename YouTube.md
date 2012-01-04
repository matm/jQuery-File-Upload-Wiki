# Upload to YouTube

YouTube provides a browser upload API that can be used together with the File Upload plugin:
http://code.google.com/apis/youtube/1.0/developers_guide_python.html#BrowserUpload (Python)
http://code.google.com/apis/youtube/1.0/developers_guide_php.html#BrowserUpload (PHP)

The following tutorial makes use of the File Upload **send** [API](https://github.com/blueimp/jQuery-File-Upload/wiki/API) to submit the meta data via a server-side application and then upload the video file itself directly to YouTube:

## Client-side

```js
$('#fileupload').fileupload({
    forceIframeTransport: true,
    submit: function (e, data) {
        var that = $(this);
        // Post the current file meta data to your application,
        // to receive the YouTube upload url and token:
        // http://code.google.com/apis/youtube/1.0/developers_guide_python.html#BrowserUpload
        $.post('/your_application_url', data.files[0], function (result) {
            data.url = result.url;
            data.formData = {token: result.token};
            that.fileupload('send', data);
        });
        return false;
    }
});
```

Since YouTube only allows single file uploads and expects a certain parameter name, the file input field needs to be adjusted as well:

```html
<input type="file" name="file">
```

## Server-side

In *index.html* duplicate the *form* tag with the *id* "fileupload" and change *id* to *class* like this:

```html
<form class="fileupload" action="server/php/" method="POST" enctype="multipart/form-data">
    <!-- ... -->
</form>
<form class="fileupload" action="server/php/" method="POST" enctype="multipart/form-data">
    <!-- ... -->
</form>
```

In *application.js* change

```js
$('#fileupload').fileupload();
```

to

```js
$('.fileupload').each(function () {
    $(this).fileupload({
        dropZone: $(this)
    });
});
```

This will give you two complete independent file upload widgets with their own dropZone.

**Note:**  
If you want to allow specific drop zones but disable the default browser action for file drops on the document, add the following JavaScript code:

```js
$(document).bind('drop dragover', function (e) {
    e.preventDefault();
});
```

Note that preventing the default action for both the "drop" and "dragover" events is required to disable the default browser drop action.

## Individual upload/download templates
To use different upload/download templates for the different fileupload widgets, you can set the following options on widget initialization (see [[Options]] documentation):

* uploadTemplateId
* downloadTemplateId

The easiest way to set these options is to make use of data attributes in the following way:

```html
<form class="fileupload" action="server/php/" method="POST" enctype="multipart/form-data"
    data-upload-template-id="template-upload-1"
    data-download-template-id="template-download-1">
    <!-- ... -->
</form>
<form class="fileupload" action="server/php/" method="POST" enctype="multipart/form-data"
    data-upload-template-id="template-upload-2"
    data-download-template-id="template-download-2">
    <!-- ... -->
</form>
```
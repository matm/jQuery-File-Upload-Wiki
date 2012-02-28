In *index.html* duplicate the *form* tag with the *id* "fileupload" and change *id* to *class* like this:

```html
<form class="fileupload" action="server/php/" method="POST" enctype="multipart/form-data">
    <div class="row">
        <div class="span16 fileupload-buttonbar">
            <div class="progressbar fileupload-progressbar"><div style="width:0%;"></div></div>
            <span class="btn success fileinput-button">
                <span>Add files...</span>
                <input type="file" name="files[]" multiple>
            </span>
            <button type="submit" class="btn primary start">Start upload</button>
            <button type="reset" class="btn info cancel">Cancel upload</button>
            <button type="button" class="btn danger delete">Delete selected</button>
            <input type="checkbox" class="toggle">
        </div>
    </div>
    <br>
    <div class="row">
        <div class="span16">
            <table class="zebra-striped"><tbody class="files"></tbody></table>
        </div>
    </div>
</form>
<form class="fileupload" action="php/index.php" method="POST" enctype="multipart/form-data">
    <div class="row">
        <div class="span16 fileupload-buttonbar">
            <div class="progressbar fileupload-progressbar"><div style="width:0%;"></div></div>
            <span class="btn success fileinput-button">
                <span>Add files...</span>
                <input type="file" name="files[]" multiple>
            </span>
            <button type="submit" class="btn primary start">Start upload</button>
            <button type="reset" class="btn info cancel">Cancel upload</button>
            <button type="button" class="btn danger delete">Delete selected</button>
            <input type="checkbox" class="toggle">
        </div>
    </div>
    <br>
    <div class="row">
        <div class="span16">
            <table class="zebra-striped"><tbody class="files"></tbody></table>
        </div>
    </div>
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
In *index.html* duplicate the *form* tag with the *id* "fileupload" and change *id* to *class* like this:

```html
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
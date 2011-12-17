The plugin can be applied to a form with multiple file input fields out of the box.
The files are sent to the server with the parameter name of the file input field clicked by the user.

The following is a short howto on how to add an additional file input field to the example:

In *index.html* duplicate the *span* tag with the *class* "fileinput-button" like this:

```html
<form id="fileupload" action="php/index.php" method="POST" enctype="multipart/form-data">
    <div class="row">
        <div class="span16 fileupload-buttonbar">
            <div class="progressbar fileupload-progressbar"><div style="width:0%;"></div></div>
            <span class="btn success fileinput-button">
                <span>Add files...</span>
                <input type="file" name="files[]" multiple>
            </span>
            <!-- Extra file input start /-->
            <span class="btn fileinput-button">
                <span>Other...</span>
                <input type="file" name="files2[]" multiple>
            </span>
            <!--/ Extra file input stop -->
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

Adjust the *name* attribute of your file input fields according to what is expected on your server-side handler.
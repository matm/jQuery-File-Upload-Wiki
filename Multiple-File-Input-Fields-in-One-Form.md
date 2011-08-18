The plugin can be applied to a form with multiple file input fields out of the box.
The files are sent to the server with the parameter name of the file input field clicked by the user.

The following is a short howto on how to add an additional file input field to the example:

In *example/index.html* duplicate the *label* with the *class* "fileinput-button" like this:

```html
<div id="fileupload">
    <form action="upload.php" method="POST" enctype="multipart/form-data">
        <div class="fileupload-buttonbar">
            <label class="fileinput-button">
                <span>Add files...</span>
                <input type="file" name="files[]" multiple>
            </label>
            <label class="fileinput-button">
                <span>Add more files...</span>
                <input type="file" name="files2[]" multiple>
            </label>
            <button type="submit" class="start">Start upload</button>
            <button type="reset" class="cancel">Cancel upload</button>
            <button type="button" class="delete">Delete files</button>
        </div>
    </form>
    <div class="fileupload-content">
        <table class="files"></table>
        <div class="fileupload-progressbar"></div>
    </div>
</div>
```

Adjust the *name* attribute of your file input fields according to what is expected on your server-side handler.
## Adding input fields to the upload form
The easiest way to submit additional form data is by adding additional input fields to the upload form:

```html
<form id="fileupload" action="php/index.php" method="POST" enctype="multipart/form-data">
    <!-- additional form data start / -->
    <input type="hidden" name="example1" value="test">
    <div class="row">
        <label>Example: <input type="text" name="example2"></label>
    </div>
    <!-- / additional form data stop -->
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

By default, the plugin calls [jQuery's serializeArray method](http://api.jquery.com/serializeArray) on the upload form to gather additional form data for all input fields (including hidden fields).  
The value of these form fields will be sent to the server along with the selected files.

## Adding additional form data programmatically
The formData option (see [[Options]] documentation) can be used to set additional form data programmatically, e.g.:

```js
$('#fileupload').fileupload({
    formData: {example: 'test'}
});
```

### Setting formData on upload start
A common use case is to set the formData option on upload start.  
This can be easily achieved by binding a callback to the "submit" event:

```js
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    // The example input, doesn't have to be part of the upload form:
    var input = $('#input');
    data.formData = {example: input.val()};
    if (!data.formData.example) {
      input.focus();
      return false;
    }
});
```

**Note:**
If the submit event callback returns false, the upload request will not start.

### Setting formData on upload start for each individual file upload
First, we adjust the upload template and add a new cell with an input field for a file title:  

```html
<script id="template-upload" type="text/html">
{% for (var i=0, files=o.files, l=files.length, file=files[0]; i<l; file=files[++i]) { %}
    <tr class="template-upload fade">
        <td class="preview"><span class="fade"></span></td>
        <td class="name">{%=file.name%}</td>
        <td class="size">{%=o.formatFileSize(file.size)%}</td>
        <!-- additional form data start / -->
        <td class="title"><label>Title: <input name="title[]" required></label></td>
        <!-- / additional form data stop -->
        {% if (file.error) { %}
            <td class="error" colspan="2"><span class="label important">Error</span> {%=fileUploadErrors[file.error] || file.error%}</td>
        {% } else if (o.files.valid && !i) { %}
            <td class="progress"><div class="progressbar"><div style="width:0%;"></div></div></td>
            <td class="start">{% if (!o.options.autoUpload) { %}<button class="btn primary">Start</button>{% } %}</td>
        {% } else { %}
            <td colspan="2"></td>
        {% } %}
        <td class="cancel">{% if (!i) { %}<button class="btn info">Cancel</button>{% } %}</td>
    </tr>
{% } %}
</script>
```

The title field has "title[]" as name to account for multi-file request uploads. The "required" attribute is used to prevent the file upload if this field is not filled out.

Next we only need to adjust the submit event callback method to gather the form data via the context option, which has been set by the UI version of the plugin inside of the *add* callback to the upload row:

```js
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    var inputs = data.context.find(':input');
    if (inputs.filter('[required][value=""]').first().focus().length) {
        return false;
    }
    data.formData = inputs.serializeArray();
});
```

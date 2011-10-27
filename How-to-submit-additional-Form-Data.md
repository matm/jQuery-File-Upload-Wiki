## Adding input fields to the upload form
The easiest way to submit additional form data is by adding additional input fields to the upload form:

```html
<div id="fileupload">
    <form action="upload.php" method="POST" enctype="multipart/form-data">
        <!-- additional form data start / -->
        <input type="hidden" name="example1" value="test">
        <label>Example: <input type="text" name="example2"></label>
        <!-- / additional form data stop -->
        <div class="fileupload-buttonbar">
            <label class="fileinput-button">
                <span>Add files...</span>
                <input type="file" name="files[]" multiple>
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
First, we adjust the upload template and add a new cell with an input field for the title:  

```html
<script id="template-upload" type="text/x-jquery-tmpl">
    <tr class="template-upload{{if error}} ui-state-error{{/if}}">
        <td class="preview"></td>
        <td class="name">{{if name}}${name}{{else}}Untitled{{/if}}</td>
        <td class="size">${sizef}</td>
        <td class="title"><label>Title: <input name="title"></label></td>
        {{if error}}
            <td class="error" colspan="2">Error:
                {{if error === 'maxFileSize'}}File is too big
                {{else error === 'minFileSize'}}File is too small
                {{else error === 'acceptFileTypes'}}Filetype not allowed
                {{else error === 'maxNumberOfFiles'}}Max number of files exceeded
                {{else}}${error}
                {{/if}}
            </td>
        {{else}}
            <td class="progress"><div></div></td>
            <td class="start"><button>Start</button></td>
        {{/if}}
        <td class="cancel"><button>Cancel</button></td>
    </tr>
</script>
```

Next we only need to adjust the submit event callback method to gather the form data via the context option, which has been set by the UI version of the plugin inside of the *add* callback to the upload row:

```js
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    data.formData = data.context.find(':input').serializeArray();
    if (!data.formData.length || !data.formData[0].value) {
      data.context.find(':input')[0].focus();
      return false;
    }
});
```

The above submit callback also works when multiple input fields are added to the upload template (thanks to the [serializeArray method](http://api.jquery.com/serializeArray)), but only checks the first one for a value.
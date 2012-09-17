## Adding input fields to the upload form
The easiest way to submit additional form data is by adding additional input fields to the upload form:

```html
<form id="fileupload" action="server/php/" method="POST" enctype="multipart/form-data">
    <input type="hidden" name="example1" value="test">
    <div class="row">
        <label>Example: <input type="text" name="example2"></label>
    </div>
    <!-- ... -->
</form>
```

By default, the plugin calls [jQuery's serializeArray method](http://api.jquery.com/serializeArray) on the upload form to gather additional form data for all input fields (including hidden fields).  
The value of these form fields will be sent to the server along with the selected files.

**Note:**  
If you set the formData option, these fields won't be send to the server, since the formData object will override them.  
You can however create a formData object of the form fields manually using [jQuery's serializeArray method](http://api.jquery.com/serializeArray) method:

```js
var formData = $('form').serializeArray();
```

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
<script id="template-upload" type="text/x-tmpl">
{% for (var i=0, file; file=o.files[i]; i++) { %}
    <tr class="template-upload fade">
        <!-- ... -->
        <td class="title"><label>Title: <input name="title[]" required></label></td>
        <!-- ... -->
    </tr>
{% } %}
</script>
```

The title field has "title[]" as name to account for multi-file request uploads. The "required" attribute is used to prevent the file upload if this field is not filled out.

Next we need to bind a callback to the "submit" event to gather the form data via the upload row context (the context is set by the UI version of the plugin inside of the *add* callback to the upload row node):

```js
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    var inputs = data.context.find(':input');
    if (inputs.filter('[required][value=""]').first().focus().length) {
        return false;
    }
    data.formData = inputs.serializeArray();
});
```

## How to retrieve and set form data asynchronously.
See how to [[submit files asynchronously]].


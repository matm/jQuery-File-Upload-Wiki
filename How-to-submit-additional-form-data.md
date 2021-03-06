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
If you set the formData option, these fields won't be sent to the server, since the formData object will override them.  
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
      data.context.find('button').prop('disabled', false);
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
    if (inputs.filter(function () {
            return !this.value && $(this).prop('required');
        }).first().focus().length) {
        data.context.find('button').prop('disabled', false);
        return false;
    }
    data.formData = inputs.serializeArray();
});
```

### Setting formData on upload start for each individual file upload for AngularJS version
First, we adjust the upload template and add a new cell with an input field for a file title:  

```html
<!-- ... -->
<table class="table table-striped files ng-cloak">
        <tr data-ng-repeat="file in queue"
            data-ng-class="{'processing': file.$processing()}">
                <!-- ... -->
                <td><input type="text" name="title" ng-model="file.title" placeholder="Title"></td>
                <!-- ... -->
        </tr>
</table>
<!-- ... -->
```

The title field has "file.title" as a ng-model so value of field title will be added to binded object "file" on edit.

Next we need to bind a callback to the "submit" event to gather the form data via the upload row context (the context is set by the UI version of the plugin inside of the *add* callback to the upload row node):

```js
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    data.formData = {title: data.files[0].title};
});
```

## How to retrieve and set form data asynchronously.
See how to [[submit files asynchronously]].

## Handling additional form data on server-side
The additional form data will be sent to the server as standard POST parameters.  
e.g. an additional input field with the **name** attribute **example1** will arrive on server-side as POST parameter **example1**.  
The same goes for a custom formData object, *formData: {example2: 'test'}* will arrive on server-side as POST parameter **example2**.

### PHP
The provided PHP upload handler already provides a method stub to handle additional form data, the function **handle_form_data**:

```php
<?php
// ...
    protected function handle_form_data($file, $index) {
        // Handle form data, e.g. $_REQUEST['description'][$index]
    }
// ...
```

e.g. the formData object *{example2: 'test'}* can be accessed as *$_REQUEST['example2']*.
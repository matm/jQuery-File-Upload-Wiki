# Calling the plugin

Plugin needs to be called on the form itself. You can also call it on a wrapping element, as the plugin will find the inner form(s) of that element. The plugin will then read the form to find file fields using the jQuery *find* method and the *input:file* selector.

For example : 


    <form action='submit.php' method='post' id='uploadForm'>
      <input type=file name='fileUpload' id='fileUpload' />
    </form>


Would be used with the following code : 


    $('#uploadForm').fileUpload(options);


# Options

This is my best guess at what options are and do. If you found more about them, please edit.

### namespace (String)

Defaults to *file_upload*. Not to sure what this is for, but i assume it's used to allow multiple instances of the plugin on a page.

### cssClass (String)

Defaults to *file_upload*. This class is added to the form element on which the plugin is called.

### url (String)

Defaults to the *action* attribute of your form. In our example above, it would be *submit.php*. This is where the page where the form data will be submited. More details on this page later.

### method (String)

Defaults to the form's *method* attribute. Can be **get** or **post**.

### fieldName (String)

Defaults to the form's file-input name attribute. This is the name your script will use. For example, if you set this to 'image', you would retrieve the file through the $_FILES['image'] variable in PHP.

### streaming (boolean)

Defaults to false. This seems to make the progress bar go with the progress of the upload if set to yes, else the progress bar will be at 100% all the time. Not too sure yet how to implement this in your backend script. 
**Needs to be completed**

### formData (function)

Defaults to a basic function which serializes all of the form. Will be used before the AJAX call to find the parameters of the request. You may use the following variables inside of your function : 

* uploadForm : the form itself as a jQuery object
* fileInput : the *input:file* field as a jQuery object

For example, here is the default function used by the plugin : 


    function () {
      return uploadForm.serializeArray();
    }


### withCredentials (boolean)

Default to false. Will set the remote request (xhr) *withCredentials* parameter.


# Backend script examples

**Need help on this**

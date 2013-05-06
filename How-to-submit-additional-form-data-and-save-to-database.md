Files that will be modified. index.html and UploadHandeler.php.

# Step 1 - Index.html
Submitting additional form data.  This can be done by simply adding more input fields as suggested in [how to submit additional form data](https://github.com/blueimp/jQuery-File-Upload/wiki/How-to-submit-additional-form-data). Find the template-upload section as below.

```php<!-- The template to display files available for upload -->
<script id="template-upload" type="text/x-tmpl">
{% for (var i=0, file; file=o.files[i]; i++) { %}
    <tr class="template-upload fade">
        <td>
            <span class="preview"></span>
        </td>
        <td>
            <p class="name">{%=file.name%}</p>
            {% if (file.error) { %}
                <div><span class="label label-important">Error</span> {%=file.error%}</div>
            {% } %}
        </td>
<!-- ADD YOUR FIELD HERE.  THEY CAN BE WHATEVER YOU CHOOSE. REMEMBER THE NAME ATTRIBUTE FOR LATER -->
        <td class="post"><label>Post ID: <input type="hidden" name="postid[]" value="101" required></label></td>
        <td class="client"><label>Client ID: <input name="clientid[]" value="88" required></label></td>
<!-- CODE CONTINUES ON -->
        ```
Just before the </head> tag add the following. It will send the input data to the upload script.
```php
<script>
$('#fileupload').bind('fileuploadsubmit', function (e, data) {
    var inputs = data.context.find(':input');
    if (inputs.filter('[required][value=""]').first().focus().length) {
        return false;
    }
    data.formData = inputs.serializeArray();
});
</script>
</head>
```

# Step 2 - UploadHandler.php

Following some of the details in [Working with databases](https://github.com/blueimp/jQuery-File-Upload/wiki/Working-with-databases) and extending on them so that $_POST data will be received and then can be written to the database. 

Firstly find this function around lin 506.
```php
protected function handle_form_data($file, $index) {
        // Handle form data, e.g. $_REQUEST['description'][$index]
    }
```
This is the function that we will use and modify to collect the $_POST data from the two extra inputs we created in index.html

For ease of use we will modify or create two new fuctions based on this one. Each one will receive the form data that has been sent via $_POST.
Continuing with our example date form index.html we will make the two function. 
## Step 2.1
```php
 protected function handle_form_postid($file, $index) {
        $postid = $_REQUEST['postid'][$index];
        return $postid;
    }
    
    protected function handle_form_clientid($file, $index) {
        $clientid = $_REQUEST['clientid'][$index];
        return $clientid;
    }
```
You may change the fucntions name to suit but you will need to know them to call them soon. 
NOTE: These two functions must be above handle_file_upload(..  so leave them in teh original function locationat about line 517 ish. 

Next follow the instructions to connect to the DB.
find: this->options = array(
Add the following.
## Step 2.2
```php
// mysql connection settings  
'database' => 'YOUR DATABASE',  
'host' => 'localhost',  
'username' => 'YOUR USERNAME',  
'password' => 'YOUR PASSWORD',  
// end
```
Create the database query function.
## Step 2.3
```php
function query($query) {  
	$database = $this->options['database'];  
	$host = $this->options['host'];  
	$username = $this->options['username'];  
	$password = $this->options['password'];  
	$link = mysql_connect($host,$username,$password);  
	if (!$link) {  
		die(mysql_error());  
	}
	$db_selected = mysql_select_db($database);  
	if (!$db_selected) {  
		die(mysql_error());  
	}  
	$result = mysql_query($query);  
	mysql_close($link);  
	return $result;  
}  
```
## Step 2.4
Add files to the database function follows directly below. Notice that the return variables have been added to this function from the function that we made in step 2.1.
```php
function add_img($postid,$clientid,$whichimg)  
{  
	$add_to_db = $this->query("INSERT INTO [yourtable] ([columnone,columtwo,columnthree]) VALUES
	     ('$clientid','$postid','$whichimg')") or die(mysql_error());  
	return $add_to_db;  
}
```

## Step 2.5
This add_img function needs to be called to make it work.  Find handle_file_upload(.. which is above the functions that we have just added. 

Locate and add the code as follows in the handle_file_upload

```php
//Locate this line
if ($this->validate($uploaded_file, $file, $error, $index)) {
            //Add a call to the function we made at step 2.1
            $clientid = $this->handle_form_clientid($file, $index);
            $postid = $this->handle_form_postid($file, $index);
            //Add the call to the database to add the data. Notice the three variables
            //which are passed to the add_img function.           
            $this->add_img($file->name,$clientid,$postid);
            //End of our additions. The code continues...
            $upload_dir = $this->get_upload_path();
```

### Step 3 - Deleting
Add the delete function below the add_img function.
```php
function delete_img($delimg)  
{  
	$delete_from_db = $this->query("DELETE FROM **yourtable** WHERE **yourcolumnone** = '$delimg'") or die(mysql_error());  
	return $delete_from_db;  
}
```

Find the delete function at about line 891 and find this  if ($success) {
Then add our delete call to functionso the code looks like this.
          ```php
 if ($success) {
            //Added to call delete function to delete the image from DB 
            $this->delete_img($file_name);
//Code continues on..
```

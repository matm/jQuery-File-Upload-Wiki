Hi, this is what I have done to have dynamic directory creation for file uploads, using the demo files (html, php, js):

I renamed index.html to index.php, defined a js variable "historia" to pass it to main.js (Must be added before loading main.js file) like this:
```php
<script type="text/javascript">
  var historia= '<? echo $_GET['historia'];?>'; // <<<<<<<<<<<<<
</script>
<script src="js/main.js"></script>
```
Then in main.js itself replace this code:
```js
   // Initialize the jQuery File Upload widget:
    $('#fileupload').fileupload({
        // Uncomment the following to send cross-domain cookies:
        //xhrFields: {withCredentials: true},
        url: 'server/php/' // <<<<<<<<<<<<<
    });
```
with this:
```js
   // Initialize the jQuery File Upload widget:
    $('#fileupload').fileupload({
        // Uncomment the following to send cross-domain cookies:
        //xhrFields: {withCredentials: true},
        url: 'server/php/index.php?historia=' + historia // <<<<<<<<<<<<<
    });
```

Edited the file server/php/index.php to look like this:
```php
error_reporting(E_ALL | E_STRICT);
$historia = $_REQUEST['historia'] ;
require('UploadHandler.php');
if( $historia != '' ){
  $options = array('historia' => $historia);
  $upload_handler = new UploadHandler($options,true);
}
else{
  $upload_handler = new UploadHandler();
}
```

Edited the file UploadHandler.php: 

added new option 'historia' => '', to the constructor
set the option 'user_dirs' => true
set the option 'download_via_php' => false

modified the function on line 151, to look like this:
```php
protected function get_user_id() {
    return $this->options['historia'];
}
```
Modified the funcion on line 186, to look like this:
```php
protected function set_file_delete_properties($file) {
    $file->delete_url = $this->options['script_url']
        .'?historia='.$this->options['historia'].'&file='.rawurlencode($file->name); // <<<<<<<<<<<<<<
    $file->delete_type = $this->options['delete_type'];
    if ($file->delete_type !== 'DELETE') {
        $file->delete_url .= '&_method=DELETE';
    }
    if ($this->options['access_control_allow_credentials']) {
        $file->delete_with_credentials = true;
    }
}
```
In versions over 6.4.3, the function `set_file_delete_properties` is `set_additional_file_properties`, and is in the file 246.

Then call the main file from your browser with the variable historia=your_directory such as:
my/demo/index.php?historia=new_dir

and it wil use '/server/php/files/new_dir' for uploads and directory reading...

And it seems to be working nicely and smoothly, list directory contents, uploads perfect and deletes as well.

Thanks for this contribution... good work!
Since version 5.17 the PHP Upload Handler supports user directories out of the box.
This can be enabled by setting the option **user_dirs** to *true* (in *index.php*):

```php
<?php
// ...
$upload_handler = new UploadHandler(array(
    'user_dirs' => true
));
```

By default, the upload handler makes use of session IDs for the user directories.  
To provide your own implementation, you can override the *get_user_id* method:

```php
<?php
require('upload.class.php');

class CustomUploadHandler extends UploadHandler {
    protected function get_user_id() {
        @session_start();
        return session_id();
    }
}

$upload_handler = new CustomUploadHandler(array(
    'user_dirs' => true
));
```

PHP session IDs are generally not guessable. However, for added security it is possible to provide file downloads only via PHP. To do so, the option **download_via_php** can be set to *true*:

```php
<?php
// ...
$upload_handler = new UploadHandler(array(
    'download_via_php' => true
));
```

Next, the **files** directory can be protected via *.htaccess* (by uncommenting the following lines):

```
AuthName "Authorization required"
AuthType Basic
require valid-user
```
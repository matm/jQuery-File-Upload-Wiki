## Steps
1. Place `UploadHandler.php` in `vendor/file.upload`. You can use any name instead of `file.upload`.
2. Have jQuery setup in your app.
3. Copy jQuery File Upload files in the proper directories of your Cake app, usually `webroot/js`.
4. You can now initialize jQuery-File-Upload options in your controller using following way.
```
App::import('Vendor', 'UploadHandler', array('file' => 'file.upload/UploadHandler.php'));
$options = array(
	        'upload_dir' => 'Your upload directory', 	    
	        'accept_file_types' => '/\.(gif|jpe?g|png)$/i'                     
           );

$upload_handler = new UploadHandler($options);
```
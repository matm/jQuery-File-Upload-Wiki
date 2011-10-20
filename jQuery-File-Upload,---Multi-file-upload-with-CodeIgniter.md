### This page expands the example bundled with the plugin and the previous example of using codeigniter

***

This is a handy piece of code if you are using ajax/json. put this in your config/constants.php 

```
// Define Ajax Request
define('IS_AJAX', isset($_SERVER['HTTP_X_REQUESTED_WITH']) && strtolower($_SERVER['HTTP_X_REQUESTED_WITH']) == 'xmlhttprequest');
```

## controller: upload.php

difference from the original tutorial is setting the json data that is returned to the page.  the image resize is commented out (because I didn't test it yet, should work though) and I added a delete function


```
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Upload extends Controller {



	public function __construct()
	{
		parent::__construct();
		$this->load->helper(array('form', 'url'));
	}

	public function index()
	{
		$this->load->view('admin/upload', array('error' => ''));
	}
		public function do_upload()
	{
	
	$upload_path_url = base_url().'uploads/';
	
		$config['upload_path'] = FCPATH.'uploads/';
		$config['allowed_types'] = 'jpg';
		$config['max_size'] = '30000';
		
	  	$this->load->library('upload', $config);

	  	if ( ! $this->upload->do_upload())
	  	{
	  		$error = array('error' => $this->upload->display_errors());
	  		$this->load->view('upload', $error);
	  	}
	  	else
	  	{ 		
		   $data = $this->upload->data();
		/*	
                  // to re-size for thumbnail images un-comment and set path here and in json array	
		   $config = array(
			'source_image' => $data['full_path'],
			'new_image' => $this->$upload_path_url '/thumbs',
			'maintain_ration' => true,
			'width' => 80,
			'height' => 80
		  );
		
		$this->load->library('image_lib', $config);
		$this->image_lib->resize();
		*/
	//set the data for the json array	
	$info->name = $data[file_name];
        $info->size = $data[file_size];
	$info->type = $data[file_type];
        $info->url = $upload_path_url .$data[file_name];
	$info->thumbnail_url = $upload_path_url .$data[file_name];//I set this to original file since I did not create thumbs.  change to thumbnail directory if you do = $upload_path_url .'/thumbs' .$data[file_name]
        $info->delete_url = base_url().'upload/deleteImage/'.$data[file_name];
        $info->delete_type = 'DELETE';
          

	if (IS_AJAX) {   //this is why we put this in the constants to pass only json data
	           echo json_encode(array($info));
                      }
	else {   // so that this will still work if java is not enabled
		  	$file_data['upload_data'] = $this->upload->data();
		  	$this->load->view('admin/upload_success', $file_data);
		}
					
	 }

}
	

public function deleteImage($file)//gets the job done but you might want to add error checking and security
	{
		$success =unlink(FCPATH.'uploads/' .$file);
		//info to see if it is doing what it is supposed to	
		$info->sucess =$success;
		$info->path =base_url().'uploads/' .$file;
		$info->file =is_file(FCPATH.'uploads/' .$file);
	if (IS_AJAX) {//I don't think it matters if this is set but good for error checking in the console/firebug
	    echo json_encode(array($info));}
	else {     //here you will need to decide what you want to show for a successful delete
		  	$file_data['delete_data'] = $file';
		  	$this->load->view('admin/delete_success', $file_data); 
	       }
}
```
## View: index.php
 this is mostly unchanged from the example code. Change name="files[]" to name="userfile"

I also set the img width to 80px since I did not set a thumbnail image.  I know scaling is a bad idea 

I appended the application.js to the bottom so it does not get excluded

```
<html>
	<head>
	<title>Upload Form</title>
	<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.9/themes/base/jquery-ui.css" id="theme">
	<link rel="stylesheet" href="<?php echo base_url(); ?>js/jquery.fileupload-ui.css">
	<link rel="stylesheet" href="<?php echo base_url(); ?>css/style.css">
	<style>
	body {
		font-family: Verdana, Arial, sans-serif;
		font-size: 13px;
		margin: 0;
		padding: 20px;
	}
	</style>
	</head>
	<body>
<?php echo $error;?>

<div id="fileupload">
    <form action="upload/do_upload" method="POST" enctype="multipart/form-data">
        <div class="fileupload-buttonbar">
            <label class="fileinput-button">
                <span>Add files...</span>
                <input type="file" name="userfile" multiple />
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
<script id="template-upload" type="text/x-jquery-tmpl">
    <tr class="template-upload{{if error}} ui-state-error{{/if}}">
        <td class="preview"></td>
        <td class="name">${name}</td>
        <td class="size">${sizef}</td>
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
<script id="template-download" type="text/x-jquery-tmpl">
    <tr class="template-download{{if error}} ui-state-error{{/if}}">
        {{if error}}
            <td></td>
            <td class="name">${name}</td>
            <td class="size">${sizef}</td>
            <td class="error" colspan="2">Error:
                {{if error === 1}}File exceeds upload_max_filesize (php.ini directive)
                {{else error === 2}}File exceeds MAX_FILE_SIZE (HTML form directive)
                {{else error === 3}}File was only partially uploaded
                {{else error === 4}}No File was uploaded
                {{else error === 5}}Missing a temporary folder
                {{else error === 6}}Failed to write file to disk
                {{else error === 7}}File upload stopped by extension
                {{else error === 'maxFileSize'}}File is too big
                {{else error === 'minFileSize'}}File is too small
                {{else error === 'acceptFileTypes'}}Filetype not allowed
                {{else error === 'maxNumberOfFiles'}}Max number of files exceeded
                {{else error === 'uploadedBytes'}}Uploaded bytes exceed file size
                {{else error === 'emptyResult'}}Empty file upload result
                {{else}}${error}
                {{/if}}
            </td>
        {{else}}
            <td class="preview">
                {{if thumbnail_url}}
                    <a href="${url}" target="_blank"><img width ="80"src="${thumbnail_url}"></a>
                {{/if}}
            </td>
            <td class="name">
                <a href="${url}"{{if thumbnail_url}} target="_blank"{{/if}}>${name}</a>
            </td>
            <td class="size">${sizef}</td>
            <td colspan="2"></td>
        {{/if}}
        <td class="delete">
            <!--<button data-type="${delete_type}" data-url="${delete_url}">Delete</button>-->
												<button data-type="${delete_type}" data-url="${delete_url}">Delete</button>
        </td>
    </tr>
</script>

<script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>
<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.13/jquery-ui.min.js"></script>
<script src="//ajax.aspnetcdn.com/ajax/jquery.templates/beta1/jquery.tmpl.min.js"></script>
<script src="<?php echo base_url(); ?>js/jquery.iframe-transport.js"></script>
<script src="<?php echo base_url(); ?>js/jquery.fileupload.js"></script>
<script src="<?php echo base_url(); ?>js/jquery.fileupload-ui.js"></script>
<!--<script src="<?php echo base_url(); ?>js/application.js"></script>-->
				<script>
			$(function () {
    'use strict';

    // Initialize the jQuery File Upload widget:
    $('#fileupload').fileupload();

    // Load existing files:
    $.getJSON($('#fileupload form').prop('action'), function (files) {
        var fu = $('#fileupload').data('fileupload');
        fu._adjustMaxNumberOfFiles(-files.length);
        fu._renderDownload(files)
            .appendTo($('#fileupload .files'))
            .fadeIn(function () {
                // Fix for IE7 and lower:
                $(this).show();
            });
    });

    // Open download dialogs via iframes,
    // to prevent aborting current uploads:
    $('#fileupload .files a:not([target^=_blank])').live('click', function (e) {
        e.preventDefault();
        $('<iframe style="display:none;"></iframe>')
            .prop('src', this.href)
            .appendTo('body');
    });

});
			
				</script>		 
	</body>
</html>
```



## Success View : upload_success.php
```php
<?php
echo '{"name":"'.$upload_data['file_name'].'","type":"'.$upload_data['file_type'].'","size":"'.$upload_data['file_size'].'"}';
?>
```
## Delete View :  delete_success.php
```php
<?php
echo 'file:' .$delete_data .'-delted' ;
?>
```
-Make sure Uploads/ directory is writable

-I had a problem with seeing images/thumbnails initially then I realized I had not set uploads/ directory in
   my .htacces file

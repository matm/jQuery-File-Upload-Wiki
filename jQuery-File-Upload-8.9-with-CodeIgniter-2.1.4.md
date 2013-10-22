### This is an update to the last CodeIgniter Tutorial

Some things have change and need to be adapted

!!! Please notice this solution has to be completed !!!

I took the sample from the recent version jQuery-File-Upload 8.9 and mixed it with the tutorials for Codeigniter. The upload is currently working but not all status updates are currently correctly presented.

***

This is a handy piece of code if you are using ajax/json. put this in your config/constants.php.

```
// Define Ajax Request
define('IS_AJAX', isset($_SERVER['HTTP_X_REQUESTED_WITH']) && strtolower($_SERVER['HTTP_X_REQUESTED_WITH']) == 'xmlhttprequest');
```

Alternatively you can use CI's native ajax detection

```
$this->input->is_ajax_request();
```
***

Create a directory like /uploads + /uploads/thumnail and change chmod properly (e.g. 777)

***
## Router: routes.php

```
$route['upload'] = 'upload';
$route['upload/do_upload'] = 'upload/do_upload';
```

## controller: upload.php

Especially the way the Ajax return data is composed has to be updated.

```
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Upload extends CI_Controller {

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

	  	if ( ! $this->upload->do_upload()) {
	  		$error = array('error' => $this->upload->display_errors());
	  		$this->load->view('upload', $error);
		
		} else {
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
			$info->name = $data['file_name'];
	        	$info->size = $data['file_size'];
			$info->type = $data['file_type'];
		        $info->url = $upload_path_url .$data['file_name'];
			// I set this to original file since I did not create thumbs.  change to thumbnail directory if you do = $upload_path_url .'/thumbs' .$data['file_name']
			$info->thumbnail_url = $upload_path_url .$data['file_name'];
		        $info->delete_url = base_url().'upload/deleteImage/'.$data['file_name'];
		        $info->delete_type = 'DELETE';
			$info->error = null;
                        
                        $files[] = $info;
			//this is why we put this in the constants to pass only json data
			if (IS_AJAX) {
				echo json_encode(array("files" => $files));
				//this has to be the only data returned or you will get an error.
				//if you don't give this a json array it will give you a Empty file upload result error
				//it you set this without the if(IS_AJAX)...else... you get ERROR:TRUE (my experience anyway)

			// so that this will still work if javascript is not enabled
			} else {
		  		$file_data['upload_data'] = $this->upload->data();
			  	$this->load->view('upload/upload_success', $file_data);
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
		
		if (IS_AJAX) {
			//I don't think it matters if this is set but good for error checking in the console/firebug
			echo json_encode(array($info));
		} else {
			//here you will need to decide what you want to show for a successful delete		
			$file_data['delete_data'] = $file;
			$this->load->view('admin/delete_success', $file_data); 
		}
	}
}
```
## View: /views/upload/upload.php
Mostly unchanged from the example code. Change name="files[]" to name="userfile" is very important!

I included the same css and js as the current version 8.9 example. The files are moved to assets/frontend/js/fileupload/ and assets/frontend/css/fileupload/. If you go for another path you have to change the code accordingly.

```
<!-- Bootstrap styles -->
<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
<!-- Generic page styles -->
<link rel="stylesheet" href="<?php echo base_url(); ?>assets/frontend/css/fileupload/style.css">
<!-- blueimp Gallery styles -->
<link rel="stylesheet" href="http://blueimp.github.io/Gallery/css/blueimp-gallery.min.css">
<!-- CSS to style the file input field as button and adjust the Bootstrap progress bars -->
<link rel="stylesheet" href="<?php echo base_url(); ?>assets/frontend/css/fileupload/jquery.fileupload.css">
<link rel="stylesheet" href="<?php echo base_url(); ?>assets/frontend/css/fileupload/jquery.fileupload-ui.css">
<!-- CSS adjustments for browsers with JavaScript disabled -->
<noscript><link rel="stylesheet" href="css/jquery.fileupload-noscript.css"></noscript>
<noscript><link rel="stylesheet" href="css/jquery.fileupload-ui-noscript.css"></noscript>

<div id="fileupload">
       <!-- Upload function on action form -->
	<form action="upload/do_upload" method="POST" enctype="multipart/form-data">
        <!-- The fileupload-buttonbar contains buttons to add/delete files and start/cancel the upload -->
        <div class="row fileupload-buttonbar">
            <div class="col-lg-7">
                <!-- The fileinput-button span is used to style the file input field as button -->
                <span class="btn btn-success fileinput-button">
                    <i class="glyphicon glyphicon-plus"></i>
                    <span>Add files...</span>
                    <input type="file" name="userfile" multiple>
                </span>
                <button type="submit" class="btn btn-primary start">
                    <i class="glyphicon glyphicon-upload"></i>
                    <span>Start upload</span>
                </button>
                <button type="reset" class="btn btn-warning cancel">
                    <i class="glyphicon glyphicon-ban-circle"></i>
                    <span>Cancel upload</span>
                </button>
                <button type="button" class="btn btn-danger delete">
                    <i class="glyphicon glyphicon-trash"></i>
                    <span>Delete</span>
                </button>
                <input type="checkbox" class="toggle">
                <!-- The loading indicator is shown during file processing -->
                <span class="fileupload-loading"></span>
            </div>
            <!-- The global progress information -->
            <div class="col-lg-5 fileupload-progress fade">
                <!-- The global progress bar -->
                <div class="progress progress-striped active" role="progressbar" aria-valuemin="0" aria-valuemax="100">
                    <div class="progress-bar progress-bar-success" style="width:0%;"></div>
                </div>
                <!-- The extended global progress information -->
                <div class="progress-extended">&nbsp;</div>
            </div>
        </div>
        <!-- The table listing the files available for upload/download -->
        <table role="presentation" class="table table-striped"><tbody class="files"></tbody></table>
    </form>
</div>

            <!-- The blueimp Gallery widget -->
<div id="blueimp-gallery" class="blueimp-gallery blueimp-gallery-controls" data-filter=":even">
    <div class="slides"></div>
    <h3 class="title"></h3>
    <a class="prev">‹</a>
    <a class="next">›</a>
    <a class="close">×</a>
    <a class="play-pause"></a>
    <ol class="indicator"></ol>
</div>
<!-- The template to display files available for upload -->
<script id="template-upload" type="text/x-tmpl">
{% for (var i=0, file; file=o.files[i]; i++) { %}
    <tr class="template-upload fade">
        <td>
            <span class="preview"></span>
        </td>
        <td>
            <p class="name">{%=file.name%}</p>
            {% if (file.error) { %}
                <div><span class="label label-danger">Error</span> {%=file.error%}</div>
            {% } %}
        </td>
        <td>
            <p class="size">{%=o.formatFileSize(file.size)%}</p>
            {% if (!o.files.error) { %}
                <div class="progress progress-striped active" role="progressbar" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"><div class="progress-bar progress-bar-success" style="width:0%;"></div></div>
            {% } %}
        </td>
        <td>
            {% if (!o.files.error && !i && !o.options.autoUpload) { %}
                <button class="btn btn-primary start">
                    <i class="glyphicon glyphicon-upload"></i>
                    <span>Start</span>
                </button>
            {% } %}
            {% if (!i) { %}
                <button class="btn btn-warning cancel">
                    <i class="glyphicon glyphicon-ban-circle"></i>
                    <span>Cancel</span>
                </button>
            {% } %}
        </td>
    </tr>
{% } %}
</script>
<!-- The template to display files available for download -->
<script id="template-download" type="text/x-tmpl">
{% for (var i=0, file; file=o.files[i]; i++) { %}
    <tr class="template-download fade">
        <td>
            <span class="preview">
                {% if (file.thumbnailUrl) { %}
                    <a href="{%=file.url%}" title="{%=file.name%}" download="{%=file.name%}" data-gallery><img src="{%=file.thumbnailUrl%}"></a>
                {% } %}
            </span>
        </td>
        <td>
            <p class="name">
                {% if (file.url) { %}
                    <a href="{%=file.url%}" title="{%=file.name%}" download="{%=file.name%}" {%=file.thumbnailUrl?'data-gallery':''%}>{%=file.name%}</a>
                {% } else { %}
                    <span>{%=file.name%}</span>
                {% } %}
            </p>
            {% if (file.error) { %}
                <div><span class="label label-danger">Error</span> {%=file.error%}</div>
            {% } %}
        </td>
        <td>
            <span class="size">{%=o.formatFileSize(file.size)%}</span>
        </td>
        <td>
            {% if (file.deleteUrl) { %}
                <button class="btn btn-danger delete" data-type="{%=file.deleteType%}" data-url="{%=file.deleteUrl%}"{% if (file.deleteWithCredentials) { %} data-xhr-fields='{"withCredentials":true}'{% } %}>
                    <i class="glyphicon glyphicon-trash"></i>
                    <span>Delete</span>
                </button>
                <input type="checkbox" name="delete" value="1" class="toggle">
            {% } else { %}
                <button class="btn btn-warning cancel">
                    <i class="glyphicon glyphicon-ban-circle"></i>
                    <span>Cancel</span>
                </button>
            {% } %}
        </td>
    </tr>
{% } %}
</script>
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<!-- The jQuery UI widget factory, can be omitted if jQuery UI is already included -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.ui.widget.js"></script>
<!-- The Templates plugin is included to render the upload/download listings -->
<script src="http://blueimp.github.io/JavaScript-Templates/js/tmpl.min.js"></script>
<!-- The Load Image plugin is included for the preview images and image resizing functionality -->
<script src="http://blueimp.github.io/JavaScript-Load-Image/js/load-image.min.js"></script>
<!-- The Canvas to Blob plugin is included for image resizing functionality -->
<script src="http://blueimp.github.io/JavaScript-Canvas-to-Blob/js/canvas-to-blob.min.js"></script>
<!-- Bootstrap JS is not required, but included for the responsive demo navigation -->
<script src="//netdna.bootstrapcdn.com/bootstrap/3.0.0/js/bootstrap.min.js"></script>
<!-- blueimp Gallery script -->
<script src="http://blueimp.github.io/Gallery/js/jquery.blueimp-gallery.min.js"></script>
<!-- The Iframe Transport is required for browsers without support for XHR file uploads -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.iframe-transport.js"></script>
<!-- The basic File Upload plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload.js"></script>
<!-- The File Upload processing plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-process.js"></script>
<!-- The File Upload image preview & resize plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-image.js"></script>
<!-- The File Upload audio preview plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-audio.js"></script>
<!-- The File Upload video preview plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-video.js"></script>
<!-- The File Upload validation plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-validate.js"></script>
<!-- The File Upload user interface plugin -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/jquery.fileupload-ui.js"></script>
<!-- The main application script -->
<script src="<?php echo base_url(); ?>assets/frontend/js/fileupload/main.js"></script>
<!-- The XDomainRequest Transport is included for cross-domain file deletion for IE 8 and IE 9 -->
<!--[if (gte IE 8)&(lt IE 10)]>
<script src="js/cors/jquery.xdr-transport.js"></script>
<![endif]-->
```



## Success View : /views/upload/upload_success.php

this will only show if javascript is disabled or the browser does not support the plugin. When testing this with javascript disabled, it will only upload the last file in the list if multiple items are selected.
delete does not work because of no direct access for security

```php
<?php
echo '{"name":"'.$upload_data['file_name'].'","type":"'.$upload_data['file_type'].'","size":"'.$upload_data['file_size'].'"}';
?>
<br/><br/><br/>
<a href= "<?php echo base_url().'uploads/' .$upload_data['file_name'] ?>" ><img style="float:left; padding: 20px;" width="80" src="<?php echo base_url().'uploads/' .$upload_data['file_name']/*or set to thumbnail image*/ ?>"/></a><?php echo '<br/>name: ' .$upload_data['file_name'] .'<br/>size: ' .$upload_data['file_size'] .' k' ?> <!-- <br/><a href="upload/delete <?php echo $upload_data['file_name']?>"  >DELETE</a>-->
```
## Delete View :  /views/upload/delete_success.php
probably won't be used but here is is anywayss
```php
<?php
echo 'file:' .$delete_data .'-deleted' ;
?>
```

### main.js
```
/*
 * jQuery File Upload Plugin JS Example 8.9.0
 * https://github.com/blueimp/jQuery-File-Upload
 *
 * Copyright 2010, Sebastian Tschan
 * https://blueimp.net
 *
 * Licensed under the MIT license:
 * http://www.opensource.org/licenses/MIT
 */

/*jslint nomen: true, regexp: true */
/*global $, window, blueimp */

$(function () {
    'use strict';

    // Initialize the jQuery File Upload widget:
    $('#fileupload').fileupload({});

    // Enable iframe cross-domain access via redirect option:
    $('#fileupload').fileupload(
        'option',
        'redirect',
        window.location.href.replace(
            /\/[^\/]*$/,
            '/cors/result.html?%s'
        )
    );

   // Load existing files:
    $('#fileupload').addClass('fileupload-processing');

    $.ajax({
        // Uncomment the following to send cross-domain cookies:
        //xhrFields: {withCredentials: true},
        url: $('#fileupload').fileupload('option', 'url'),
        dataType: 'json',
        context: $('#fileupload')[0]
    }).always(function () {
        $(this).removeClass('fileupload-processing');
    }).done(function (result) {
        $(this).fileupload('option', 'done')
            .call(this, $.Event('done'), {result: result});
    });
    

});

```
-Make sure Uploads/ directory is writable

-make sure you have the jquery-file-upload  .js and .css files in the right place

-I had a problem with seeing images/thumbnails initially then I realized I had not set uploads/ directory in
   my .htacces file

***
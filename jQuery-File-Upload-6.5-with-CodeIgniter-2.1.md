## Issues
If the upload doesn't work :

* Look if your upload directory exist and is writable : is_dir(), is_writable()
* Look your php.ini : Safe_mod, file_uploads ...
* The .htaccess may be false
* You have an error in your code ahah !

##Source 
If you want the [source](http://perrot-julien.fr/tuto/Ci_source.zip)

## config/constants.php
This is a handy piece of code if you are using ajax/json. put this in your config/constants.php



```php
<?php
// Define Ajax Request
define('IS_AJAX', isset($_SERVER['HTTP_X_REQUESTED_WITH']) && strtolower($_SERVER['HTTP_X_REQUESTED_WITH']) == 'xmlhttprequest');
```


## Upload View :

```html
<html>
    <head>
          <link rel="stylesheet" type="text/css" media="screen" href="http://url/assets/css/bootstrap.css"/>
          <link rel="stylesheet" type="text/css" media="screen" href="http://url/assets/css/fileupload/bootstrap-image-gallery.min.css"/>
          <link rel="stylesheet" type="text/css" media="screen" href="http://url/assets/css/fileupload/jquery.fileupload-ui.css"/>
          <link rel="stylesheet" type="text/css" media="screen" href="http://url/assets/css/jquery-ui.css"/>
          
          <script  src="http://code.jquery.com/jquery-1.7.min.js" ></script>
          <script  type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.8.16/jquery-ui.min.js" ></script>

          <script  type="text/javascript" src="http://url/assets/js/fileupload/tmpl.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/load-image.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/canvas-to-blob.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/bootstrap.min.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/bootstrap-image-gallery.min.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/jquery.iframe-transport.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/jquery.fileupload.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/jquery.fileupload-ip.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/jquery.fileupload-ui.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/locale.js" ></script>
          <script  type="text/javascript" src="http://url/assets/js/fileupload/main.js" ></script>
     </head>
<body>

<div id="upload-img">
    <h2>Upload a file</h2>

<!-- Upload function on action form -->
    <form id="fileupload" action="<? echo base_url() . 'upload/upload_img'; ?>" method="POST" enctype="multipart/form-data">

<!-- The fileupload-buttonbar contains buttons to add/delete files and start/cancel the upload -->
    <div class="row fileupload-buttonbar">

        <div class="span7">

<!-- The fileinput-button span is used to style the file input field as button -->
            <span class="btn btn-success fileinput-button">
                <span><i class="icon-plus icon-white"></i> Add files...</span>
<!-- Replace name of this input by userfile-->
                <input type="file" name="userfile" multiple>
            </span>
            <button type="submit" class="btn btn-primary start">
                <i class="icon-upload icon-white"></i> Start upload
           </button>
           <button type="reset" class="btn btn-warning cancel">
                <i class="icon-ban-circle icon-white"></i> Cancel upload
          </button>
          <button type="button" class="btn btn-danger delete">
               <i class="icon-trash icon-white"></i> Delete
          </button>
          <input type="checkbox" class="toggle">
       </div>
       
       <div class="span5">

 <!-- The global progress bar -->
        <div class="progress progress-success progress-striped active fade">
             <div class="bar" style="width:0%;"></div>
        </div>
     </div>
  </div>

<!-- The loading indicator is shown during image processing -->
   <div class="fileupload-loading"></div>
        <br>
<!-- The table listing the files available for upload/download -->
        <table class="table table-striped"><tbody class="files" data-toggle="modal-gallery" data-target="#modal-gallery"></tbody></table>
   </form>
</div>

<!-- The template text-tmpl upload/download -->


...
...

</body>
</html>
```

## Controller Upload : 


characters to replace : 
$name = strtr($name, 'ÀÁÂÃÄÅÇÈÉÊËÌÍÎÏÒÓÔÕÖÙÚÛÜÝàáâãäåçèéêëìíîïðòóôõöùúûüýÿ', 'AAAAAACEEEEIIIIOOOOOUUUUYaaaaaaceeeeiiiioooooouuuuyy');


```php
<?php if (!defined('BASEPATH'))
    exit('No direct script access allowed');

class Upload extends CI_Controller {

    protected $path_img_upload_folder;
    protected $path_img_thumb_upload_folder;
    protected $path_url_img_upload_folder;
    protected $path_url_img_thumb_upload_folder;

    protected $delete_img_url;

  function __construct() {
        parent::__construct();
        $this->load->helper(array('form', 'url'));

//Set relative Path with CI Constant
        $this->setPath_img_upload_folder(FCPATH . "assets/img/articles/");
        $this->setPath_img_thumb_upload_folder(FCPATH . "assets/img/articles/thumbnails/");

        
//Delete img url
        $this->setDelete_img_url(base_url() . 'admin/deleteImage/');
 

//Set url img with Base_url()
        $this->setPath_url_img_upload_folder(base_url() . "assets/img/articles/");
        $this->setPath_url_img_thumb_upload_folder(base_url() . "assets/img/articles/thumbnails/");
  }

  public function index() {
      $this->load->view('upload_v/upload_view');
  }

// Function called by the form
  public function upload_img() {

        //Format the name
        $name = $_FILES['userfile']['name'];
        $name = strtr($name, 'ÀÁÂÃÄÅÇÈÉÊËÌÍÎÏÒÓÔÕÖÙÚÛÜÝàáâãäåçèéêëìíîïðòóôõöùúûüýÿ', 'AAAAAACEEEEIIIIOOOOOUUUUYaaaaaaceeeeiiiioooooouuuuyy');

        // replace characters other than letters, numbers and . by _
        $name = preg_replace('/([^.a-z0-9]+)/i', '_', $name);

        //Your upload directory, see CI user guide
        $config['upload_path'] = $this->getPath_img_upload_folder();
  
        $config['allowed_types'] = 'gif|jpg|png|JPG|GIF|PNG';
        $config['max_size'] = '1000';
        $config['file_name'] = $name;

       //Load the upload library
        $this->load->library('upload', $config);

       if ($this->do_upload()) {
            
            //If you want to resize 
            $config['new_image'] = $this->getPath_img_thumb_upload_folder();
            $config['image_library'] = 'gd2';
            $config['source_image'] = $this->getPath_img_upload_folder() . $name;
            $config['create_thumb'] = FALSE;
            $config['maintain_ratio'] = TRUE;
            $config['width'] = 193;
            $config['height'] = 94;

            $this->load->library('image_lib', $config);

            $this->image_lib->resize();

           $data = $this->upload->data();

            //Get info 
            $info = new stdClass();
            
            $info->name = $data['file_name'];
            $info->size = $data['file_size'];
            $info->type = $data['file_type'];
            $info->url = $this->getPath_img_upload_folder() . $name;
            $info->thumbnail_url = $this->getPath_img_thumb_upload_folder() . $name; //I set this to original file since I did not create thumbs.  change to thumbnail directory if you do = $upload_path_url .'/thumbs' .$name
            $info->delete_url = $this->getDelete_img_url() . $name;
            $info->delete_type = 'DELETE';


           //Return JSON data
           if (IS_AJAX) {   //this is why we put this in the constants to pass only json data
                echo json_encode(array($info));
                //this has to be the only the only data returned or you will get an error.
                //if you don't give this a json array it will give you a Empty file upload result error
                //it you set this without the if(IS_AJAX)...else... you get ERROR:TRUE (my experience anyway)
            } else {   // so that this will still work if javascript is not enabled
                $file_data['upload_data'] = $this->upload->data();
                echo json_encode(array($info));
            }
        } else {

            $error = array('error' => $this->upload->display_errors());

            echo json_encode($error);
        }


       }
  }

//Function for the upload : return true/false
  public function do_upload() {

        if (!$this->upload->do_upload()) {

            return false;
        } else {
            //$data = array('upload_data' => $this->upload->data());

            return true;
        }
     }


//Function Delete image
    public function deleteImage() {

        //Get the name in the url
        $file = $this->uri->segment(3);
        
        $success = unlink($this->getPath_img_upload_folder() . $file);
        $success_th = unlink($this->getPath_img_thumb_upload_folder() . $file);

        //info to see if it is doing what it is supposed to	
        $info = new stdClass();
        $info->sucess = $success;
        $info->path = $this->getPath_url_img_upload_folder() . $file;
        $info->file = is_file($this->getPath_img_upload_folder() . $file);
        if (IS_AJAX) {//I don't think it matters if this is set but good for error checking in the console/firebug
            echo json_encode(array($info));
        } else {     //here you will need to decide what you want to show for a successful delete
            var_dump($file);
        }
    }


//Load the files
    public function get_files() {

        $this->get_scan_files();
    }

//Get info and Scan the directory
    public function get_scan_files() {

        $file_name = isset($_REQUEST['file']) ?
                basename(stripslashes($_REQUEST['file'])) : null;
        if ($file_name) {
            $info = $this->get_file_object($file_name);
        } else {
            $info = $this->get_file_objects();
        }
        header('Content-type: application/json');
        echo json_encode($info);
    }

    protected function get_file_object($file_name) {
        $file_path = $this->getPath_img_upload_folder() . $file_name;
        if (is_file($file_path) && $file_name[0] !== '.') {

            $file = new stdClass();
            $file->name = $file_name;
            $file->size = filesize($file_path);
            $file->url = $this->getPath_url_img_upload_folder() . rawurlencode($file->name);
            $file->thumbnail_url = $this->getPath_url_img_thumb_upload_folder() . rawurlencode($file->name);
            //File name in the url to delete 
            $file->delete_url = $this->getDelete_img_url() . rawurlencode($file->name);
            $file->delete_type = 'DELETE';
            
            return $file;
        }
        return null;
    }

//Scan
       protected function get_file_objects() {
        return array_values(array_filter(array_map(
             array($this, 'get_file_object'), scandir($this->getPath_img_upload_folder())
                   )));
    }



// GETTER & SETTER 


    public function getPath_img_upload_folder() {
        return $this->path_img_upload_folder;
    }

    public function setPath_img_upload_folder($path_img_upload_folder) {
        $this->path_img_upload_folder = $path_img_upload_folder;
    }

    public function getPath_img_thumb_upload_folder() {
        return $this->path_img_thumb_upload_folder;
    }

    public function setPath_img_thumb_upload_folder($path_img_thumb_upload_folder) {
        $this->path_img_thumb_upload_folder = $path_img_thumb_upload_folder;
    }

    public function getPath_url_img_upload_folder() {
        return $this->path_url_img_upload_folder;
    }

    public function setPath_url_img_upload_folder($path_url_img_upload_folder) {
        $this->path_url_img_upload_folder = $path_url_img_upload_folder;
    }

    public function getPath_url_img_thumb_upload_folder() {
        return $this->path_url_img_thumb_upload_folder;
    }

    public function setPath_url_img_thumb_upload_folder($path_url_img_thumb_upload_folder) {
        $this->path_url_img_thumb_upload_folder = $path_url_img_thumb_upload_folder;
    }

    public function getDelete_img_url() {
        return $this->delete_img_url;
    }

    public function setDelete_img_url($delete_img_url) {
        $this->delete_img_url = $delete_img_url;
    }


}



```

## js/main.js

```js
//this is the application.js file from the example code//
$(function () {
    'use strict';

    // Initialize the jQuery File Upload widget:
    $('#fileupload').fileupload();
    
    // Enable iframe cross-domain access via redirect option:
    $('#fileupload').fileupload(
        'option',
        'redirect',
        window.location.href.replace(
            /\/[^\/]*$/,
            './cors/result.html?%s'
            )
        );

//Set your url localhost or your ndd (perrot-julien.fr)
    if (window.location.hostname === 'localhost') {
        //Load files
        // Upload server status check for browsers with CORS support:
        if ($.ajaxSettings.xhr().withCredentials !== undefined) {
            $.ajax({
                url: 'upload/get_files',
                dataType: 'json', 
                
                success : function(data) {  

                    var fu = $('#fileupload').data('fileupload'), 
                    template;
                    fu._adjustMaxNumberOfFiles(-data.length);
                    template = fu._renderDownload(data)
                    .appendTo($('#fileupload .files'));
                    
                    // Force reflow:
                    fu._reflow = fu._transition && template.length &&
                    template[0].offsetWidth;
                    template.addClass('in');
                    $('#loading').remove();
                }  
         
                
            }).fail(function () {
                $('<span class="alert alert-error"/>')
                .text('Upload server currently unavailable - ' +
                    new Date())
                .appendTo('#fileupload');
            });
        }
    } else {
        // Load existing files:
        $('#fileupload').each(function () {
            var that = this;
            $.getJSON(this.action, function (result) {
                if (result && result.length) {
                    $(that).fileupload('option', 'done')
                    .call(that, null, {
                        result: result
                    });
                }
            });
        });
    }


    // Open download dialogs via iframes,
    // to prevent aborting current uploads:
    $('#fileupload .files a:not([target^=_blank])').live('click', function (e) {
        e.preventDefault();
        $('<iframe style="display:none;"></iframe>')
        .prop('src', this.href)
        .appendTo('body');
    });

});

```
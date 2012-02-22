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

```php
<?php if (!defined('BASEPATH'))
    exit('No direct script access allowed');

class Upload extends CI_Controller {

  function __construct() {
        parent::__construct();
        $this->load->helper(array('form', 'url'));
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
        $config['upload_path'] = './assets/img/articles/';
        $upload_path_url = $config['upload_path'];
        $config['allowed_types'] = 'gif|jpg|png|JPG|GIF|PNG';
        $config['max_size'] = '1000';
        $config['file_name'] = $name;

       //Load the upload library
        $this->load->library('upload', $config);

       if ($this->do_upload()) {
            
            //If you want to resize 
            $config['new_image'] = './assets/img/articles/thumbnails/';
            $config['image_library'] = 'gd2';
            $config['source_image'] = './assets/img/articles/' . $name;
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
            $info->url = $upload_path_url . $data['file_name'];
            $info->thumbnail_url = $upload_path_url.'thumbnails/' . $data['file_name']; //I set this to original file since I did not create thumbs.  change to thumbnail directory if you do = $upload_path_url .'/thumbs' .$data['file_name']
            $info->delete_url = base_url() . 'upload/deleteImage/' . $data['file_name'];
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
        
        $success = unlink(FCPATH . 'assets/img/articles/' . $file);
        $success_th = unlink(FCPATH . 'assets/img/articles/thumbnails/' . $file);

        //info to see if it is doing what it is supposed to	
        $info = new stdClass();
        $info->sucess = $success;
        $info->path = base_url() . 'assets/img/articles/' . $file;
        $info->file = is_file(FCPATH . 'assets/img/articles/' . $file);
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
        $file_path = FCPATH . 'assets/img/articles/' . $file_name;
        if (is_file($file_path) && $file_name[0] !== '.') {

            $file = new stdClass();
            $file->name = $file_name;
            $file->size = filesize($file_path);
            $file->url = base_url() . 'assets/img/articles/' . rawurlencode($file->name);
            $file->thumbnail_url = base_url() . 'assets/img/articles/thumbnails/' . rawurlencode($file->name);
            //File name in the url to delete 
            $file->delete_url = base_url() ."upload/deleteImage/". rawurlencode($file->name);
            $file->delete_type = 'DELETE';
            
            return $file;
        }
        return null;
    }

//Scan
       protected function get_file_objects() {
        return array_values(array_filter(array_map(
             array($this, 'get_file_object'), scandir(FCPATH . 'assets/img/articles/')
                   )));
    }

}



```
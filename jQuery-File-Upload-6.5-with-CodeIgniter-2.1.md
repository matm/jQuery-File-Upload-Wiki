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
  }

//Function for the upload : return true/false
  public function do_upload() {

        if (!$this->upload->do_upload()) {
            $error = array('error' => $this->upload->display_errors());
            foreach ($error as $v) {
                echo $v;
            }
            return false;
        } else {
            //$data = array('upload_data' => $this->upload->data());

            return true;
        }
     }

  }



```
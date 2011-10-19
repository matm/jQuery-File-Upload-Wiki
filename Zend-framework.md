#Zend Framework Integration 
CONTRIBUTOR: Christopher Hogan <ceo@foundco.com> http://foundco.com

// USER CONTROLLER (example form with one action ... "index")

./modules/users/controller/UserController.php

./modules/users/views/scripts/index/index.phtml (view action)

./modules/users/views/scripts/forms/myform.phtml (view action form)

// UPLOAD CONTROLLER (example code below, this is used for uploading files sent from jquery-file-upload iframe module)

./modules/users/controller/UploadController.php

# Script Examples
This is a simple upload controller for use with BlueImp/JQuery-File-Upload.  The idea is that you create a module, then you put the controller in it.  So for instance if you have a module called "User" you create a controller called "UploadController.php".  Then you put the url for the upload controller in the form which you will use as your view script (the default form with jquery-file-upload).

## Example Form for Zend Framework Integration
### (view script index.phtml) 

    <head>
    <script type="text/javascript" src="http://cdn.jquerytools.org/1.2.6/full/jquery.tools.min.js"></script>
    </head>
    <!-- this form uses jquery tools -->
    <form id="myform" method="post" enctype="multipart/form-data action="<?=$this->url(array("module"=>"user","controller"=>"**upload**","action"=>"index"),null,true);?>">
        <?= $this->partial('forms/**myform.phtml**', array('data'=>$this->data) ?>
        <button type="submit" name="signup"><?= $this->translate->_("Submit") ?></button> 
    </form>
    <script> 
    $("#myform").validator();
    </script> 

# MEAT AND POTATOES:
The bulk of this code is primarily the Upload Controller.  This will replace the "upload.php" file, and it's used primarily because ZendFramework doesn't play nice with the standard "upload.php" code as you might have noticed.  It's better to completely integrate with the framework, so this provides a super simple way to do that.  It can certainly be refined.

    <?php
    /************************
     * @author Christopher Hogan <ceo@foundco.com>
     ******************************/
    
    class User_UploadController extends Objectcode_Controller_User
    {
      public $session;
      private $uploads = '/../public/images/uploads';
      private $uploads_rel = '/images/uploads/';
    
    	public function init() {
    		parent::init();  // this is just for session init (pulls session data for user_id, and email)
    	}
      
      public function indexAction() {
          if ($this->_request->isOptions()) {  
            // we're using user_id and email here as a way to verify the upload and store the file in a specific directory,
            // you can strip that out for your purposes.
            $this->upload( $this->session->user['id'], $this->session->user['email'] );
          }
          if ($this->_request->isPost()) {
            $this->upload( $this->session->user['id'], $this->session->user['email'] );
          }
          if ($this->_request->isGet()) {
            $this->upload( $this->session->user['id'], $this->session->user['email'] );
          }
          if ($this->_request->isDelete() || $_SERVER['REQUEST_METHOD'] == 'DELETE') {
            $this->delete( $this->session->user['id'], $this->session->user['email'] );
          }
          exit;
      }
    
      public function upload($user_id, $email) {
          
          if ($user_id && $email) {
              $adapter = new Zend_File_Transfer_Adapter_Http();
              $user_path = PUBLIC_PATH. $this->uploads_rel. $user_id;
    
              if (!file_exists($user_path)) mkdir($user_path);
    
              $adapter->setDestination(PUBLIC_PATH. $this->uploads_rel. $user_id);
              $adapter->addValidator('Extension', false, 'jpg,png,gif');
      
              $files = $adapter->getFileInfo();
              foreach ($files as $file => $info) {
                  $name = $adapter->getFileName($file);
                  // file uploaded & is valid
                  if (!$adapter->isUploaded($file)) continue; 
                  if (!$adapter->isValid($file)) continue;
    
                  // receive the files into the user directory
                  $adapter->receive($file); // this has to be on top
           
                        // you could apply a filter like this too (if you want), to rename the file:     
                        //  $filterFileRename = new Zend_Filter_File_Rename(array('target' => $rename));
                        //  $filterFileRename->filter($name); // this has to use name
    
                  $fileclass = new stdClass();
    
                  // we stripped out the image thumbnail for our purpose, primarily for security reasons
                  // you could add it back in here.
                  $fileclass->name = str_replace(PUBLIC_PATH. $this->uploads_rel, 'New Image Upload Complete:   ', preg_replace('/\d\//','',$name));  
                  $fileclass->size = $adapter->getFileSize($file);  
                  $fileclass->type = $adapter->getMimeType($file);  
                  $fileclass->delete_url = '/user/upload';
                  $fileclass->delete_type = 'DELETE';
                  //$fileclass->error = 'null';
                  $fileclass->url = '/'; 
            
                  $datas[] = $fileclass;
              }
              
              header('Pragma: no-cache');
              header('Cache-Control: private, no-cache');
              header('Content-Disposition: inline; filename="files.json"');
              header('X-Content-Type-Options: nosniff');
              header('Vary: Accept');
              echo json_encode($datas);
          }
      }
    
      public function delete($user_id, $email) {
          if ($user_id && $email) {
            $file_name = $this->_request->getParam('files');
            // this has been customized to remove only specific images in certain user_id folders
            // you should modify that to your needs
            $file_path = PUBLIC_PATH. $this->uploads_rel. $user_id. '/'. $file_name;
            $success = is_file($file_path) && $file_name[0] !== '.' && unlink($file_path);
          }
          echo json_encode($success);
      }
    
    }

## Standard Blueimp/Jquery-File-Upload form template:
This is just the standard template for myform.phtml. The template above is mainly split up for educational purposes to show you what you need to change.  The code below is in the jquery-file-upload example.  It's included just so you know what goes in the myform.phtml.

    <div id="fileupload">
            <div class="fileupload-buttonbar">
                <label class="fileinput-button">
                    <span>Add files...</span>
                    <input type="file" name="files[]" multiple>
                </label>
                <button type="submit" class="start">Start upload</button>
                <button type="reset" class="cancel">Cancel upload</button>
                <button type="button" class="delete">Delete files</button>
            </div>
    
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
                        <a href="${url}" target="_blank"><img src="${thumbnail_url}"></a>
                    {{/if}}
                </td>
                <td class="name">
                    <a href="${url}"{{if thumbnail_url}} target="_blank"{{/if}}>${name}</a>
                </td>
                <td class="size">${sizef}</td>
                <td colspan="2"></td>
            {{/if}}
            <td class="delete">
                <button data-type="${delete_type}" data-url="${delete_url}">Delete</button>
            </td>
        </tr>
    </script>


# Conclusion
This script is very simple.  It shows a nifty way to replicate the behavior utilized in the default upload.php code, the only difference being that it's done in Zend Framework and with 100 lines of code.  If you need to add stuff to a database I highly recommend using Doctrine for that!

Questions and suggestions are encouraged.  Thanks!
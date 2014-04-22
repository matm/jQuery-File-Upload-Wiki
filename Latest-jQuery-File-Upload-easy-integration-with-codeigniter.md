** Updated for version 9.5.x (April 2014) **

Its very easy. No need to change anything in Codeigniter or modify the jQuery-File-Upload library (meaning that upgrades will be very simple). Steps:

1. Place UploadHandler.php into ~/application/libraries/. No modifications required.
2. Create fileupload.php in ~/application/controllers/ with the content below.
3. Create ~/files and ~/thumbnails with appropriate permissions for the webserver to write to the directories.
4. Configure your client side file upload script to point to '/fileupload' (or whatever your path is to this Codeigniter controller).

Contents of ~/application/controllers/fileupload.php :
```
<?php if (!defined('BASEPATH')) exit('No direct script access allowed');

class Fileupload extends CI_Controller
{
    function __construct()
    {
        parent::__construct();
    }

    function index()
    {
        error_reporting(E_ALL | E_STRICT);
        $this->load->library("UploadHandler");
    }
}
```


Note that '~' above denotes the Codeigniter root directory.
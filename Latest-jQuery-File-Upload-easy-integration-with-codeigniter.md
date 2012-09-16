Its very easy. no need to make a constant file etc.

just place your upload.class.php into applications/libraries as uploadhandler.php and autoload in autoload.php in config dir.

Now you can use its instance as $this->uploadhandler in controllers. now make a function do_upload in your controller and paste the contents of server/php/index.php in that function and remove require('upload.class.php'); as well as don't make instance of upload_handler because you are already have this as $this->uploadhandler. so user it and enjoy!.

and don't forget to place 2 directories files and thumbnails in your codeigniter root dir.
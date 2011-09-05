jQuery-File-Upload allows cross domain requests.

We can modify the example to demonstrate this feature.

To allow cross domain uploads modify example/upload.php near the top:

    'script_url' => "http://<MY IP/DOMAIN HERE>".$_SERVER['PHP_SELF'],
    'upload_url' => "http://<MY IP/DOMAIN HERE>/".dirname($_SERVER['PHP_SELF']).'/files/',

and a few lines later:

    'thumbnail' => array(
        'upload_url' => "http://<MY IP/DOMAIN HERE>/".dirname($_SERVER['PHP_SELF']).'/thumbnails/',

also add:

    case 'OPTIONS':
        break;

on the supported request methods at the end of upload.php.

You also need to add Header set Access-Control-Allow-Origin * to your "upload server". For documentation on how to do this for the apache server check this page: [[http://enable-cors.org/#how-apache]]. You will also find how to do it for PHP and other servers on the same page.

Don't forget to ensure that you have GD enabled and suitable directory permissions on the files and thumbnails directories.

Tested with Firefox 6.0.1, Chrome 13.0 and IE6 (it uploads successfully but gives some Javascript error – known limitation)

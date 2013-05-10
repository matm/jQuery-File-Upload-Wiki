this would be the index.php

<?php
/*
 * jQuery File Upload Plugin PHP Example for S3
 * https://github.com/blueimp/jQuery-File-Upload
 *
 * Copyright 2012, Roberto Colonello
 * http://www.parsec.it
 *
 * Licensed under the MIT license:
 * http://www.opensource.org/licenses/MIT
 */

error_reporting(E_ALL | E_STRICT);

require('awssdk.php');
$bucket = "YOUR BUCKET NAME";
$subFolder = "";  // leave blank for upload into the bucket directly


header('Pragma: no-cache');
header('Cache-Control: no-store, no-cache, must-revalidate');
header('Content-Disposition: inline; filename="files.json"');
header('X-Content-Type-Options: nosniff');
header('Access-Control-Allow-Origin: *');
header('Access-Control-Allow-Methods: OPTIONS, HEAD, GET, POST, PUT, DELETE');
header('Access-Control-Allow-Headers: X-File-Name, X-File-Type, X-File-Size');


switch ($_SERVER['REQUEST_METHOD']) {
    case 'OPTIONS':
        break;
    case 'HEAD':
    case 'GET':
       $files=array("files"=>getListOfContents($bucket, $subFolder));
        echo json_encode($files);
        break;
    case 'POST':
        if (isset($_REQUEST['_method']) && $_REQUEST['_method'] === 'DELETE') {
            $files=array("files"=>deleteObject($bucket, $subFolder));
			echo json_encode($files);
        } else {
			$files=array("files"=>uploadFiles($bucket, $subFolder));
        	echo json_encode($files);
        }
        break;
    case 'DELETE':
         echo json_encode(deleteFiles($bucket, $subFolder));
        break;
    default:
        header('HTTP/1.1 405 Method Not Allowed');
}


This would be the awssdk.php

<?php
/*
 * jQuery File Upload Plugin Server PHP for S3 Amazon
 *
 * Copyright 2012, Roberto Colonello
 * http://www.parsec.it
 *
 * Licensed under the MIT license:
 * http://www.opensource.org/licenses/MIT
 */
 
require_once 'sdk.class.php';
$s3 = new AmazonS3();


function getFileInfo($bucket, $fileName) {
    global $s3;
    $fileArray = "";
    $size = $s3->get_object_filesize($bucket, $fileName);
    $furl = "https://" . $bucket . ".s3.amazonaws.com/" . $fileName;
    $fileArray['name'] = $fileName;
    $fileArray['size'] = $size;
    $fileArray['url'] = $furl;
    //$fileArray['thumbnail_url'] = $furl;
    $fileArray['delete_url'] = "server/aws/index.php?file=" . $fileName;
    $fileArray['delete_type'] = "DELETE";
    return $fileArray;
}

function getListOfContents($bucket, $prefix="") {
    global $s3;
    if ($prefix=="") {
      $contents = $s3->get_object_list($bucket);
    } else {
    	$contents = $s3->get_object_list($bucket, array("prefix" => $prefix));
    }
    $resultArray = "";
    for ($i = 0;$i < count($contents);$i++) {
        $resultArray[] = getFileInfo($bucket, $contents[$i]);
    }
    return $resultArray;
}

function uploadFiles($bucket, $prefix="") {
    global $s3;
    if (isset($_REQUEST['_method']) && $_REQUEST['_method'] === 'DELETE') {
        return "";
    }
    $upload = isset($_FILES['files']) ? $_FILES['files'] : null;
    $info = array();
    if ($upload && is_array($upload['tmp_name'])) {
        foreach($upload['tmp_name'] as $index => $value) {
            $fileTempName = $upload['tmp_name'][$index];
			//$fileTempContentType = $upload['type'];
			
            $fileName = (isset($_SERVER['HTTP_X_FILE_NAME']) ? $_SERVER['HTTP_X_FILE_NAME'] : $upload['name'][$index]);
            $fileName = $prefix.str_replace(" ", "_", $fileName);
            $response = $s3->create_object($bucket, $fileName, array('fileUpload' => $fileTempName, 'acl' => AmazonS3::ACL_PRIVATE, 'meta' => array('keywords' => 'example, test'),));
            if ($response->isOK()) {
                $info[] = getFileInfo($bucket, $fileName);
            } else {
                //     echo "<strong>Something went wrong while uploading your file... sorry.</strong>";   
            }
        }
    } else {
        if ($upload || isset($_SERVER['HTTP_X_FILE_NAME'])) {
            $fileTempName = $upload['tmp_name'];
			//$fileTempContentType = $upload['type'];
			
            $fileName = (isset($_SERVER['HTTP_X_FILE_NAME']) ? $_SERVER['HTTP_X_FILE_NAME'] : $upload['name']);
            $fileName =  $prefix.str_replace(" ", "_", $fileName);
            $response = $s3->create_object($bucket, $fileName, array('fileUpload' => $fileTempName, 'acl' => AmazonS3::ACL_PRIVATE, 'meta' => array('keywords' => 'example, test'),));
            if ($response->isOK()) {
                $info[] = getFileInfo($bucket, $fileName);
            } else {
                //     echo "<strong>Something went wrong while uploading your file... sorry.</strong>";
                
            }
        }
    }
    header('Vary: Accept');
    $json = json_encode($info);
    $redirect = isset($_REQUEST['redirect']) ? stripslashes($_REQUEST['redirect']) : null;
    if ($redirect) {
        header('Location: ' . sprintf($redirect, rawurlencode($json)));
        return;
    }
    if (isset($_SERVER['HTTP_ACCEPT']) && (strpos($_SERVER['HTTP_ACCEPT'], 'application/json') !== false)) {
        header('Content-type: application/json');
    } else {
        header('Content-type: text/plain');
    }
    return $info;
}

function deleteFiles($bucket, $prefix="") {
global $s3;
$file_name = $prefix.(isset($_REQUEST['file']) ? basename(stripslashes($_REQUEST['file'])) : null);
$s3->delete_object($bucket, $file_name);
$success = "";
    
    header('Content-type: application/json');
    return $success;
}
?>
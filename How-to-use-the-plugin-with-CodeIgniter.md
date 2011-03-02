## Controller :
```php
<?php if ( ! defined('BASEPATH')) exit('No direct script access allowed');

class Upload extends CI_Controller {

	public function __construct()
	{
		parent::__construct();
		$this->load->helper(array('form', 'url'));
	}

	public function index()
	{
		$this->load->view('form', array('error' => ''));
	}

	public function do_upload()
	{
		$config['upload_path'] = FCPATH.'uploads/';
		$config['allowed_types'] = 'gif|jpg|png|jpeg';
		$config['max_size'] = '36000';
		$config['max_width'] = '10240';
		$config['max_height'] = '76800';

		$this->load->library('upload', $config);

		if ( ! $this->upload->do_upload())
		{
			$error = array('error' => $this->upload->display_errors());

			$this->load->view('form', $error);
		}
		else
		{
			$data['upload_data'] = $this->upload->data();
			$this->load->view('success', $data);
		}
	}
}
// End of file upload

```

## Form View :
```html
<html>
	<head>
	<title>Upload Form</title>
	<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.9/themes/base/jquery-ui.css" id="theme">
	<link rel="stylesheet" href="http://dl.dropbox.com/u/8626323/dnd/jquery.fileupload-ui.css">
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
		<?php echo form_open_multipart('upload/do_upload', array('id' => 'file_upload')); ?>
			<input type="file" name="userfile" multiple />
			<button>Upload</button>
			<div>Upload files</div>
		<?php echo form_close(); ?>
		<table id="files"></table>
			<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.5.0/jquery.min.js"></script>
			<script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.9/jquery-ui.min.js"></script>
			<script src="../jquery.fileupload.js"></script>
			<script src="../jquery.fileupload-ui.js"></script>
			<script>
			/*global $ */
			$(function () {
				$('#file_upload').fileUploadUI({
				uploadTable: $('#files'),
				downloadTable: $('#files'),
				buildUploadRow: function (files, index) {
				return $('<tr><td>' + files[index].name + '<\/td>' +
					'<td class="file_upload_progress"><div><\/div><\/td>' +
					'<td class="file_upload_cancel">' +
					'<button class="ui-state-default ui-corner-all" title="Cancel">' +
					'<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
					'<\/button><\/td><\/tr>');
				},
				buildDownloadRow: function (file) {
					return $('<tr><td>' + file.name + '<\/td><\/tr>');
				}
				});
			});
			</script> 
	</body>
</html>
```

## Success View :
```php
<?php
echo '{"name":"'.$upload_data['file_name'].'","type":"'.$upload_data['file_type'].'","size":"'.$upload_data['file_size'].'"}';
?>
```
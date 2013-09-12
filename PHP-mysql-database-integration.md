## SQL code to for the MySQL files table:

```sql
CREATE TABLE `files` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `size` int(11) DEFAULT NULL,
  `type` varchar(255) DEFAULT NULL,
  `url` varchar(255) DEFAULT NULL,
  `title` varchar(255) DEFAULT NULL,
  `description` text,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## Extension of index.php for the database integration:

```php
<?php
$options = array(
    'delete_type' => 'POST',
    'db_host' => 'localhost',
    'db_user' => 'root',
    'db_pass' => '',
    'db_name' => 'example',
    'db_table' => 'files'
);

error_reporting(E_ALL | E_STRICT);
require('UploadHandler.php');

class CustomUploadHandler extends UploadHandler {

    protected function initialize() {
    	$this->db = new mysqli(
    		$this->options['db_host'],
    		$this->options['db_user'],
    		$this->options['db_pass'],
    		$this->options['db_name']
    	);
        parent::initialize();
        $this->db->close();
    }

    protected function handle_form_data($file, $index) {
    	$file->title = @$_REQUEST['title'][$index];
    	$file->description = @$_REQUEST['description'][$index];
    }

    protected function handle_file_upload($uploaded_file, $name, $size, $type, $error,
            $index = null, $content_range = null) {
        $file = parent::handle_file_upload(
        	$uploaded_file, $name, $size, $type, $error, $index, $content_range
        );
        if (empty($file->error)) {
			$sql = 'INSERT INTO `'.$this->options['db_table']
				.'` (`name`, `size`, `type`, `title`, `description`)'
				.' VALUES (?, ?, ?, ?, ?)';
	        $query = $this->db->prepare($sql);
	        $query->bind_param(
	        	'sisss',
	        	$file->name,
	        	$file->size,
	        	$file->type,
	        	$file->title,
	        	$file->description
	        );
	        $query->execute();
	        $file->id = $this->db->insert_id;
        }
        return $file;
    }

    protected function set_additional_file_properties($file) {
        parent::set_additional_file_properties($file);
        if ($_SERVER['REQUEST_METHOD'] === 'GET') {
        	$sql = 'SELECT `id`, `type`, `title`, `description` FROM `'
        		.$this->options['db_table'].'` WHERE `name`=?';
        	$query = $this->db->prepare($sql);
 	        $query->bind_param('s', $file->name);
	        $query->execute();
	        $query->bind_result(
	        	$id,
	        	$type,
	        	$title,
	        	$description
	        );
	        while ($query->fetch()) {
	        	$file->id = $id;
        		$file->type = $type;
        		$file->title = $title;
        		$file->description = $description;
    		}
        }
    }

    public function delete($print_response = true) {
        $response = parent::delete(false);
        foreach ($response as $name => $deleted) {
        	if ($deleted) {
	        	$sql = 'DELETE FROM `'
	        		.$this->options['db_table'].'` WHERE `name`=?';
	        	$query = $this->db->prepare($sql);
	 	        $query->bind_param('s', $name);
		        $query->execute();
        	}
        } 
        return $this->generate_response($response, $print_response);
    }

}

$upload_handler = new CustomUploadHandler($options);
```

## Client-side file upload initialization:

```js
$('#fileupload').fileupload({
    url: 'server/php/'
}).on('fileuploadsubmit', function (e, data) {
    data.formData = data.context.find(':input').serializeArray();
});
```

## Client side upload template additions:

```html
<label class="title">
    <span>Title:</span><br>
    <input name="title[]" class="form-control">
</label>
<label class="description">
    <span>Description:</span><br>
    <input name="description[]" class="form-control">
</label>
```

## Client side download template additions:

```html
<p class="title"><strong>{%=file.title||''%}</strong></p>
<p class="description">{%=file.description||''%}</p>
```
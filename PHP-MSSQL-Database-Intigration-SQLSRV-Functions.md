$options = array(
	'db_table' => 'files',
	'db_name' => 'DB',
	'db_user' => 'UserName',
	'db_password' => 'Password'
	);
error_reporting(E_ALL | E_STRICT);
require('UploadHandler.php');

class CustomUploadHandler extends UploadHandler {

    protected function initialize() {
    	$connectionInfo = array("UID" => $this->options['db_user'], "PWD" =>$this->options['db_password'], "Database"=>$this->options['db_name']);
		$serverName = "(local)";
		$conn=sqlsrv_connect( $serverName, $connectionInfo);
		if($conn) {
			$this->db = $conn;
		}else{
			/* Error logging for php-errors.log*/
			error_log(__FUNCTION__);
			error_log("Connection could not be established.");
			$err=print_r( sqlsrv_errors(), true);
        	error_log($err);
		}
        parent::initialize();
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
			$sql = 'INSERT INTO '.$this->options['db_table']
				.' (name, size, type, title, description)'
				.' VALUES (?, ?, ?, ?, ?)';
				$params=array($file->name,
	        	$file->size,
	        	$file->type,
	        	$file->title,
	        	$file->description);
	        $stmt = sqlsrv_query ($this->db,$sql,$params);
	    		/* Error logging for php-errors.log*/
		        if( $stmt === false ) {
		        	error_log(__FUNCTION__);
		        	$err=print_r( sqlsrv_errors(), true);
		        	error_log($err);
		        	return die( print_r( sqlsrv_errors(), true));
		        }
			sqlsrv_next_result($stmt);
			sqlsrv_fetch($stmt);
			$file->id = sqlsrv_get_field($stmt, 0);
	       	sqlsrv_free_stmt( $stmt);
        }
        return $file;
    }

    protected function set_additional_file_properties($file) {
        parent::set_additional_file_properties($file);
        if ($_SERVER['REQUEST_METHOD'] === 'GET') {
        	$sql = 'SELECT id, type, title, description FROM '
        		.$this->options['db_table'].' WHERE name=?';
        	$params=array($file->name);
        	$stmt = sqlsrv_query ($this->db,$sql,$params);
			if( $stmt === true ){
				while( $row = sqlsrv_fetch_array( $stmt, SQLSRV_FETCH_ASSOC)) {
					$file->id = $row['$id'];
	        		$file->type = $row['$type'];
	        		$file->title = $row['$title'];
	        		$file->description = $row['$description'];
				}
				sqlsrv_free_stmt( $stmt);
			}
        }
    }

    public function delete($print_response = true) {
        $response = parent::delete(false);
        foreach ($response as $name => $deleted) {
        	if ($deleted) {
	        	$sql = 'DELETE FROM '
	        		.$this->options['db_table'].' WHERE name=?';
	        	$params=array($file->name);
        		$stmt = sqlsrv_query ($this->db,$sql,$params);
        		sqlsrv_free_stmt( $stmt);
        	}
        } 
        return $this->generate_response($response, $print_response);
    }

}

$upload_handler = new CustomUploadHandler($options);
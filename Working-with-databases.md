All changes and modifications must be made ​​in the **upload.class.php**
## General settings

First, set your database connection details in the options area:

_Search this line - > **$this->options = array(**_ 
   
 Add the following Code in the next lines :

>// mysql connection settings  
>	        'database' => **YOUR DATABASE**,  
>	        'host' => **localhost**,  
>	        'username' => **YOUR USERNAME**,  
>	        'password' => **YOUR PASSWORD**,  
>// end

So now you have to write a function for the SQL Query, copy & paste the following code for example after the **handle_file_upload** function :

>	function query($query) {  
>	$database = $this->options['database'];  
>	$host = $this->options['host'];  
>	$username = $this->options['username'];  
>	$password = $this->options['password'];  
>	$link = mysql_connect($host,$username,$password);  
>	if (!$link) {  
>	die(mysql_error());  
>	}  
>	$db_selected = mysql_select_db($database);  
>	if (!$db_selected) {  
>	die(mysql_error());  
>	}  
>	$result = mysql_query($query);  
>	return $result;  
>	mysql_close($link);  
>	}  

I complete this wiki page next time...


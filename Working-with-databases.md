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

## Add file details to database
I explain this function with a picture upload, so here we save the picture name to the database
Add this function also too the **upload.class.php** 

>	function add_img($whichimg)  
>        {  
>	$add_to_db = $this->query("INSERT INTO **yourtable** (**yourcolumnone**) VALUES     ('".$whichimg."')") or die(mysql_error());  
>	return $add_to_db;  
>	}  

so in this function we call the function _query_ with the string between the clamps. You could also insert other details too, for example, the file size.

At least we have to call this function, with the following code at the end of the function **handle_file_upload**.
Paste the following code underneath or over this line : **$file->size = $file_size;**  
>$file->upload_to_db = $this->add_img($file->name);

## Delete the entry we created 
Deleting the entry we made before is very easy, we create a new function which makes also an sql query to delete it.  
>	function delete_img($delimg)  
>	{  
>		$delete_from_db = $this->query("DELETE FROM **yourtable** WHERE **yourcolumnone** = '$delimg'") or die(mysql_error());  
>		return $delete_from_db;  
>	}  

Now we must call the function, this time of the delete function.  
Go to the delete function and search this line : **if ($success) {** paste the following code over this.  
>$this->delete_img($file_name);

_If you find a mistake or want to provide feedback, just send me a message :)_
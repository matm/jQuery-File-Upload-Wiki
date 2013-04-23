If you allow users to upload files to your web server, you have a potential security hole.  
To prevent an attacker from executing arbitrary files in the context of your web application, some security considerations have to be kept in mind.

For an in-depth understanding of the potential security risks and possible mitigations, the [OWASP project](https://www.owasp.org/) has a good overview:

**[Unrestricted File Upload (OWASP)](https://www.owasp.org/index.php/Unrestricted_File_Upload)**


Following are some security considerations specific to the jQuery File Upload plugin:

## Client-side
File upload restrictions on client-side are only means to improve the usability of your application.
The client-side is under the control of any potential attacker, therefore you cannot trust any client-side input and absolutely have to implement validations and restrictions on server-side.

You also need to keep in mind files that can be downloaded and executed on client-side in the context of another user to exploit [Cross-site scripting (XSS)](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS\)) vulnerabilities or spread malware. As this can only be controlled on server-side, it's part of the next section.

## Server-side

To prevent arbitrary files (e.g. JavaScript files) from being executed on client-side in the context of the web application, the provided server-side implementations serve non-image files with attachment headers, to force a download dialog:

```
Content-Disposition:attachment; filename=script.js;
```

Image files are served with their proper mime type headers (e.g. "image/png") and an additional header to [prevent Internet Explorer 8 and higher from sniffing the mime type](http://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx) and executing JavaScript masked as image files:

```
X-Content-Type-Options: nosniff
```

Please note that all provided implementations allow cross-site file uploads. If you don't need that functionality, you should remove the code parts that set the "Access-Control-Allow" headers.

Please also note that the provided implementations allow anyone to upload files. The uploaded files are also accessible for anyone to download. It's up to you to incorporate the server-side file upload implementation into your user-authentication system.

### Google App Engine (Python, Go)
On Google App Engine, uploaded files cannot be saved directly on the file system and are instead stored in the Blobstore. This prevents executing uploaded files (e.g. Python scripts) on server-side in the context of your web application.  
The provided Python and Go implementations also restrict uploaded files by default to image files (GIF, JPEG, PNG).

### Node.js
The Node.js implementation serves uploaded files with a static file server, which - as the name implies - only serves static files and doesn't interpret any uploaded scripts (as Apache would do e.g. with PHP scripts with the PHP module installed).  
The Node.js implementation also restricts uploaded files by default to image files (GIF, JPEG, PNG).

### PHP
The PHP implementation allows to upload all file types by default. Since you probably don't need that functionality, it is recommended to adjust the **accept_file_types** option. If you're running PHP >= 5.3, you can override this option in the upload handler instantiation, else you have to edit the appropriate line in the upload class itself:

```php
$upload_handler = new UploadHandler(array(
    'accept_file_types' => '/\.(gif|jpe?g|png)$/i'
));
```

Please note that the **accept_file_types** option isn't meant to prevent attackers from uploading and executing PHP scripts. This is done with a **.htaccess** directive explained in the following paragraphs.

The PHP implementation stores uploaded files in the pre-defined directory "files". This directory contains a file **.htaccess** that is absolutely required as it contains directives for Apache to enforce security restrictions:

```
ForceType application/octet-stream
<FilesMatch "(?i)\.(gif|jpe?g|png)$">
  ForceType none
</FilesMatch>

# Prevent IE from MIME-sniffing:
Header set X-Content-Type-Options "nosniff"
```

The [ForceType](http://httpd.apache.org/docs/2.4/mod/core.html#forcetype) directive "application/octet-stream" enforces Apache to serve all files with an attachment header to force a download dialog. More importantly, it prevents Apache to run any of the uploaded files through an interpreter like PHP, even if the file extension is ".php".

### HTTP Basic Authentication
Although HTTP Basic Authentication (e.g. via **.htaccess**) is a simple means to add password protection, it's advisable to use Cookie based authentication instead, since IE does seem to have problems with HTTP Basic Authentication and invisible Iframes, which are used for the Iframe Transport.  
See also Issue [#1264](https://github.com/blueimp/jQuery-File-Upload/issues/1264).
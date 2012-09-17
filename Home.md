# Documentation Overview

## Basic plugin information
* [Demo page](http://blueimp.github.com/jQuery-File-Upload/) (and [[Demo implementation]] information).
* How to [[Setup]] the plugin on your website.
* How to use only the [[Basic plugin]] (minimal setup guide).
* **[[Security]] considerations.**
* **The plugin [[API]].**
* **List of all available [[Options]] - including events and callback methods.**
* Explanation of all [[Plugin files]].
* Extended [[Browser support]] information.
* [[Frequently Asked Questions]] (FAQ)

## Customization how-tos
* **[[Plugin extensions]] (Developer Guide)**
* How to implement a different [[Template Engine]].
* [[Performance Optimizations]] to speed up page load times.
* [[Style Guide]] with explanations for the provided [CSS code](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/jquery.fileupload-ui.css).
* Using the plugin with [[jQuery UI]].
* [[How to submit additional Form Data]].
* How to [[submit files asynchronously]].
* [[Upload multiple resolutions of one image with multiple resize options]]
* [[Multiple File Upload Widgets on the same page]]
* [[Multiple File Input Fields in one Form]].
* How to implement [[Chunked file uploads]].
* [[Cross domain uploads]] (Cross-site uploads).
* How to add [[Drop zone effects]].
* How to display [[Extended progress information]].
* [[Drag and drop uploads from another web page]]
* [[Fixing Safari hanging on very high speed connections (1Gbps)]]
* Direct upload to [[YouTube]]
* [[Working with Databases]]

## Server-side

### Python (Google App Engine, Django, web2py)
* How to use the plugin with [[Google App Engine]] (Python).
* Django integration [tutorial](http://garmoncheg.blogspot.com/2011/07/django-creating-multi-upload-form.html) and [demo code](https://github.com/garmoncheg/django_multiuploader_demo) (user contribution).
* Same [Django reusable Plugin](https://github.com/garmoncheg/django_multiuploader) and [article](http://garmoncheg.blogspot.com/2011/07/django-beautiful-multiple-files-upload.html) (user contribution).
* Another [Django integration](https://github.com/sigurdga/django-jquery-file-upload) (user contribution).
* Django >= 1.2.5 integration with CSRF enabled [repo](https://github.com/miki725/Django-jQuery-File-Uploader-Integration-demo) [wiki](https://github.com/miki725/Django-jQuery-File-Uploader-Integration-demo/wiki) (user contribution).
* [web2py (Python framework) port](https://github.com/hellais/jQuery-File-Upload) (user contribution).
* [Pyramid demo port](https://github.com/grooverdan/pyramid-jQuery-File-Upload-demo) (user contribution).

### Go
* [`jfu` package](https://github.com/jmcvetta/jfu) for integration with Go applications, backed by MongoDB + Memcache  (user contribution).

### Java
* Code samples for [[Google App Engine Java]] (user contribution).
* [File Upload with Spring and jQuery] (http://krams915.blogspot.com/2012/06/file-upload-with-spring-and-jquery-part_2793.html) (user contribution).
* [Upload Servlet excercise](https://github.com/klaalo/jQuery-File-Upload-Java) (user contribution).

### ASP (Classic)
* [ASP VBscript example](https://github.com/blueimp/jQuery-File-Upload/wiki/Classic-ASP) (user contribution)

### ASP.NET
* Fork of this project adding an [ASP.NET server example](https://github.com/timabell/jQuery-File-Upload/tree/dotnet) (contribution from Tim Abell), an IHttpHandler based example. Upload only.
 * Usage information is available in [the fork's readme](/timabell/jQuery-File-Upload/tree/dotnet/server/dotnet#readme)
* [ASP.NET example code](https://github.com/blueimp/jQuery-File-Upload/wiki/Complete-code-example-using-blueimp-jQuery-file-upload-control-in-Asp.Net.) (contribution from Sam at webtrendset.com), a simple ashx based example, available as a zip file download. Upload support only. Focus on IE handling.
* An [ASP.NET MVC 3 example](https://github.com/maxpavlov/jQuery-File-Upload.MVC3) with the same functionality as an original plugin demo + large files upload support for ASP.NET (contribution from Max Pavlov)
* A fully functional github hosted [ASP.NET example](https://github.com/i-e-b/jQueryFileUpload.Net) (contribution by Iain Ballard), based on ashx handlers. Provides full upload / delete / thumbnail functionality.
* [.NET File Upload Handler](https://github.com/swhitley/jQuery-File-Upload) implementation by [Shannon Whitley](https://github.com/swhitley)
* [ASP.NET Multiple File Upload With Drag & Drop and Progress Bar Using HTML5](http://www.codeproject.com/Articles/460142/ASP-NET-Multiple-File-Upload-With-Drag-Drop-and-Pr)

### Ruby (on Rails)
* [S3 Uploader Rails Example](https://github.com/ncri/s3_uploader_example) (user contribution).
  Is able to use XHR uploads directly to S3, despite S3 not supporting `Access-Control-Allow-Origin`, by hosting an (iframed) upload form on S3 itself (tricksy!) 
* Using the plugin to [[Upload directly to S3]] (user contribution).
  Uses the iframe-transport fallback, so lacks niceties like the progress bar
* Rails + (Carrierwave || Dragonfly) [[Rails-setup-for-V6]] (user contribution)
* [Rails 3.1.3 without asset-pipeline demo app](https://github.com/banditj/fily/tree/no-asset-pipeline) (user contribution)
* [Rails 3.2.8 wit asset-pipeline demo app](https://github.com/jalagrange/bootstrap_uploader) (Forked and improved from banditj user contribution)

### PHP
* [Beautiful HTML5 File Uploads using CakePHP and jQuery File Upload](http://blog.creativeideal.net/cakephp/beautiful-html5-file-uploads-using-cakephp-and-jquery) (user contribution)
* [jQuery File Upload + CakePHP 2.1.x](https://github.com/hugodias/FileUpload) (user contribution)
* [Source for Amazon S3 Upload with PHP SDK](https://s3.amazonaws.com/parsec_it_examples/s3-php.zip) by [roberto colonello](https://github.com/robertocolonello)
* [[Zend-framework]] integration (user contribution).
* [[jQuery-File-Upload-6.5-with-CodeIgniter-2.1]] (user contribution)
* [[jQuery-File-Upload,---Multi-file-upload-with-CodeIgniter]] (user contribution)
* [[PHP Subfolder example]]
* [[PHP Session Upload Progress]]
* Database integration ([[Working with Databases]])  

## Documentation for v.4 (old branch)

### Basic plugin information
* How to [[Setup v4]] the plugin on your website.
* The plugin [[API v4]].
* List of all available [[Options v4]] for this plugin.

### Customization how-tos
* [[How to queue files and start uploads with a button click]] v4.
* [[How to implement File Limits]] v4.
* [[How to submit additional Form Data v4]].
* [[Multiple File Input Fields in one Form v4]].
* [[Multiple plugin instances on the same page]] v4.
* [[Sequential Uploads]] v4.
* [[How to upload multiple files with one request]] v4.
* [[How to invoke a method after all uploads have completed]] v4.
* [[Chunked Uploads v4]].

### Server-side specific tutorials
* How to use the plugin with the [[Flask]] Python Microframework (user contribution)
* [[jQuery-File-Upload-v4-for-Rails-3]]: basic Rails 3 tutorial (user contribution)
* [Sample app using Rails 3, carrierwave and jquery-file-upload to upload and store files on Amazon S3](https://github.com/yortz/carrierwave_jquery_file_upload) (user contribution)
* [[jQuery File Upload for Java]]: basic Java application (user contribution)
* [[How to use the plugin with CodeIgniter]] (PHP) (user contribution)
* [[jQuery File Upload for C# (Forms)]]: basic C# Forms application (user contribution)
* C# example on [[How to populate the file list on page load or between page refresh]] (user contribution)
* [How to use the plugin with Yii framework](http://www.yiiframework.com/extension/xupload/) (PHP) (user contribution)
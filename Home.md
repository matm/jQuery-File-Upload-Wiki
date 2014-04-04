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
* [[How to submit additional Form Data]].
* How to [[submit files asynchronously]].
* How to do [[Client side Image Resizing]].
* [[Upload multiple resolutions of one image with multiple resize options]].
* [[Multiple File Upload Widgets on the same page]]
* [[Multiple File Input Fields in one Form]].
* How to implement [[Chunked file uploads]].
* [[Cross domain uploads]] (Cross-site uploads).
* How to add [[Drop zone effects]].
* How to display [[Extended progress information]].
* [[Drag and drop uploads from another web page]]
* [[Fixing Safari hanging on very high speed connections (1Gbps)]]
* Direct upload to [[YouTube]]

## Server-side

### Nginx
* [[Uploading to nginx using the nginx upload module]]
* [[Uploading-to-nginx-using-the-nginx-upload-module-with-php_handler]]

### PHP
* [[PHP user directories]]
* [[PHP Session Upload Progress]]
* [[PHP Remote File Copy]]
* [[PHP MySQL database integration]]
* [Beautiful HTML5 File Uploads using CakePHP and jQuery File Upload](http://blog.creativeideal.net/cakephp/beautiful-html5-file-uploads-using-cakephp-and-jquery) (user contribution)
* [jQuery File Upload + CakePHP 2.1.x](https://github.com/hugodias/FileUpload) (user contribution)
* [Source for Amazon S3 Upload with PHP SDK version 1 ](https://s3.amazonaws.com/parsec_it_examples/s3-php.zip) by [roberto colonello](https://github.com/robertocolonello)
* [Source for Amazon S3 Upload with PHP SDK version 2 ](https://gist.github.com/tim-peterson/8172999) by [Tim Peterson](https://github.com/tim-peterson)
    * [AWS SDK PHP Version 2](http://docs.aws.amazon.com/aws-sdk-php/guide/latest/service-s3.html#amazon-s3-stream-wrapper) allows using built-in PHP file handling functions such as `file_get_contents()` on S3 buckets
    * This version of UploadHandler.php also provides the option to accept [`formData`](https://github.com/blueimp/jQuery-File-Upload/wiki/Options#formdata) of `$_POST['files']` selected via [Dropbox Chooser](https://www.dropbox.com/developers/dropins/chooser/js)
* [[Zend-framework]] integration (user contribution).
* [[jQuery-File-Upload-6.5-with-CodeIgniter-2.1]] (user contribution)
* [[jQuery-File-Upload,---Multi-file-upload-with-CodeIgniter]] (user contribution)
* [[jQuery-File-Upload 8.8.5,with-CodeIgniter 2.1.X]] (user contribution)
* [[jQuery File Upload 8.9 with CodeIgniter 2.1.4]] (user contribution)
* [[jQuery File Upload 9.5 with CodeIgniter 2.1.4]] (user contribution)
* [Symfony2 bundle](https://github.com/punkave/symfony2-file-uploader-bundle) (user contribution)
* [Yii extension](https://github.com/Asgaroth/xupload) (user contribution)
* [Open Manager](https://github.com/rmorse/Open-Manager), a File/Media Manager implementation.
* [Fileupload](https://github.com/Gargron/fileupload) library (Composer-installable, framework-independent)

### Python
* How to use the plugin with [[Google App Engine]].
* Django integration [tutorial](http://garmoncheg.blogspot.com/2011/07/django-creating-multi-upload-form.html) and [demo code](https://github.com/garmoncheg/django_multiuploader_demo) (user contribution).
* Same [Django reusable Plugin](https://github.com/garmoncheg/django_multiuploader) and [article](http://garmoncheg.blogspot.com/2011/07/django-beautiful-multiple-files-upload.html) (user contribution).
* Another [Django integration](https://github.com/sigurdga/django-jquery-file-upload) (user contribution).
* Django >= 1.2.5 integration with CSRF enabled [repo](https://github.com/miki725/Django-jQuery-File-Uploader-Integration-demo) [wiki](https://github.com/miki725/Django-jQuery-File-Uploader-Integration-demo/wiki) (user contribution).
* [django-jfu](https://github.com/Alem/django-jfu) - A Django re-usable app for integrating jQuery File Upload into existing projects (not a demo) (user contribution).
* [web2py (Python framework) port](https://github.com/hellais/jQuery-File-Upload) (user contribution).
* [Pyramid demo port](https://github.com/grooverdan/pyramid-jQuery-File-Upload-demo) (user contribution).
* [collective.upload - Plone integration](https://github.com/collective/collective.upload) (user contribution).

### Ruby
* [S3 Uploader Rails Example](https://github.com/ncri/s3_uploader_example) (user contribution).
  Is able to use XHR uploads directly to S3, despite S3 not supporting `Access-Control-Allow-Origin`, by hosting an (iframed) upload form on S3 itself (tricksy!) 
* Using the plugin to [[Upload directly to S3]] (user contribution).
  Uses the iframe-transport fallback, so lacks niceties like the progress bar
* Using the plugin to [upload directly to S3](http://pjambet.github.com/blog/direct-upload-to-s3/) with new CORS support
* Rails + (Carrierwave || Dragonfly) [[Rails-setup-for-V6]] (user contribution)
* [Rails 3.1.3 without asset-pipeline demo app](https://github.com/banditj/fily/tree/no-asset-pipeline) (user contribution)
* [Rails 3.2.8 with asset-pipeline demo app](https://github.com/jalagrange/bootstrap_uploader) (Forked and improved from banditj user contribution)
* [Screencast 381 - jQuery File Upload](http://railscasts.com/episodes/381-jquery-file-upload) (Ryan Bates, RailsCasts)
* [Screencast 383 - Uploading to Amazon S3](http://railscasts.com/episodes/383-uploading-to-amazon-s3) (Ryan Bates, RailsCasts)
* [Rails + Blitline (upload directly to S3 + process images into thumbnails)](https://github.com/hisyam/blitline_rails_demo)
* [jquery-fileupload-rails Gem](https://github.com/tors/jquery-fileupload-rails) and [example](https://github.com/tors/jquery-fileupload-rails-paperclip-example)
* [s3_cors_fileupload Gem](https://github.com/batter/s3_cors_fileupload) - Provides easy solution for using the plugin with CORS to upload files to Amazon S3.

### Perl
* [jQuery::File::Upload perl module](https://metacpan.org/module/jQuery::File::Upload)

### JavaScript
* [Express.js middleware for jQuery File Upload](https://github.com/aguidrevitch/jquery-file-upload-middleware) by [Aleksandr Guidrevitch](https://github.com/aguidrevitch)
* [Cup](https://github.com/aparajita/Cup), a Cappuccino wrapper for jQuery File Upload.

### Go
* [jfu package](https://github.com/jmcvetta/jfu) for integration with Go applications, backed by MongoDB + Memcache  (user contribution).

### Java
* Code samples for [[Google App Engine Java]] (user contribution).
* [File Upload with Spring and jQuery] (http://krams915.blogspot.com/2012/06/file-upload-with-spring-and-jquery-part_2793.html) (user contribution).
* [Upload Servlet excercise](https://github.com/klaalo/jQuery-File-Upload-Java) (user contribution).
* [Modified Upload Servlet] (https://github.com/amilarajans/jQuery-File-Upload-for-java) complete working example (user contribution).
* [Upload to Spring MVC] (http://hmkcode.com/spring-mvc-jquery-file-upload-multiple-dragdrop-progress/) (user contribution).
* [Upload to Servlet 3.0 `request.getParts()`/ Apache FileUpload] (http://http://hmkcode.com/java-servlet-jquery-file-upload/) (user contribution).
* [Basic Plus UI implementation with Spring MVC 3.2 (Java Configuration) & Hibernate 4] (https://github.com/jdmr/fileUpload) complete working example (user contribution).

### Scala
* The _Lift Cookbook_ contains [basic plugin integration](http://cookbook.liftweb.net/#AjaxFileUpload) code and instructions (user contribution).

### ASP.NET
* [ASP.NET example code](https://github.com/blueimp/jQuery-File-Upload/wiki/Complete-code-example-using-blueimp-jQuery-file-upload-control-in-Asp.Net.) (contribution from Sam at webtrendset.com), a simple ashx based example, available as a zip file download. Upload support only. Focus on IE handling.
* An [ASP.NET MVC 3 example](https://github.com/maxpavlov/jQuery-File-Upload.MVC3) with the same functionality as an original plugin demo + large files upload support for ASP.NET (contribution from Max Pavlov)
* A fully functional github hosted [ASP.NET example](https://github.com/i-e-b/jQueryFileUpload.Net) (contribution by Iain Ballard), based on ashx handlers. Provides full upload / delete / thumbnail functionality.
* [.NET File Upload Handler](https://github.com/swhitley/jQuery-File-Upload) implementation by [Shannon Whitley](https://github.com/swhitley)
* [VB.net VS2010 implementation](https://github.com/superquinho/jQuery-File-Upload-ASPnet)
* A fully [IHttpHandler based] (https://github.com/YahavGB/jQuery-File-Upload/tree/master/server/asp-net) PHP server-side clone abstract class for uploading files. Class main propose is to upload files (supports high-quality images uploading) (Contributed by Yahav Gindi Bar).
* [Backload.](https://github.com/blackcity/Backload) A professional full featured server side file upload controller/handler for ASP.NET MVC4 (ASP.NET Web API and Standard ASP.NET WebForms supported). It has a very rich set of configuration options but also a zero configuration feature. It supports complex storage structures (set client and server side), image resizing, cropping and conversions, content type based subfolders, tracing, events, etc. [Backload demos and examples](https://github.com/blackcity/Backload) 
* [DNN Platform](https://github.com/dnnsoftware/Dnn.Platform) [gallery module](https://github.com/donker/YAG) with WebAPI upload handler
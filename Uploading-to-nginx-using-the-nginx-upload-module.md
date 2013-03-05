This document will describe the process for setting up the jQuery File Upload plugin to work with the nginx upload module (https://github.com/vkholodkov/nginx-upload-module). A lot of the sample configuration is pretty custom (including the jQuery plugin setup), but the result of this setup will allow you to benefit from the following:

* There will be no "middleman" for the upload. You don't need a PHP or Node app running as the back end to receive the file upload - nginx will take care of that. This means you can offload uploading to something else rather than have your web application have to deal with that process and the strain that comes with it.

* You will have support for resumable, chunked uploading configured to support CORS.

* You can use this configuration on any platform, be it PHP, Ruby, Java, or whatever. All that you need is this plugin and nginx compiled with the upload module.

## Section 1: Configuring the jQuery file upload plugin

An example of our custom uploader interface can be found using the links below. This specific implementation opts out of using the custom UI plugins and instead favors a much simpler interface.

* The supporting HTML markup: https://gist.github.com/4568859
* The supporting Javascript: https://gist.github.com/4568794

## Section 2: Configuring nginx to work with the plugin

Below is an example nginx config file set up to receive chunked uploads from a different origin. This nginx file assumes that you have some web application also running on the same box, and that application is configured to receive the message (200 status and uploaded file information) from nginx when the upload completes.

* The supporting nginx config file: https://gist.github.com/5086894.

There are some important settings in this config file that should be documented, however I'd like to call out again just a couple things.

First, make sure that you have the client_body_buffer_size set to be **less than or equal to** the **maxChunkSize** for your uploader. It is important to not exceed the buffer, otherwise you can end up with some really bad upload performance. Below is a text file that shows the benchmarks for various maxChunkSize and client_body_buffer_size settings. These benchmarks might not be completely accurate for your environment, so feel free to change up your configuration.

* Benchmarks for the plugin's 'maxChunkSize' and nginx's 'client_body_buffer_size settings': https://gist.github.com/3920385

Secondly, the nginx upload module has its own function for passing along headers: _upload_add_header_. This example I've provided demonstrates configuration for CORS. Without these headers (in addition to the normal headers for nginx) you will most likely not be able to upload across domains.

Lastly, make sure that you have created all necessary folders in the folder structure where you'd like your uploads to live. You need a folder for every alphanumeric character. If you plan on storing your file uploads in /var/uploads, then you need to create a single folder for every character in the set a-zA-z0-9. This has to do with how the upload module handles the file upload in combination with the sessionID header being set.

## Section 3: Tying it all together

Assuming you've got everything in place, your application's architecture should be something like this:

* Application A (www.somesite.com): Uses the jQuery File Upload plugin to create a functional upload UI for your users. This uploader posts to a different domain (www.my-upload-endpoint.com/upload) to handle the uploads. This application will wait for a response from Application B to let it know when the upload is finished.

* Application B (www.my-upload-endpoint.com/upload): Has nginx running in front of it that processes the file upload, then forwards the response to the application server running on the box. Once the application receives the information from nginx, you can simply return it so that Application A can receive the final status that everything was created. You might have to modify the response slightly, depending on how you want to format to look. The custom uploader Javascript expects the data to be returned in a specific format, so you might refer to that for some ideas.


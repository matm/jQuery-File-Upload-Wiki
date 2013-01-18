This document will describe the process for setting up the jQuery File Upload plugin to work with the nginx upload module (https://github.com/vkholodkov/nginx-upload-module). A lot of the sample configuration is pretty custom (including the jQuery plugin setup), but the result of this setup will allow you to benefit from the following:

* There will be no "middleman" for the upload. You don't need a PHP or Node app running as the back end to receive the file upload - nginx will take care of that. This means you can offload uploading to something else rather than have your web application have to deal with that process and the strain that comes with it.

* You will have support for resumable, chunked uploading configured to support CORS.

* You can use this configuration on any platform, be it PHP, Ruby, Java, or whatever. All that you need is this plugin and nginx compiled with the upload module.

## Section 1: Configuring the jQuery file upload plugin

The supporting HTML markup: https://gist.github.com/4568859
The supporting Javascript: https://gist.github.com/4568794

## Section 2: Configuring nginx to work with the plugin
pending...

## Section 3: Tying it all together
pending...
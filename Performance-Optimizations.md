This page lists several Performance Optimizations to speed up page load times when using the plugin.

## Using CDNJS

jQuery-File-Upload is distributed through [cdnjs](http://cdnjs.com/libraries/blueimp-file-upload/). [cdnjs](http://cdnjs.com/libraries/blueimp-file-upload/) is provided by CloudFlare CDN infrastructure and is an [open source community-driven project](https://github.com/cdnjs/cdnjs).

You have just to include the library using `//cdnjs.cloudflare.com/ajax/libs/blueimp-file-upload/9.5.2/jquery.fileupload.min.js`. You can include the other extensions too `//cdnjs.cloudflare.com/ajax/libs/blueimp-file-upload/9.5.2/jquery.fileupload-process.min.js`.

If you are using a web framework you can define a helper function to deliver the library from different places based on your environment settings. Below is a helper for Ruby on Rails:

```
def blueimp_file_upload_include_tag
  lib = 'blueimp-file-upload/9.5.2/jquery.fileupload.min.js'
  if Rails.env.production?
    javascript_include_tag "//cdnjs.cloudflare.com/ajax/libs/#{lib}"
  else
    javascript_include_tag "/javascripts/libs/#{lib}"
  end
end
```

## JavaScript minification
The source code of this plugin is not distributed in a minified format.  
However, it is recommended to make use of [UglifyJS](https://github.com/mishoo/UglifyJS), [Google's Closure Compiler](http://code.google.com/closure/compiler/) or a similar tool to combine and minify all your application JavaScript files for production use.

## CSS minification
Another recommended tool for production use is [CSSTidy](http://csstidy.sourceforge.net/) or [LESS](http://lesscss.org/), to tidy and minify the CSS source.

## Making use of a content delivery network for JavaScript libraries
Google provides a reliable [CDN for common Web libraries](http://code.google.com/apis/libraries/devguide.html).  
The more websites use this CDN instead of hosting their JavaScript libraries on their own server, the more likely it is that client browsers will already have those libraries in their browser cache.  
The easiest way to use it is to include the libraries the following way (using a protocol relative URL):

```html
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
```

Another branch of the plugin makes use of [[jQuery UI]], which can also be used via Google's CDN:

```js
<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.23/jquery-ui.min.js"></script>
```

Alongside the JS libraries, Google also hosts the CSS and accompanying images files for several [jQuery UI themes](http://jqueryui.com/themeroller/), e.g. the "basic" theme:

```html
<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.23/themes/base/jquery-ui.css">
```
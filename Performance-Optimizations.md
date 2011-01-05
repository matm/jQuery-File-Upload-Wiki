This page lists several Performance Optimizations to speed up page load times when using the plugin.

## JavaScript minification
The source code of this plugin is not distributed in a minified format.  
However, it is recommended to make use of [Google's Closure Compiler](http://code.google.com/closure/compiler/) or a similar tool to combine and minify all your application JavaScript files for production use.

## CSS minification
Another recommended tool for production use is [CSSTidy](http://csstidy.sourceforge.net/), to tidy and minify the CSS source.

## Making use of a content delivery network for the JQuery libraries
Google provides a reliable [CDN for common Web libraries](http://code.google.com/apis/libraries/devguide.html).  
The more websites use this CDN instead of hosting their jQuery libraries on their own server, the more likely it is that client browsers will already have those libraries in their browser cache.  
The easiest way to use it is to include the libraries the following way (replace http: with https: for SSL-Websites):
```html
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
<script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.6/jquery-ui.min.js"></script>
```

Alongside the JS libraries, Google also hosts the CSS and accompanying images files for several [jQuery UI themes](http://jqueryui.com/themeroller/), e.g. the "basic" theme:
```html
<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.6/themes/base/jquery-ui.css">
```
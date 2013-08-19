## File input styling
To achieve a cross-browser styling of the "Add files" button, the file input field is made transparent and positioned on top of the *fileinput-button* label - see the CSS definitions for the "fileinput-button" class in [jquery.fileupload-ui.css](https://github.com/blueimp/jQuery-File-Upload/blob/master/css/jquery.fileupload-ui.css).

### Why isn't it possible to programmatically trigger the file input selection?
Most browsers prevent submitting files when the input field didn't receive a direct click (or keyboard) event as a security precaution. Some browsers (e.g. Google Chrome) simply prevent the click event, while e.g. Internet Explorer doesn't submit any files that have been selected by a programmatically triggered file input field.  
Firefox 4 (and later) is so far the only browser with full support for invoking "click"-Events on a completely hidden (*display: none*) file input field.

## Bootstrap UI
The current version of the plugin builds on Twitter's [Bootstrap](http://twitter.github.com/bootstrap/) toolkit for its look&feel.

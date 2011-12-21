This tutorial shows how to add [CSS transition](https://developer.mozilla.org/en/CSS/CSS_transitions) effects to your drop zone, e.g. an enlarging effect when the user drags files over the document window.

By default, the plugin uses the complete document as target for dropping files, although it is possible to set the **dropZone** to a specific jQuery collection object (see [[Options]] and [[API]]):

```js
$('#fileupload').fileupload({
    dropZone: $('#dropzone')
});
```

The drop zone could be the following HTML element node:

```html
<div id="dropzone" class="fade well">Drop files here</div>
```

With the following CSS definitions:

```css
#dropzone {
    background: palegreen;
    width: 150px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    font-weight: bold;
}
#dropzone.in {
    width: 600px;
    height: 200px;
    line-height: 200px;
    font-size: larger;
}
#dropzone.hover {
    background: lawngreen;
}
#dropzone.fade {
    -webkit-transition: all 0.3s ease-out;
    -moz-transition: all 0.3s ease-out;
    -ms-transition: all 0.3s ease-out;
    -o-transition: all 0.3s ease-out;
    transition: all 0.3s ease-out;
    opacity: 1;
}
```

Now you only need to add the following JavaScript code the get [CSS transition](https://developer.mozilla.org/en/CSS/CSS_transitions) effects for the drop zone:

```js
$(document).bind('dragover', function (e) {
    var dropZone = $('#dropzone'),
        timeout = window.dropZoneTimeout;
    if (!timeout) {
        dropZone.addClass('in');
    } else {
        clearTimeout(timeout);
    }
    if (e.target === dropZone[0]) {
        dropZone.addClass('hover');
    } else {
        dropZone.removeClass('hover');
    }
    window.dropZoneTimeout = setTimeout(function () {
        window.dropZoneTimeout = null;
        dropZone.removeClass('in hover');
    }, 100);
});
```
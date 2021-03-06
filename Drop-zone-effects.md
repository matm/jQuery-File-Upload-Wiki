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
    var found = false,
      	node = e.target;
    do {
        if (node === dropZone[0]) {
       		found = true;
       		break;
       	}
       	node = node.parentNode;
    } while (node != null);
    if (found) {
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

**Note:**
Here is a small modification to allow for multiple drop zones

```js
$(document).bind('dragover', function (e)
{
	var dropZone = $('.dropzone'),
		foundDropzone,
		timeout = window.dropZoneTimeout;
		if (!timeout)
		{
			dropZone.addClass('in');
		}
		else
		{
			clearTimeout(timeout);
		}
		var found = false,
		node = e.target;
			
		do{

			if ($(node).hasClass('dropzone'))
			{
				found = true;
				foundDropzone = $(node);
				break;
			}

			node = node.parentNode;

		}while (node != null);

		dropZone.removeClass('in hover');

		if (found)
		{
			foundDropzone.addClass('hover');
		}

		window.dropZoneTimeout = setTimeout(function ()
		{
			window.dropZoneTimeout = null;
			dropZone.removeClass('in hover');
		}, 100);
	});
```

**Note:**  
If you want to allow specific drop zones but disable the default browser action for file drops on the document, add the following JavaScript code:

```js
$(document).bind('drop dragover', function (e) {
    e.preventDefault();
});
```

Note that preventing the default action for both the "drop" and "dragover" events is required to disable the default browser drop action.
jQuery File Upload makes use of the [JavaScript Templates](https://github.com/blueimp/JavaScript-Templates) engine to render the list of files.
You can find documentation for this template engine on the [project page](https://github.com/blueimp/JavaScript-Templates).

However, you are not required to use this specific template engine and can replace it with a different implementation, by setting the **uploadTemplateId**, **downloadTemplateId**, **uploadTemplate** and **downloadTemplate** [[Options]]:

```js
$('#fileupload').fileupload({
    uploadTemplateId: null,
    downloadTemplateId: null,
    uploadTemplate: function (o) {
        var rows = $();
        $.each(o.files, function (index, file) {
            var row = $('<tr class="template-upload fade">' +
                '<td class="preview"><span class="fade"></span></td>' +
                '<td class="name"></td>' +
                '<td class="size"></td>' +
                (file.error ? '<td class="error" colspan="2"></td>' :
                        '<td><div class="progress">' +
                            '<div class="bar" style="width:0%;"></div></div></td>' +
                            '<td class="start"><button>Start</button></td>'
                ) + '<td class="cancel"><button>Cancel</button></td></tr>');
            row.find('.name').text(file.name);
            row.find('.size').text(o.formatFileSize(file.size));
            if (file.error) {
                row.find('.error').text(
                    locale.fileupload.errors[file.error] || file.error
                );
            }
            rows = rows.add(row);
        });
        return rows;
    },
    downloadTemplate: function (o) {
        var rows = $();
        $.each(o.files, function (index, file) {
            var row = $('<tr class="template-download fade">' +
                (file.error ? '<td></td><td class="name"></td>' +
                    '<td class="size"></td><td class="error" colspan="2"></td>' :
                        '<td class="preview"></td>' +
                            '<td class="name"><a></a></td>' +
                            '<td class="size"></td><td colspan="2"></td>'
                ) + '<td class="delete"><button>Delete</button> ' +
                    '<input type="checkbox" name="delete" value="1"></td></tr>');
            row.find('.size').text(o.formatFileSize(file.size));
            if (file.error) {
                row.find('.name').text(file.name);
                row.find('.error').text(
                    locale.fileupload.errors[file.error] || file.error
                );
            } else {
                row.find('.name a').text(file.name);
                if (file.thumbnail_url) {
                    row.find('.preview').append('<a><img></a>')
                        .find('img').prop('src', file.thumbnail_url);
                    row.find('a').prop('rel', 'gallery');
                }
                row.find('a').prop('href', file.url);
                row.find('.delete button')
                    .attr('data-type', file.delete_type)
                    .attr('data-url', file.delete_url);
            }
            rows = rows.add(row);
        });
        return rows;
    }
});
```

**Notes:**

* This code snippet assumes a global variable **locale** with a map of translation strings.
* The **uploadTemplate** and **downloadTemplate** methods are supposed to return either a jQuery collection object or a string representation of the rendered upload / download template.
* The **data** parameter passed via **add** callback is stored via [jQuery.data](http://api.jquery.com/data/) on the rendered templates and later reused when submitting or canceling the file upload.
* The rendered templates are expected to have a class of **template-upload** / **template-download** for the start and cancel handlers to retrieve the **data** parameter and for the delete handler to set the context to the template.

Alternatively, you could also provide your own **window.tmpl** template method which accepts template IDs and returns template functions accepting a data parameter and rendering the template content as HTML string or jQuery collection:

```js
window.tmpl = function (id) {
    if (id === 'template-upload') {
        return function (data) {
            return '<tr class="template-upload"><!-- ... --></tr>';
        };
    }
    if (id === 'template-download') {
        return function (data) {
            return '<tr class="template-download"><!-- ... --></tr>';
        };
    }
}
```
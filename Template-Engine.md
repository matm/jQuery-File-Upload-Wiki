jQuery File Upload makes use of the [JavaScript Templates](https://github.com/blueimp/JavaScript-Templates) engine to render the list of files.
You can find documentation for this template engine on the [project page](https://github.com/blueimp/JavaScript-Templates).

However, you are not required to use this specific template engine and can replace it with a different implementation, by setting the **filesContainer**, **uploadTemplateId**, **downloadTemplateId**, **uploadTemplate** and **downloadTemplate** [[Options]]:

```js
$('#fileupload').fileupload({
    filesContainer: $('table.files'),
    uploadTemplateId: null,
    downloadTemplateId: null,
    uploadTemplate: function (o) {
        var rows = $();
        $.each(o.files, function (index, file) {
            var row = $('<tr class="template-upload fade">' +
                '<td><span class="preview"></span></td>' +
                '<td><p class="name"></p>' +
                (file.error ? '<div class="error"></div>' : '') +
                '</td>' +
                '<td><p class="size"></p>' +
                (o.files.error ? '' : '<div class="progress"></div>') +
                '</td>' +
                '<td>' +
                (!o.files.error && !index && !o.options.autoUpload ?
                    '<button class="start">Start</button>' : '') +
                (!index ? '<button class="cancel">Cancel</button>' : '') +
                '</td>' +
                '</tr>');
            row.find('.name').text(file.name);
            row.find('.size').text(o.formatFileSize(file.size));
            if (file.error) {
                row.find('.error').text(file.error);
            }
            rows = rows.add(row);
        });
        return rows;
    },
    downloadTemplate: function (o) {
        var rows = $();
        $.each(o.files, function (index, file) {
            var row = $('<tr class="template-download fade">' +
                '<td><span class="preview"></span></td>' +
                '<td><p class="name"></p>' +
                (file.error ? '<div class="error"></div>' : '') +
                '</td>' +
                '<td><span class="size"></span></td>' +
                '<td><button class="delete">Delete</button></td>' +
                '</tr>');
            row.find('.size').text(o.formatFileSize(file.size));
            if (file.error) {
                row.find('.name').text(file.name);
                row.find('.error').text(file.error);
            } else {
                row.find('.name').append($('<a></a>').text(file.name));
                if (file.thumbnailUrl) {
                    row.find('.preview').append(
                        $('<a></a>').append(
                            $('<img>').prop('src', file.thumbnailUrl)
                        )
                    );
                }
                row.find('a')
                    .attr('data-gallery', '')
                    .prop('href', file.url);
                row.find('.delete')
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

Here's how the templates look like in [Mustache / Handlebars](https://gist.github.com/elmariachi111/5407282)
# jQuery File-Upload-UI
# Includes

The fileUpload plugin can be used with jQueryUI to create a cooler look. 

**IMPORTANT** : You will have 3 files to include, which are in order : 

* jquery.ui.js (the jQueryUI plugin [[found here|http://jqueryui.com/]] )
* jquery.fileUpload.js (this plugin, without jqueryUI)
* jquery.fileUpload-ui.js (this plugin with jqueryUI)


# Calling the plugin

The same objects as the basic [[fileUpload plugin|Documentation]] are used, but the **method name changes** to fileUploadUI. For example : 

```js
    $('uploadForm').fileUploadUI(options);
```


# Options

All the options from the [[fileUpload plugin|Documentation]] are usable here as well. In addition, the following options are used : 

### uploadTable (object) **required**

A jQuery object refering to the xHTML element where you want files upload progress to be displayed. 

### downloadTable (object) **required**

A jQuery object refering to the xHTML where you want uploaded files to be displayed.

### buildUploadRow (function) **required**

A function to be called to display an upload element in the uploadTable. Must return an xHTML string to be inserted in the uploadTable using jQuery's *appendTo* method. Function receives the following arguments : 

* files : An array containing **all** the files currently uploading (each is an [[HTML5 file object|http://www.w3.org/TR/FileAPI/]] )
* index : the index in the array of the currently uploading file

As such, you can retrieving the last uploaded file using *files[index]* (See example below)

### buildDownloadRow (function) **required**

A function to be called to display an uploaded element in the downloadTable. Must also return an xHTML string that will be appeneded to the downloadTable. Function receives the following argument : 

* file : the uploaded file (an [[HTML5 file object|http://www.w3.org/TR/FileAPI/]] )


# Example : 

```js
    $('.uploadForm').fileUploadUI({
          uploadTable: $('.upload_files'),
          downloadTable: $('.download_files'),
          buildUploadRow: function (files, index) {
              var file = files[index];
              return $(
                    '<tr style="display:none">' +
                    '<td>' + file.name + '<\/td>' +
                    '<td class="file_upload_progress"><div><\/div><\/td>' +
                    '<td class="file_upload_cancel">' +
                    '<div class="ui-state-default ui-corner-all ui-state-hover" title="Cancel">' +
                    '<span class="ui-icon ui-icon-cancel"><\/span>' +
                    '<\/div>' +
                    '<\/td>' +
                    '<\/tr>'
              );
          },
          buildDownloadRow: function (file) {
              return $(
                    '<tr style="display:none"><td>' + file.name + '<\/td><\/tr>'
              );
          }
    });
```
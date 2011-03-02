## Does the plugin have to be called on a form tag?
Technically, the form is only required for iframe uploads.
If you define the *url*, *method* and *fieldName* [[Options]], you can call the plugin on any element - no form or file input field required - and the drag and drop functionality will still work.
The file input button functionality would technically also work without a form, but the plugin code currently requires the file input to be part of a form.
But without support for iframe uploads, you will leave users of IE and Opera out in the dark.

However, what you can do is using the *dropZone* option to define a container inside of the form as dropZone target.
You can also adjust the CSS code so the file input field does not cover the whole form.

Please have a look at [[How to submit additional Form Data]], specifically the second section about "Sending additional Form Data by adding input fields to the upload form".
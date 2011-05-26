The following widget class extends $.blueimpUI.fileupload (the UI version of the File Upload plugin) to add drop zone effects.

When files are dragged onto the document window, the class "ui-state-active" will be added to the dropzone element with a transition effect.  
When files are dragged over the dropzone element, the class "ui-state-highlight" will be added to the dropzone element with a transition effect.

```js
$.widget('blueimpDropZoneEffects.fileupload', $.blueimpUI.fileupload, {

    options: {
        dropZone: $()
    },

    _dropZoneActivate: function () {
        this.options.dropZone.addClass(
            'ui-state-active',
            'normal'
        );
        this._dropZoneActive = true;
    },

    _dropZoneDeactivate: function () {
        this.options.dropZone.removeClass(
            'ui-state-active',
            'normal'
        );
        this._dropZoneActive = false;
    },

    _dropZoneHighLight: function (dropZone) {
        dropZone.toggleClass(
            'ui-state-highlight',
            'normal'
        );
    },

    _dropZoneDragEnter: function (e) {
        var fu = e.data.fileupload;
        fu._dropZoneHighLight($(e.target));
    },
    
    _dropZoneDragLeave: function (e) {
        var fu = e.data.fileupload;
        fu._dropZoneHighLight($(e.target));
    },

    _documentDragEnter: function (e) {
        var fu = e.data.fileupload;
        if (!fu._dropZoneActive) {
            fu._dropZoneActivate();
        }
    },

    _documentDragOver: function (e) {
        var fu = e.data.fileupload;
        clearTimeout(fu._dragoverTimeout);
        fu._dragoverTimeout = setTimeout(function () {
            fu._dropZoneDeactivate();
        }, 200);
    },

    _create: function () {
        if (this.options.dropZone && !this.options.dropZone.length) {
            this.options.dropZone = this.element.find('.dropzone-container div');
        }
        $.blueimpUI.fileupload.prototype._create.call(this);
        var ns = this.options.namespace || this.name;
        this.options.dropZone
            .bind('dragenter.' + ns, {fileupload: this}, this._dropZoneDragEnter)
            .bind('dragleave.' + ns, {fileupload: this}, this._dropZoneDragLeave);
        $(document)
            .bind('dragenter.' + ns, {fileupload: this}, this._documentDragEnter)
            .bind('dragover.' + ns, {fileupload: this}, this._documentDragOver);
    },
    
    destroy: function () {
        var ns = this.options.namespace || this.name;
        this.options.dropZone
            .unbind('dragenter.' + ns, this._dropZoneDragEnter)
            .unbind('dragleave.' + ns, this._dropZoneDragLeave);
        $(document)
            .unbind('dragenter.' + ns, this._documentDragEnter)
            .unbind('dragover.' + ns, this._documentDragOver);
        $.blueimpUI.fileupload.prototype.destroy.call(this);
    }

});
```

The customized widget class adjusts the default dropZone setting and expects the following HTML code as child element of the file upload widget:
```html
<div class="dropzone-container">
    <div class="dropzone ui-corner-all">Drop files here</div>
</div>
```

The following CSS definitions are an example for enlarge/reduce and highlight effects for the dropzone element:
```css
.dropzone-container div {
  width: 0px;
  height: 0px;
  line-height: 0px;
  font-size: 0em;
  text-align: center;
}

.dropzone-container div.ui-state-active {
  width: 600px;
  height: 200px;
  line-height: 200px;
  font-size: 3em;
  background: lightgreen;
}

.dropzone-container div.ui-state-highlight {
  background: limegreen;
}
```
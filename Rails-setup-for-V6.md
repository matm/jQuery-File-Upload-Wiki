## Prerequisites

* have jQuery setup in your app

* copy jQuery File Upload files in the proper directories of your Rails app 

## Models
### For use with the Carrierwave gem
We'll use basic Carrierwave uploader:

``` ruby
class AvatarUploader < CarrierWave::Uploader::Base
  storage :file
  def store_dir
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
  end
end
```

Let's set up a `Picture` model

``` ruby
class Picture < ActiveRecord::Base
  include Rails.application.routes.url_helpers
  mount_uploader :avatar, AvatarUploader

  #one convenient method to pass jq_upload the necessary information
  def to_jq_upload
    {
      "name" => read_attribute(:avatar),
      "size" => avatar.size,
      "url" => avatar.url,
      "thumbnail_url" => avatar.thumb.url,
      "delete_url" => picture_path(:id => id),
      "delete_type" => "DELETE" 
    }
  end
end
```

### For use with the Dragonfly gem
set up a `Picture` model (make sure you have both avatar_uid and avatar_name columns in your database)

``` ruby
class Picture < ActiveRecord::Base
  include Rails.application.routes.url_helpers
  image_accessor :avatar

  #one convenient method to pass jq_upload the necessary information
  def to_jq_upload
    {
      "name" => read_attribute(:avatar_name),
      "size" => avatar.size,
      "url" => avatar.url,
      "thumbnail_url" => avatar.thumb('80x80#').url,
      "delete_url" => picture_path(:id => id),
      "delete_type" => "DELETE" 
    }
  end
end
```

## Controller

First one controller to handle the downloads:
(html response is for browsers using iframe sollution)

``` ruby
class PicturesController < ApplicationController

  def index
    @pictures = Picture.all
    render :json => @pictures.collect { |p| p.to_jq_upload }.to_json
  end

  def create
    @picture = Picture.new(params[:picture])
    if @picture.save
      respond_to do |format|
        format.html {  
          render :json => [@picture.to_jq_upload].to_json, 
          :content_type => 'text/html',
          :layout => false
        }
        format.json {  
          render :json => [@picture.to_jq_upload].to_json			
        }
      end
    else 
      render :json => [{:error => "custom_failure"}], :status => 304
    end
  end

  def destroy
    @picture = Picture.find(params[:id])
    @picture.destroy
    render :json => true
  end
end
```

And it's routes:

``` ruby
resources :pictures, :only => [:index, :create, :destroy]
```
    
## Let's play

In whatever view file you want, copy paste this code and enjoy.

``` html

<h2><%= t('photos.title') %></h2>
<%= form_for Picture.new, :html => { :multipart => true, :id => "fileupload"  } do |f| %>
  <div class="row">
    <div class="span16 fileupload-buttonbar">
      <div class="progressbar fileupload-progressbar nofade"><div style="width:0%;"></div></div>
      <span class="btn success fileinput-button">
        <span><%= t('photos.add_files') %>...</span>
        <%= f.file_field :path %>
      </span>
      <button type="submit" class="btn primary start"><%= t('photos.start_upload') %></button>
      <button type="reset" class="btn info cancel"><%= t('photos.cancel_upload') %></button>
      <button type="button" class="btn danger delete"><%= t('photos.delete_selected') %></button>
      <input type="checkbox" class="toggle">
    </div>
  </div>
  <br>
  <div class="row">
    <div class="span16">
      <table class="zebra-striped"><tbody class="files"></tbody></table>
      <div id="loading"> </div>
    </div>
  </div>
<% end %>
<script>
  var fileUploadErrors = {
    maxFileSize: 'File is too big',
    minFileSize: 'File is too small',
    acceptFileTypes: 'Filetype not allowed',
    maxNumberOfFiles: 'Max number of files exceeded',
    uploadedBytes: 'Uploaded bytes exceed file size',
    emptyResult: 'Empty file upload result'
  };
</script>

<!-- IMPORTANT fade class makes fileupload depend on css transition effect REMOVE or RENAME it -->
<script id="template-upload" type="text/html">
  {% for (var i=0, files=o.files, l=files.length, file=files[0]; i<l; file=files[++i]) { %}
  <tr class="template-upload nofade">
    <td class="preview"><span class="nofade"></span></td>
    <td class="name">{%=file.name%}</td>
    <td class="size">{%=o.formatFileSize(file.size)%}</td>
    {% if (file.error) { %}
    <td class="error" colspan="2"><span class="label important">Error</span> {%=fileUploadErrors[file.error] || file.error%}</td>
    {% } else if (o.files.valid && !i) { %}
    <td class="progress"><div class="progressbar"><div style="width:0%;"></div></div></td>
    <td class="start">{% if (!o.options.autoUpload) { %}<button class="btn primary"><%= t('photos.template.start') %></button>{% } %}</td>
    {% } else { %}
    <td colspan="2"></td>
    {% } %}
    <td class="cancel">{% if (!i) { %}<button class="btn info"><%= t('photos.template.cancel') %></button>{% } %}</td>
  </tr>
  {% } %}
</script>
<script id="template-download" type="text/html">
  {% for (var i=0, files=o.files, l=files.length, file=files[0]; i<l; file=files[++i]) { %}
  <tr class="template-download nofade">
    {% if (file.error) { %}
    <td></td>
    <td class="name">{%=file.name%}</td>
    <td class="size">{%=o.formatFileSize(file.size)%}</td>
    <td class="error" colspan="2"><span class="label important">Error</span> {%=fileUploadErrors[file.error] || file.error%}</td>
    {% } else { %}
    <td class="preview">{% if (file.thumbnail_url) { %}
      <a href="{%=file.url%}" title="{%=file.name%}" rel="gallery"><img src="{%=file.thumbnail_url%}"></a>
    {% } %}</td>
    <td class="name">
      <a href="{%=file.url%}" title="{%=file.name%}" rel="{%=file.thumbnail_url&&'gallery'%}">{%=file.name%}</a>
    </td>
    <td class="size">{%=o.formatFileSize(file.size)%}</td>
    <td colspan="2"></td>
    {% } %}
    <td class="delete">
    <button class="btn danger" data-type="{%=file.delete_type%}" data-url="{%=file.delete_url%}"><%= t('photos.template.delete') %></button>
    <input type="checkbox" name="delete" value="1">
    </td>
  </tr>
  {% } %}
</script>


<!-- The jQuery UI widget factory, can be omitted if jQuery UI is already included -->
<%= javascript_include_tag 'jquery.ui.widget.js' %>
<!-- The Templates and Load Image plugins are included for the FileUpload user interface -->
<script src="http://blueimp.github.com/JavaScript-Templates/tmpl.min.js"></script>
<script src="http://blueimp.github.com/JavaScript-Load-Image/load-image.min.js"></script>
<!-- The Iframe Transport is required for browsers without support for XHR file uploads -->
<%= javascript_include_tag 'jquery.iframe-transport.js' %>
<%= javascript_include_tag 'jquery.fileupload.js' %>
<%= javascript_include_tag 'jquery.fileupload-ui.js' %>
<!-- add include_tag js files to config.assets.precompile in ...environments/production.rb if you have it in vendor/ assets -->

<script type="text/javascript" charset="utf-8">
    $(function () {
        // Initialize the jQuery File Upload widget:
        $('#fileupload').fileupload();
        // 
        // Load existing files:
        $.getJSON($('#fileupload form').prop('action'), function (files) {
          var fu = $('#fileupload').data('fileupload'), 
            template;
          fu._adjustMaxNumberOfFiles(-files.length);
          template = fu._renderDownload(files)
            .appendTo($('#fileupload .files'));
          // Force reflow:
          fu._reflow = fu._transition && template.length &&
            template[0].offsetWidth;
          template.addClass('in');
          $('#loading').remove();
        });

    });
</script>
```
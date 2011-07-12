**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.
User contributions for an updated example are welcome.

This page sums up the necessary steps to make the plugin work for Rails 3 and Paperclip.

It's very basic now: no error handling.

If you want more features, like crop, see: [[https://github.com/apneadiving/Pic-upload---Crop-in-Ajax]]

# Pre-requisites

You should have a model, say `Upload` with:

1. the classic scaffold controller

2. a Paperclip attachment set-up, say `picture`

You should have downloaded the jQuery-File-Upload assets:

1. jquery.fileupload.js

2. jquery.fileupload-ui.js

3. jquery.fileupload-ui.css

# Let's do it

## Create a new controller
`rails g controller home`

Create your `index` action:

    def index
      @upload  = Upload.new
    end

## Create a new page
###1. Create `index.html.erb` in your `views/home` page.

###2. Fill in the view:

    <%= stylesheet_link_tag('jquery.fileupload-ui') %>

    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
    <script src="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.9/jquery-ui.min.js"></script>
    <%= javascript_include_tag 'jquery.fileupload', 'jquery.fileupload-ui' %>
	
    <script type="text/javascript" charset="utf-8">	
       $(function () {
	  $('.upload').fileUploadUI({
	        uploadTable: $('.upload_files'),
	        downloadTable: $('.download_files'),
	        buildUploadRow: function (files, index) {
	            var file = files[index];
	            return $('<tr><td>' + file.name + '<\/td>' +
	                    '<td class="file_upload_progress"><div><\/div><\/td>' +
	                    '<td class="file_upload_cancel">' +
	                    '<button class="ui-state-default ui-corner-all" title="Cancel">' +
	                    '<span class="ui-icon ui-icon-cancel">Cancel<\/span>' +
	                    '<\/button><\/td><\/tr>');
	        },
	        buildDownloadRow: function (file) {
	            return $('<tr><td><img alt="Photo" width="40" height="40" src="' + file.pic_path + '">' + file.name + '<\/td><\/tr>');
	        },
	    });
	});
    </script>

    <div class="files"> 
	<%= form_for @upload, :html => { :class => "upload", :multipart => true } do |f| %>
	  <%= f.file_field :picture %>
	  <div>Upload files</div>
	<% end %>

    <table class="upload_files"></table>
    <table class="download_files"></table>
    </div>

## Set up your routes.rb file

It should contain:

    resources :uploads
    root :to => "home#index"

## Update your `uploads_controller.rb` file
```ruby
    def create
      @upload = Upload.new(params[:upload])
      if @upload.save
        render :json => { :pic_path => @upload.picture.url.to_s , :name => @upload.picture.instance.attributes["picture_file_name"] }, :content_type => 'text/html'
      else
        render :json => { :result => 'error'}, :content_type => 'text/html'
      end
    end
```

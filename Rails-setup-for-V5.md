## Prerequisites

* have jQuery setup in your app

* copy jQuery File Upload files in the proper directories of your Rails app 

## Models
### For use with the Carrierwave gem
We'll use basic Carrierwave uploader:

    class AvatarUploader < CarrierWave::Uploader::Base
      storage :file
      def store_dir
        "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
      end
    end

Let's set up a `Picture` model

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

### For use with the Dragonfly gem
set up a `Picture` model (make sure you have both avatar_uid and avatar_name columns in your database)

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

## Controller

First one controller to handle the downloads:

    class PicturesController < ApplicationController

      def index
        @pictures = Picture.all
        render :json => @pictures.collect { |p| p.to_jq_upload }.to_json
      end

      def create
        @picture = Picture.new(params[:picture])
        if @picture.save
          render :json => [ @picture.to_jq_upload ].to_json
        else 
          render :json => [ @picture.to_jq_upload.merge({ :error => "custom_failure" }) ].to_json
        end
      end

      def destroy
        @picture = Picture.find(params[:id])
        @picture.destroy
        render :json => true
      end
    end

And it's routes:

    resources :pictures, :only => [:index, :create, :destroy]
    
## Let's play

In whatever view file you want, copy paste this code and enjoy.

		<link rel="stylesheet" href="http://ajax.googleapis.com/ajax/libs/jqueryui/1.8.13/themes/base/jquery-ui.css" id="theme">
		<%= stylesheet_link_tag 'jquery.fileupload-ui' %>

		<div id="fileupload">
			<%= form_for Picture.new, :html => { :multipart => true } do |f| %>
		        <div class="fileupload-buttonbar">
		            <label class="fileinput-button">
		                <span>Add files...</span>
                                <%= f.file_field :avatar %>
		            </label>
		            <button type="submit" class="start">Start upload</button>
		            <button type="reset" class="cancel">Cancel upload</button>
		            <button type="button" class="delete">Delete files</button>
		        </div>
			<% end %>
		    <div class="fileupload-content">
		        <table class="files"></table>
		        <div class="fileupload-progressbar"></div>
		    </div>
		</div>


		<script id="template-upload" type="text/x-jquery-tmpl">
		    <tr class="template-upload{{if error}} ui-state-error{{/if}}">
		        <td class="preview"></td>
		        <td class="name">${name}</td>
		        <td class="size">${sizef}</td>
		        {{if error}}
		            <td class="error" colspan="2">Error:
		                {{if error === 'custom_failure'}}Custom Error Message
		                {{else}}${error}
		                {{/if}}
		            </td>
		        {{else}}
		            <td class="progress"><div></div></td>
		            <td class="start"><button>Start</button></td>
		        {{/if}}
		        <td class="cancel"><button>Cancel</button></td>
		    </tr>
		</script>
		<script id="template-download" type="text/x-jquery-tmpl">
		    <tr class="template-download{{if error}} ui-state-error{{/if}}">
		        {{if error}}
		            <td></td>
		            <td class="name">${name}</td>
		            <td class="size">${sizef}</td>
		            <td class="error" colspan="2">Error:
		                {{if error === 1}}File exceeds upload_max_filesize (php.ini directive)
		                {{else}}${error}
		                {{/if}}
		            </td>
		        {{else}}
		            <td class="preview">
		                {{if thumbnail_url}}
		                    <a href="${url}" target="_blank"><img src="${thumbnail_url}"></a>
		                {{/if}}
		            </td>
		            <td class="name">
		                <a href="${url}"{{if thumbnail_url}} target="_blank"{{/if}}>${name}</a>
		            </td>
		            <td class="size">${sizef}</td>
		            <td colspan="2"></td>
		        {{/if}}
		        <td class="delete">
		            <button data-type="${delete_type}" data-url="${delete_url}">Delete</button>
		        </td>
		    </tr>
		</script>
		<script src="//ajax.googleapis.com/ajax/libs/jquery/1.6.1/jquery.min.js"></script>
		<script src="//ajax.googleapis.com/ajax/libs/jqueryui/1.8.13/jquery-ui.min.js"></script>
		<script src="//ajax.aspnetcdn.com/ajax/jquery.templates/beta1/jquery.tmpl.min.js"></script>
		<%= javascript_include_tag 'jquery.iframe-transport' %>
		<%= javascript_include_tag 'jquery.fileupload' %>
		<%= javascript_include_tag 'jquery.fileupload-ui' %>
		<script type="text/javascript" charset="utf-8">
			$(function () {
			    // Initialize the jQuery File Upload widget:
			    $('#fileupload').fileupload();
			    // 
			    // Load existing files:
			    $.getJSON($('#fileupload form').prop('action'), function (files) {
			        var fu = $('#fileupload').data('fileupload');
			        fu._adjustMaxNumberOfFiles(-files.length);
			        fu._renderDownload(files)
			            .appendTo($('#fileupload .files'))
			            .fadeIn(function () {
			                // Fix for IE7 and lower:
			                $(this).show();
			            });
			    });

			    // Open download dialogs via iframes,
			    // to prevent aborting current uploads:
			    $('#fileupload .files a:not([target^=_blank])').live('click', function (e) {
			        e.preventDefault();
			        $('<iframe style="display:none;"></iframe>')
			            .prop('src', this.href)
			            .appendTo('body');
			    });

			});
		</script>
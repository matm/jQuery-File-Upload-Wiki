Here is a rails-specific example of uploading directly to S3 using the newest version of the plugin.

It makes a lot of assumptions, but it will get you where you need to be!

Ok, given you have a form like so:
```haml
%form#file_upload(action="https://YOURBUCKET.s3.amazonaws.com" method="post" enctype="multipart/form-data")
  # order is important!
  # also, the things that are not filled in right now *will* be filled in soon.  See below.
  %input{:type => :hidden, :name => :key)
  %input{:type => :hidden, :name => "AWSAccessKeyId", :value => "YOUR_ACCESS_KEY"}
  %input{:type => :hidden, :name => :acl,  :value => :private}
  %input{:type => :hidden, :name => :success_action_redirect}
  %input{:type => :hidden, :name => :policy}
  %input{:type => :hidden, :name => :signature}

  .fileupload-content
    .fileupload-progress
  .file-upload
    %label.fileinput-button
      %span Upload Document
      %input{:type => :file, :name => :file}

```

..and you have javascript to respond to that form like so:

```javascript
$(function() {
  $('#file_upload').fileupload({
    forceIframeTransport: true,
    autoUpload: true,
    send: function (event, data) {
      $.ajax({
        url: "/documents",
        type: 'POST',
        dataType: 'json',
        data: {doc: {title: data.files[0].name}},
        async: false,
        success: function(retdata) {
          // after we created our document in rails, it is going to send back JSON of they key,
          // policy, and signature.  We will put these into our form before it gets submitted to amazon.
          $('#file_upload').find('input[name=key]').val(retdata.key);
          $('#file_upload').find('input[name=policy]').val(retdata.policy);
          $('#file_upload').find('input[name=signature]').val(retdata.signature);
          $('#file_upload').find('input[name=success_action_redirect]').val(retdata.success_action_redirect);
        }
      });

      // show a loading spinner because now the form will be submitted to amazon, and the file will be directly uploaded there, via an iframe in the background. 
      $('#loading').show();
    },
    fail: function(e, data) {
      console.log('fail');
      console.log(data);
    },
    done: function (event, data) {
      // here you can perform an ajax call to get your documents to display on the screen.
      $('#your_documents').load("/documents?for_item=1234");
      
      // hide the loading spinner that we turned on earlier.
      $j('#loading').hide();
    },
  });
});
```

and your controller looks something like this:

```ruby
  # create the document in rails, then send json back to our javascript to populate the form that will be
  # going to amazon.
  def create
    @document = Document.create(params[:doc])
    render :json => {
      :policy => s3_upload_policy_document, 
      :signature => s3_upload_signature, 
      :key => @document.s3_key, 
      :success_action_redirect => document_upload_success_document_url(@document)
    }
  end
  
  # just in case you need to do anything after the document gets uploaded to amazon.
  # but since we are sending our docs via a hidden iframe, we don't need to show the user a 
  # thank-you page.
  def s3_confirm
    head :ok
  end
 
  private
  
  # generate the policy document that amazon is expecting.
  def s3_upload_policy_document
    return @policy if @policy
    ret = {"expiration" => 5.days.from_now.utc.xmlschema,
      "conditions" =>  [ 
        {"bucket" =>  YOUR_BUCKET_NAME}, 
        ["starts-with", "$key", @document.s3_key],
        {"acl" => "private"},
        {"success_action_redirect" => document_upload_success_document_url(@doucment)},
        ["content-length-range", 0, 1048576]
      ]
    }
    @policy = Base64.encode64(ret.to_json).gsub(/\n/,'')
  end

  # sign our request by Base64 encoding the policy document.
  def s3_upload_signature
    signature = Base64.encode64(OpenSSL::HMAC.digest(OpenSSL::Digest::Digest.new('sha1'), YOUR_ACCESS_KEY, s3_upload_policy_document)).gsub("\n","")
  end
```
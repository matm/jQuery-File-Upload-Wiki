# Upload to YouTube

YouTube provides a browser upload API that can be used together with the File Upload plugin:
http://code.google.com/apis/youtube/1.0/developers_guide_python.html#BrowserUpload (Python)
http://code.google.com/apis/youtube/1.0/developers_guide_php.html#BrowserUpload (PHP)

The following tutorial makes use of the File Upload **send** [API](https://github.com/blueimp/jQuery-File-Upload/wiki/API) to submit the meta data via a server-side application and then upload the video file itself directly to YouTube:

## Client-side

```js
$('#fileupload').fileupload({
    forceIframeTransport: true,
    submit: function (e, data) {
        var that = $(this);
        // Post the current file meta data to your application,
        // to receive the YouTube upload url and token:
        // http://code.google.com/apis/youtube/1.0/developers_guide_python.html#BrowserUpload
        $.post('/your_application_url', data.files[0], function (result) {
            data.url = result.url;
            data.formData = {token: result.token};
            that.fileupload('send', data);
        });
        return false;
    }
});
```

Since YouTube only allows single file uploads and expects a certain parameter name, the file input field needs to be adjusted as well:

```html
<input type="file" name="file">
```

## Server-side

When you want to let your users upload video to YOUR Youtube channel (not requiring them to have an Youtube account and not forcing them to keep the video alive in their channel), you'll have to use deprecated ClientLogin auth method. This method allows you to log in to your account with email and password, get the access token and make requests to API with it.

### PHP
Here's an example code on PHP.

```php
$EMAIL = YOUR_EMAIL;
$PASS = YOUR_PASS;
$ch = curl_init();
$header = array();
$header[] = "Content-type: application/x-www-form-urlencoded;";
curl_setopt_array($ch, array(
	CURLOPT_URL=>'https://www.google.com/accounts/ClientLogin',
	CURLOPT_HTTPHEADER=>$header,
	CURLOPT_RETURNTRANSFER=>true,
	CURLOPT_SSL_VERIFYPEER=>false,
	CURLOPT_SSL_VERIFYHOST=>false,
	CURLOPT_POST=>true,
	// source is needed for stats, so define it as you like
	CURLOPT_POSTFIELDS=>"Email=$EMAIL&Passwd=$PASS&service=youtube&source=SOURCE"
));

// the main token needed to make requests
// since it hasn't limit by time, you can store it somewhere and not log in each time
$authToken = trim(array_pop(explode('Auth=',curl_exec($ch))));
// developer key could be found in app console by creating any web app
// it can be called 'api key' or 'simple api key', any of them should work
$DEVELOPER_KEY = DEVELOPER_KEY;

curl_close($ch);

// we need to construct xml to interact
$post_body = '<?xml version="1.0"?>
	<entry xmlns="http://www.w3.org/2005/Atom"
	  xmlns:media="http://search.yahoo.com/mrss/"
	  xmlns:yt="http://gdata.youtube.com/schemas/2007">
	  <media:group>
		<media:title type="plain">TITLE_OF_VIDEO</media:title>
		<media:description type="plain">
		  Description
		</media:description>
		<media:category
		  scheme="http://gdata.youtube.com/schemas/2007/categories.cat">CATEGORY
		</media:category>
		<media:category
		  scheme="http://gdata.youtube.com/schemas/2007/categories.cat">SECOND_CATEGORY
		</media:category>
		<media:keywords>KEYWORDS_SEPARATED_BY_COMMA</media:keywords>
	  </media:group>
	</entry>';

$ch = curl_init();
$header = array();
$header[] = "X-GData-Key: key=$DEVELOPER_KEY";
$header[] = "Authorization: GoogleLogin auth=$authToken";
$header[] = "GData-Version: 2";
$header[] = "Content-length: ".strlen($post_body);
$header[] = "Content-type: application/atom+xml; charset=UTF-8";
curl_setopt_array($ch, array(
	CURLOPT_URL=>'gdata.youtube.com/action/GetUploadToken',
	CURLOPT_HTTPHEADER=>$header,
	CURLOPT_RETURNTRANSFER=>true,
	CURLOPT_POST=>true,
	CURLOPT_POSTFIELDS=>$post_body
));
$response = (array)simplexml_load_string(curl_exec($ch));
// we need to tune the url with necessary parameter 'nexturl'
// on which you'll get redirect with 'id' of video, 'status' (HTTP responce) and 'error' (if there is some) in GET
$result['url'] .= '?nexturl='.url_encode(YOUR_REDIRECT_PAGE);

// result will have 'token' and 'url'
```
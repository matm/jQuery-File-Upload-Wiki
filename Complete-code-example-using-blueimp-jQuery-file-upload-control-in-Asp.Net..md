Hi guys,

I struggled a lot especially to make my code work in IE (7.0, 8.0, 9.0) version when trying to post files data to the server.Once we posted to the server I want to get the file storage details and other attributes of the file posted as JSON object from the server.
ddd
For this I posted using blueimp jQuery file upload plugin to post and using a handler on the server side to respond for this request. While doing this
I found few interesting problems with IE, like getting warning as below.

Once I fixed the issue with this, I was able to post to the server but not getting the JSON content as “fileuploaddone” event is not firing up. This is because I had the content type as
“Application/json” in handler. Once I changed the response content type as “text/plain” it started working and able to get the event. But as IE don’t jqXHR object we should tweak our code
to get the JSON data (from server) and binding them to the download template in the page. In fact somehow IE is posting the full client name (c:\tmp\file.jpg)
in the htppposted file object (in the server handler) where as other browsers giving only file name like “file.jpg”. for that I am handling the IE request differently in the server.
All these issues I figured out and fixed and I am posting the source code at the below url.

http://www.webtrendset.com/2011/06/22/complete-code-example-for-using-blueimp-jquery-file-upload-control-in-asp-net/

Download the zip file and extract, then create new website project using visual studio by pointing the website to the file directory you unzipped and run the application. Have fun coding.


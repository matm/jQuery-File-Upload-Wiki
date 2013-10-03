Using this latest version with CI 2.1.x is quite simple. Many thanks to [Sinisa](http://stackoverflow.com/users/2167492/sinisa) who wrote a post pointing me to the right direction. And apologies in advance for the less than pleasing formatting - markdown is still something that I haven't fully 
mastered. 

So - here is how to do use jQuery-File-Upload 8.8.5 with CI 2.1.x in five (well actually four) steps. 

**Step1**: Copy UploadHandler.php into <yoursite>/application/libraries. You can rename it if you like - but change the class name as well. (I renamed it to Uploadhandler.php, and changed the classname to as given below) 

<?php
    class Uploadhandler {
    // Rest of the code as supplied
    }

**Step2**: In Uploadhandler.php (use the name as you have given above), change the option value for "script_url" to as below:

        $this->options = array(
            'script_url' => $this->get_full_url().'<your controller and method>',
         ...

in my code, for instance, I had a controller called uploads. I created a method called do_upload in this controller - so my options value was:

        $this->options = array(
            'script_url' => $this->get_full_url().'/uploads/do_upload',
         ...

Note: I use a blank ('') base_url in config.php, and use .htaccess to create a "REST"-like interface. 

$config['base_url']	= '';

**Step3**: Create your controller and method as per the name above and instantiate the library like this (I am directly copying excerpts from my own controller class - you may of course want a different controller:

<?php
class Uploads extends CI_Controller {

    public function __construct()
    {
        parent::__construct();
    }

    public function do_upload()
    {
        $this->load->library("uploadhandler");
    }
}


**Step4**: Update the controller and method information in the variable "url" in your js - either main.js - or in the inline javascript (I used inline because my app needed it - the values and the place remain the same however). Do note that my example continues with the controller and method I wrote for myself - don't forget to update it in case you are using your own. 

    <script type="text/javascript">
        $(function () {
            'use strict';
            var url = "<?=base_url()?>uploads/do_upload";
            $('#fileupload').fileupload({
                url: url,
                dataType: 'json',
                done: function (e, data) {
                    $.each(data.result.files, function (index, file) {
                        $('<p/>').text(file.name).appendTo('#files');
                    });
                },
                progressall: function (e, data) {
                    var progress = parseInt(data.loaded / data.total * 100, 10);
                    $('#progress .progress-bar').css(
                        'width',
                        progress + '%'
                    );
                }
            }).prop('disabled', !$.support.fileInput)
                .parent().addClass($.support.fileInput ? undefined : 'disabled');
        });
    </script>

**Step5**: Thats it. I created the relevant directories ('files') made sure it had write permissions for apache and it worked. 

I havent had the time to extract the code files and upload - in case anyone is interested, please let me know and I will be happy to put it up. 
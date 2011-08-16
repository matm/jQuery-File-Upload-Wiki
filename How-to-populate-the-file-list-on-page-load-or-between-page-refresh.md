**Note:**
This tutorial is for the outdated [v4 branch](https://github.com/blueimp/jQuery-File-Upload/tree/v4) of the plugin.
User contributions for an updated example are welcome.

# How to populate the file list on page load or between page refresh

If one is editting files in a particular folder, it may be that you wish the files to be pre-populated on page load. Or perhaps you wish to be able to refresh the page and keep the list of files that were uploaded from disapearing. This is relatively simple to achieve. 

This is all done by creating the correct rows in the following element that was created in step 4 of the Setup Demo:

```html
<table class="download_files">...populate rows here...</table>
```

One simply needs to add the table rows inside of this element, in the identical format you are using to build the rows in the 'buildDownloadRow' event. The simplest way to do this is by building the rows dynamically in the server side language you are using (e.g. C#, PHP, etc).



C# MVC Example:

```html
<table class="download_files">
    <% foreach (System.IO.FileInfo  file in (System.IO.FileInfo[])ViewData["FileEntryCollection"]) { %>
            <tr style="display: block;">
                <td class="file_preview"></td>
                <td class="filename"><a href="DownloadUploadedFile?file=<%=file.Name %>"><%=file.Name%></a></td>
                <td class="filesize"><%=file.Length / 1024 %> KB</td>
                <td class="file_delete">
                <button title="Delete" class="ui-state-default ui-corner-all ui-state-hover" 
                    onclick="OpenDeleteDialog(this, '<%=file.Name %>')">
                <span class="ui-icon ui-icon-trash"></span>
                </button>
                </td>
            </tr>
    <% } %>
</table>
```

It is up to you obviously to choose a class to represent the file. It should have at the least a name and a size property - ideally the size is nicely formatted to display KB, MB and so forth. For the sake of this demo, I am using the standard FileInfo object that is included in the System.IO namespace in the .NET framework.

For those that are interested in the server side code here it is:

C# MVC Server side code:
`
        public ActionResult UploadFiles()
        {
            string path = "C:\Some Path";                       //The path for the uploads
            DirectoryInfo di = new DirectoryInfo(path);
            ViewData["FileEntryCollection"] = di.GetFiles();
            return View();
        }
`



Code samples to integrate the plugin with Google App Engine Java.  
Contributed by [yamsellem](https://github.com/yamsellem).

* [index.html from the jQuery Upload Sample code](https://github.com/blueimp/jQuery-File-Upload/archives/master)

```html
<div id="fileupload">
  <form method="post" enctype="multipart/form-data">
    <div class="fileupload-buttonbar">
     <label class="fileinput-button">
      <span>Upload</span>
      <input type="file" name="files[]" multiple>
     </label>
    </div>
  </form>
  <div class="fileupload-content">
    <table class="files"></table>
  </div>
</div>

<script id="template-upload" type="text/x-jquery-tmpl">
...<!-- report all the templates from the jQuery Upload Sample code (link above) -->...
</script>
```

* application.js:

```javascript
$(function() {
  /* activate the plugin */
  $('#fileupload').fileupload();

  /* generate an App Engine url on each click */
  $('input:file', '#fileupload').button().click(function() {
    $.getJSON('/rest/file/url', function (response) {
      $('#fileupload form').prop('action', response.url);
      $('#fileupload').fileupload({
        add: function (e, data) {
          var that = this;
          data.url = response.url;
          /* configure the plugin with the /_ah/upload url */
          $.blueimpUI.fileupload.prototype.options.add.call(that, e, data);
        }
      });
    });
  });
});
```

* `/rest/file/url` returns a url bound to GAE (`/_ah/upload`) with a callback to our multipart POST method, `/rest/file`, [as specified here](http://code.google.com/intl/fr-FR/appengine/docs/java/blobstore/overview.html):
* `/rest/file` POSTing save the file and redirect to `/rest/file/meta`
* `/rest/file/meta` returns a JSON response [as specified here](https://github.com/blueimp/jQuery-File-Upload/wiki/Setup)

```java
@Path("/file")
public class FileResource {

  private final BlobstoreService blobstoreService = BlobstoreServiceFactory.getBlobstoreService();
  private final BlobInfoFactory blobInfoFactory = new BlobInfoFactory();
  
  /* step 1. get a unique url */
  
  @GET
  @Path("/url")
  public Response getCallbackUrl() {
    /* this is /_ah/upload and it redirects to its given path */
    String url = blobstoreService.createUploadUrl("/rest/file");
    return Response.ok(new FileUrl(url)).build();
  }
  
  /* step 2. post a file */
  
  @POST
  @Consumes(MediaType.MULTIPART_FORM_DATA)
  public void post(@Context HttpServletRequest req, @Context HttpServletResponse res) throws IOException, URISyntaxException {
    Map<String, BlobKey> blobs = blobstoreService.getUploadedBlobs(req);
    BlobKey blobKey = blobs.get("files[]");
    res.sendRedirect("/rest/file/" + blobKey.getKeyString() + "/meta");
  }
  
  /* step 3. redirected to the meta info */
  
  @GET
  @Path("/{key}/meta")
  public Response redirect(@PathParam("key") String key) throws IOException {
    BlobKey blobKey = new BlobKey(key);
    BlobInfo info = blobInfoFactory.loadBlobInfo(blobKey);

    String name = info.getFilename();
    long size = info.getSize();
    String url = "/rest/file/" + key; 
    FileMeta meta = new FileMeta(name, size, url);

    List<FileMeta> metas = Lists.newArrayList(meta);
    GenericEntity<List<FileMeta>> entity = new GenericEntity<List<FileMeta>>(metas) {};
    return Response.ok(entity).build();
  }

  /* step 4. download the file */
  
  @GET
  @Path("/{key}")
  public Response serve(@PathParam("key") String key, @Context HttpServletResponse response) throws IOException {
    BlobKey blobKey = new BlobKey(key);
    final BlobInfo blobInfo = blobInfoFactory.loadBlobInfo(blobKey);
    response.setHeader("Content-Disposition", "attachment; filename=" + blobInfo.getFilename());
    BlobstoreServiceFactory.getBlobstoreService().serve(blobKey, response);
    return Response.ok().build();
  }
  
  /* step 5. delete the file */
  
  @DELETE
  @Path("/{key}")
  public Response delete(@PathParam("key") String key) {
    Status status;
    try {
      blobstoreService.delete(new BlobKey(key));
      status = Status.OK;
    } catch (BlobstoreFailureException bfe) {
      status = Status.NOT_FOUND;
    }
    return Response.status(status).build();
  }
}
```

* the representations

```java
@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class FileUrl {
  String url;

  public FileUrl(String url) {
    this.url = url;
  }
}

@XmlRootElement
@XmlAccessorType(XmlAccessType.FIELD)
public class FileMeta {
  String name;
  long size;
  String url;
  String delete_url;  
  String delete_type;  

  public FileMeta(String filename, long size, String url) {
    this.name = filename;
    this.size = size;
    this.url = url;
    this.delete_url = url;
    this.delete_type = "DELETE";
  }

  public FileMeta() {
  }
}
```

* web.xml

```xml
<web-app>
  <servlet>
    <servlet-name>jersey</servlet-name>
    <servlet-class>com.sun.jersey.spi.container.servlet.ServletContainer</servlet-class>
    <init-param>
      <param-name>com.sun.jersey.api.json.POJOMappingFeature</param-name>
      <param-value>true</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>jersey</servlet-name>
    <url-pattern>/rest/*</url-pattern>
  </servlet-mapping>
</web-app>
```

* pom.xml

```xml
<dependency>
  <groupId>com.sun.jersey</groupId>
  <artifactId>jersey-server</artifactId>
  <version>1.7</version>
</dependency>
<dependency>
  <groupId>com.sun.jersey</groupId>
  <artifactId>jersey-json</artifactId>
  <version>1.7</version>
</dependency>
<dependency>
  <groupId>com.sun.jersey.contribs</groupId>
  <artifactId>jersey-multipart</artifactId>
  <version>1.7</version>
</dependency>
```
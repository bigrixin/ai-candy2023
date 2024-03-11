---
description: File Upload with process bar, using Jquery, JSON return
---

# Jquery File Upload

```
@model WebApp1.Models.MyModel
@{
    ViewBag.Title = "Upload";
}
<style>
    .progress {
        position: relative;
        border: 1px solid #ddd;
        padding: 1px;
        border-radius: 3px;
    }
</style>

<h2>Upload</h2>
<link href="~/Content/jQuery.FileUpload/css/jquery.fileupload.css" rel="stylesheet" />
<script src="~/Scripts/jquery-ui-1.12.1.min.js"></script>
<script src="~/Scripts/jQuery.FileUpload/jquery.fileupload.js"></script>
<script src="~/Scripts/jQuery.FileUpload/jquery.fileupload-ui.js"></script>
<script src="~/Scripts/jQuery.FileUpload/jquery.fileupload-process.js"></script>

@section scripts{
    <script type="text/javascript">
        jQuery.noConflict()(function ($) {
            $(document).ready(function () {
                $('.fileupload').fileupload({
                    dataType: 'json',
                    url: '/Home/UploadFiles',
                    autoUpload: true,
                    progressall: function (e, data) {
                        var progress = parseInt(data.loaded / data.total * 100, 10);
                        var thisId = $(this).attr('id');
                        // file1 ~ file5, bar1 ~ bar5
                        var barId = "bar" + thisId.substring(4);
                        $('#'+barId).css('width', progress + '%');
                    },
                    //start: function (e) {
                    //    $('.progress .progress-bar').css(
                    //        'width', '0%'
                    //    );
                    //},
                    fail: function (event, data) {
                        if (data.files[0].error) {
                            alert(data.files[0].error);
                        }
                    },
                    done: function (e, data) {

                        var thisId = $(this).attr('id');
                        var resultId = "result" + thisId.substring(4);
                        var cancelId = "cancelUpload" + thisId.substring(4);
                        
                        if (data.result.isUploaded) {
                            $('#' + resultId).html(data.result.message);
                            $('#' + cancelId).css('disply','none');
                          //  $('#' + cancelId).style = "display: block";
                        }
                    }
                })
            });

    
        });


    </script>
}


<div class="form-group">
    <div class="col-md-5">
        <span class="btn btn-success fileinput-button">
            <i class="glyphicon glyphicon-plus"></i>
            <span>Add file</span>
            <input id="file1" class="fileupload" type="file" name="files[]" multiple>
        </span>
    </div>
    <div class="col-md-2">
        <div class="progress">
            <div id="bar1" class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" style="width: 0%;">
                <div class="sr-only">0% complete</div>
            </div>
        </div>
    </div>
    <div class="col-md-1">
        <div id="result1"></div>
        <span class="text-danger"></span>
    </div>

    <div class="col-md-1">
        <button type="button" id="cancelUpload1" class="btn btn-danger delete">
            <i class="glyphicon glyphicon-trash"></i>
            <span>Cancel</span>
        </button>
    </div>
 
</div>

<br />

<div class="form-group">
    <div class="col-md-5">
        <span class="btn btn-success fileinput-button">
            <i class="glyphicon glyphicon-plus"></i>
            <span>Add file</span>
            <input id="file2" class="fileupload" type="file" name="files[]" multiple>
        </span>
    </div>
    <div class="col-md-2">
        <div class="progress">
            <div id="bar2" class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" style="width: 0%;">
                <div class="sr-only">0% complete</div>
            </div>
        </div>
    </div>
    <div class="col-md-1">
        <div id="result2"></div>
        <span class="text-danger"></span>
    </div>

    <div class="col-md-1">
        <button type="button" id="cancelUpload2" class="btn btn-danger delete" style="visibility:hidden;">
            <i class="glyphicon glyphicon-trash"></i>
            <span>Cancel</span>
        </button>
    </div>

    <div class="col-md-3">
        <span class="file_name"></span>
        <span class="file_type"></span>
        <span class="file_size"></span>
    </div>
</div>
```

```
   [HttpPost]
        public virtual ActionResult UploadFiles()
        {
            var r = new List<UploadFilesResult>();
            string path = Server.MapPath("~/App_Data");
            Directory.CreateDirectory(path);
            bool isUploaded = false;
            string message = "File upload failed";
            foreach (string file in Request.Files)
            {
                HttpPostedFileBase hpf = Request.Files[file] as HttpPostedFileBase;
                if (hpf.ContentLength == 0)
                    continue;
                string savedFileName = Path.Combine(path, Path.GetFileName(hpf.FileName));

                r.Add(new UploadFilesResult()
                {
                    Name = hpf.FileName,
                    Length = hpf.ContentLength,
                    Type = hpf.ContentType
                });

                try
                {
                    hpf.SaveAs(savedFileName);
                    isUploaded = true;
                    message = "Ok!";
                }
                catch (Exception ex)
                {
                    message = string.Format("Failed: {0}", ex.Message);
                }
            }

            return Json(new { isUploaded = isUploaded, message = message, name = r[0].Name, type = r[0].Type, size = r[0].Length }, "text/html");
        }

```

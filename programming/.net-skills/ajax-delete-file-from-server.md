# ajax delete file from server

```
	// delete file from server
	$.ajax({
		dataType: 'json',
		url: '/Patient/DeleteFile',
		type: 'DELETE',
		data: {'fileName': fileName },
		success: function (result) {
			alert(result.message);
		},
		error: function () {
			alert("error");
		}
	});
```

```
	[HttpDelete]
	public virtual ActionResult DeleteFile(string fileName)
	{
		if (this._uploadServices.DeleteFileFromServer(fileName))
			return Json(new { message = "The file has delete !" }, "text/html");
		else
			return Json(new { message = "Error" }, "text/html");
	}

```

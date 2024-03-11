# Upload file to Azure

```
In NuGet to install:
WindowsAzure.Storage
Microsoft.Azure.Management.Storage   ?? not need
Microsoft.WindowsAzure.ConfigurationManager
 
		[HttpPost]
		public virtual ActionResult UploadFileToAzure()
		{
			bool isUploaded = false;
			string path = ConfigurationManager.AppSettings["uploadAzurePath_Patient"];
			HttpPostedFileBase file = Request.Files[0];

			string url = this._uploadServices.UploadToAzureStorage(file, path);
			if (url != null)
			{
				isUploaded = true;
				string message = "100% complete";
				return Json(new
				{
					statusCode = 200,
					status = "File uploaded.",
					file = url,
					isUploaded = isUploaded,
					message = message
				}, "text/html");

			}
			else
				return Json(new
				{
					statusCode = 500,
					status = "Error uploading image.",
					file = string.Empty,
					isUploaded = isUploaded
				}, "text/html");


		}
		
		
	   [HttpDelete]
		public virtual ActionResult DeleteFile(string fileURL)
		{
			string path = ConfigurationManager.AppSettings["uploadAzurePath_Patient"];
			if (this._uploadServices.DeleteFromAzureStorage(fileURL, path))
				return Json(new { message = "The file has delete !" }, "text/html");
			else
				return Json(new { message = "Error" }, "text/html");
		}
		
```

```
	public string UploadToAzureStorage(HttpPostedFileBase file, string containerName)
		{
			if (file == null)
				return "False";

			//The container name must be lowercase.
			//		string containerName = "client";
			string pathFileName = getNewFileName(file);

			// Retrieve storage account from connection string.
			CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

			// Create the blob client.
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

			// Retrieve a reference to a container.
			CloudBlobContainer container = blobClient.GetContainerReference(containerName);

			// Create the container if it doesn't already exist.
			container.CreateIfNotExists();

			// Make container for public
			container.SetPermissions(new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

			// Retrieve reference to a blob named "myblob".
			CloudBlockBlob blockBlob = container.GetBlockBlobReference(pathFileName);

			// Reset the Stream to the Beginning before upload
			//MemoryStream memoryStream = new MemoryStream();
			//file.InputStream.CopyTo(memoryStream);
			//memoryStream.Seek(0, SeekOrigin.Begin);
			//memoryStream.ToArray();
			file.InputStream.Seek(0, SeekOrigin.Begin);
			blockBlob.UploadFromStream(file.InputStream);
			string fileURL = blockBlob.Uri.ToString();
			return blockBlob.Uri.ToString();
		}

		public void DeleteFromAzureStorage(string fileName, string containerName)
		{
			// Retrieve storage account from connection string.
			CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
					CloudConfigurationManager.GetSetting("StorageConnectionString"));

			// Create the blob client.
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

			// Retrieve reference to a previously created container.
			CloudBlobContainer container = blobClient.GetContainerReference(containerName);

			// Retrieve reference to a blob named "myblob.txt".
			CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

			// Delete the blob.
			blockBlob.Delete();
		}


		public void ListBlobItemFromAzure(string containerName)
		{
			// Retrieve storage account from connection string.
			CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
					CloudConfigurationManager.GetSetting("StorageConnectionString"));

			// Create the blob client.
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

			// Retrieve reference to a previously created container.
			CloudBlobContainer container = blobClient.GetContainerReference(containerName);

			// Loop over items within the container and output the length and URI.
			foreach (IListBlobItem item in container.ListBlobs(null, false))
			{
				if (item.GetType() == typeof(CloudBlockBlob))
				{
					CloudBlockBlob blob = (CloudBlockBlob)item;

					Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

				}
				else if (item.GetType() == typeof(CloudPageBlob))
				{
					CloudPageBlob pageBlob = (CloudPageBlob)item;

					Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

				}
				else if (item.GetType() == typeof(CloudBlobDirectory))
				{
					CloudBlobDirectory directory = (CloudBlobDirectory)item;

					Console.WriteLine("Directory: {0}", directory.Uri);
				}
			}
		}

		
		public bool DeleteFromAzureStorage(string fileURL, string containerName)
		{
			if (String.IsNullOrEmpty(fileURL))
				return false;
			int position = fileURL.IndexOf(containerName) + containerName.Length + 1;
			string fileName = fileURL.Substring(position);
			// Retrieve storage account from connection string.
			CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
					CloudConfigurationManager.GetSetting("StorageConnectionString"));

			// Create the blob client.
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

			// Retrieve reference to a previously created container.
			CloudBlobContainer container = blobClient.GetContainerReference(containerName);

			// Retrieve reference to a blob named "myblob.txt".
			CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

			if (blockBlob.Exists())
			{
				blockBlob.Delete();
				return true;
			}
			return false;
		}
		#endregion

		#region Helper

		private string getNewFileName(HttpPostedFileBase file)
		{
			// 	string mimeType = file.ContentType.ToLower();   //mimeType == "image/png", or "application/pdf"
			MD5 md5 = MD5.Create();
			var hashMD5 = md5.ComputeHash(file.InputStream);
			//For new file name prefix
			string fileNamePrefix = Guid.NewGuid().ToString();
			string fileMD5String = BitConverter.ToString(hashMD5).Replace("-", string.Empty);
			string path = fileMD5String.Substring(0, 3);  //sub directory
			string fileName = file.FileName.ToLower();
			//Use for long file name, cut to last 10 characters
			if (fileName.Length > 10)
				fileName = fileName.Substring(fileName.Length - 10);

			string newFileName = path + "/" + fileNamePrefix + "-" + fileName;
			return newFileName;
		}
		#endregion
```

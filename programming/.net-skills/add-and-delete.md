# Add and Delete

```
  JobController.cs
        [HttpPost, Route("job/create")]
        [ValidateAntiForgeryToken]
        public ActionResult Create(JobViewModel model, IEnumerable<HttpPostedFileBase> files)
        {
            if (ModelState.IsValid)
            {
                var client = this.GetLoggedInClient();
                /* 
                 * Get a timestamp as filename prefix, 
                 * before uploading file, change the filename, the new file name (URL) save in database.
                 * maybe change later
                 */
                string fileNamePrefix = this._uploadService.GetFilePrefix();
                string newPictureURL = UploadFile(files, fileNamePrefix);   //upload file and get path

                Job newJob = this._clientService.PostNewJob(client.ID, model.Title, model.Description, model.SuburbId, model.GenderId, model.ServiceId, model.ServicedAt, newPictureURL);

                return RedirectToAction("Details/" + newJob.ID.ToString());
            }

            return View(model);
        }
```

```
JobController.cs
        //upload file and return path
        private string UploadFile(IEnumerable<HttpPostedFileBase> files, string fileNamePrefix)
        {
            string ownString = "job";    //used for check picture name has change
            string path = ConfigurationManager.AppSettings["uploadPath_Job"];
            var uploadPath = Server.MapPath(path);
            var newPathWithFileName = "";

            if (files.FirstOrDefault() != null)
            {
                this._uploadService.UploadFile(files, ownString + fileNamePrefix, uploadPath);

                if (fileNamePrefix != null)
                    newPathWithFileName = path + "/" + ownString + fileNamePrefix + files.First().FileName;
            }
            return newPathWithFileName;
        }
```

```
UploadService.cs

        //generate timestamp string
        public string GetFilePrefix()
        {
            string filePrefix = "";
            long lastTimeStamp = DateTime.UtcNow.Ticks;
            filePrefix = lastTimeStamp.ToString();
            return filePrefix;
        }




        public void UploadFile(IEnumerable<HttpPostedFileBase> files, string fileNamePrefix, string uploadPath)
        {
            foreach (var file in files)
            {
                if (file != null && file.ContentLength > 0)
                {
                    var fileName = fileNamePrefix + file.FileName;
                    var path = Path.Combine(uploadPath, fileName);
                    Directory.CreateDirectory(uploadPath);
                    file.SaveAs(path);
                }
            }
            //  Response.Write("The file has been uploaded.");
        }

        public bool DeleteFileFromServer(string filePath)
        {
            var fullPath = HostingEnvironment.MapPath(filePath);
            if (!System.IO.File.Exists(fullPath))
                return false;
            try
            {
                System.IO.File.Delete(fullPath);
                return true;
            }
            catch (Exception e)
            {
                throw e;
            }
        }
```

```
ClientService.cs
        public Job PostNewJob(int ownerClientId, string title, string description, int suburbId, int genderId, int serviceId, DateTime? serviceAt, string pictureURL)
        {
            if (ownerClientId < 1)
                throw new ArgumentException("ownerClientId must be an ID greater than 1.");
            var client = this._entities.Single<Client>(c => c.ID == ownerClientId);
            if (client == null)
                throw new ArgumentNullException("client");
            var now = DateTime.Now;
            var clientId = client.ID;
            var job = new Job(clientId)
            {
                CreatedAt = now,
                UpdatedAt = now,
                Title = title,
                Description = description,
                SuburbId = suburbId,
                GenderId = genderId,
                ServiceId = serviceId,
                ServiceAt = serviceAt,
                PictureURL = pictureURL,
                JobStatus = "New"
            };

            client.PostedJobs.Add(job);
            this._entities.Update(client);
            this._entities.Save();
            return job;
        }

```

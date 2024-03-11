# PDFpig: Send Email with PDF attachment



```
// Some code

        private async Task sendEmailAsync()
        {
            //Add PDF attachment.
            using (Stream stream = new MemoryStream(CreatePDFFile()))
            {
                var emailMessage = new MimeMessage();
                emailMessage.From.Add(new MailboxAddress(null, _emailConfig.From));
                emailMessage.To.Add(new MailboxAddress(string.Empty, "steven.zhai@nwigroup.com.au"));
                emailMessage.Subject = "This is Subject";

                var content = "sample email attached PDF ";

                string emailBody = "this is email body";
                var bodyBuilder = new BodyBuilder { HtmlBody = string.Format(emailBody, content) };
                bodyBuilder.Attachments.Add("attachmentFilename.pdf", stream, ContentType.Parse("application/pdf"));

                emailMessage.Body = bodyBuilder.ToMessageBody();

                await SendAsync(emailMessage);
            }
        }


        private async Task SendAsync(MimeMessage mailMessage)
        {
            using (var client = new MailKit.Net.Smtp.SmtpClient())
            {
                client.ServerCertificateValidationCallback = (s, c, h, e) => true;

                try
                {
                    await client.ConnectAsync(_emailConfig.SmtpServer, _emailConfig.Port, SecureSocketOptions.Auto);   //true
                    client.AuthenticationMechanisms.Remove("XOAUTH2");
                    await client.AuthenticateAsync(_emailConfig.UserName, _emailConfig.Password);

                    await client.SendAsync(mailMessage);
                }
                catch
                {
                    //log an error message or throw an exception, or both.
                    throw;
                }
                finally
                {
                    await client.DisconnectAsync(true);
                    client.Dispose();
                }
            }
        }

        private byte[] CreatePDFFile()
        {
            PdfDocumentBuilder builder = new PdfDocumentBuilder();
            PdfPageBuilder page = builder.AddPage(PageSize.A4);
            PdfDocumentBuilder.AddedFont font = builder.AddStandard14Font(Standard14Font.CourierBold);

            page.AddText("This is a sample text.", 16, new PdfPoint(5, 300), font);

            byte[] documentBytes = builder.Build();

            return documentBytes;
        }

```

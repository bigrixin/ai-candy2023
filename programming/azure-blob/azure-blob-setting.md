# Azure blob setting



{% code fullWidth="true" %}
```
// appsettings.json

{
"BlobContainer": "image", "BlobUrl": "https://sample.blob.core.windows.net/",
"ConnectionStrings": { "AzureBlobConnectionString": "DefaultEndpointsProtocol=https;AccountName=nwiinternal;AccountKey=iDi7eCDrcAXkhWb+fsx2W/F74ucp+sFu76LmfkmnhKNm5Q8tum5a78nQzY5JHKdJrUaQfMHlVJ+EVrzR9OfgWA==;EndpointSuffix=core.windows.net" },
}
```
{% endcode %}



Storage account->blob name->

1. Containers-> -- get \[container name]
2. Endpoints ->Primary endpoint, Blob service -- get \[url]
3. Access keys-> -- get \[connection string]

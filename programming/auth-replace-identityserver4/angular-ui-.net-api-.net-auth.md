# Angular UI - .NET API - .NET Auth

{% code fullWidth="true" %}
```
// Auth
  InMemoryConfig.cs

        public static IEnumerable<Client> GetClients(string url_api, string url_ui) =>
             new List<Client>
             {
               new Client
               {
		  ClientId = "WWWebUI",
		   ....
		},
               new Client
               {
	   	  ClientId = "WWWeb_Api",
		   ....
               }
             }

 ...

        public static IEnumerable<ApiScope> GetApiScopes() =>
              new List<ApiScope> { new ApiScope("WWWeb_Api", "NWI Web API") };

        public static IEnumerable<ApiResource> GetApiResources() =>
            new List<ApiResource>
            {
                new ApiResource("WWWeb_Api", "NWI Web API")
                {
                    Scopes = { "WWWeb_Api" }
                }
            };


// UI
  account.service.ts
       param.set('client_Id', 'WWWebUI');

// API
  Startup.cs
       o.Audience = "WWWeb_Api";
```
{% endcode %}

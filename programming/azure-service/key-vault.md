---
icon: key-skeleton
---

# Key Vault



{% code fullWidth="true" %}
```
// Setting

Azure Key vaults

1. Azure portal
2. Entry App Services, find, and entry a API app
3. Under settings, click on Identity
4. Change the status to On

5. Entry Key Vaults, Create a key vault, in Objects, Secrets, Generate a secret key 
6. In Access policies
7. Click Create
8. Under the Secret Permission, select Get and List
9. Click Next, then, search the app resource of the API, click Next
11. Click Create

```
{% endcode %}

{% code fullWidth="true" %}
```
// Some setting in API app

1. Install package Azure.Security.KeyVault.Secrets

2. appsettings.json

{
  "Authentication": {
    "ApiKeyName": "portal-api-key",
    "ApiKeyUrl": "https://aaaa.vault.azure.net/",
    "ApiKeyHeaderName": "Api-Key"
  },
}

3. startup.cs
	// API key authentication
	services.AddMemoryCache();
	services.AddScoped<IKeyVaultService, KeyVaultService>();
	
4.  public class KeyVaultResponseViewModel
    {
        public string Value { get; set; }
    }
```
{% endcode %}

{% code fullWidth="true" %}
```
// IKeyVaultService

public interface IKeyVaultService
{
    Task<string> GetApiKey();
}


// KeyVaultService.cs

namespace Services.KeyVaultService
{
    public class KeyVaultService : IKeyVaultService
    {
        private readonly SecretClient _secretClient;
        private readonly IConfiguration _configuration;
        private readonly IMemoryCache _cache;
        private readonly TimeSpan _cacheDuration = TimeSpan.FromMinutes(10);
        public KeyVaultService(IConfiguration configuration, IMemoryCache cache)
        {
            _configuration = configuration;
            _secretClient = new SecretClient(new Uri(_configuration.GetValue<string>("Authentication:ApiKeyUrl")), new DefaultAzureCredential());
            _cache = cache;
        }
        public async Task<string> GetApiKey()
        {
            return await GetSecretAsync(_configuration.GetValue<string>("Authentication:ApiKeyName"));
        }
        private async Task<string> GetSecretAsync(string secretName)
        {
            // Save to cache
            return await _cache.GetOrCreateAsync(secretName, async entry =>
            {
                entry.AbsoluteExpirationRelativeToNow = _cacheDuration;
                entry.SlidingExpiration = TimeSpan.FromMinutes(5); // Refresh if accessed within 10 minutes
                try
                {
                    KeyVaultSecret secret = await _secretClient.GetSecretAsync(secretName);
                    return secret.Value;
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"Error fetching key: {ex.Message}");
                    return null;
                }
            });
        }
    }
}
```
{% endcode %}

{% code fullWidth="true" %}
```
// Controller

namespace System.API.Controllers
{
    [ApiController]
    [Authorize]
    public class KeyVaultController : ControllerBase
    {
        private readonly IKeyVaultService _keyVaultService;
            
        public KeyVaultController(IKeyVaultService keyVaultService)
        {
            _keyVaultService = keyVaultService;
        }
        
        [HttpGet]
        [Route("api/ApiKey")]
        public async Task<IActionResult> GetApiKey()
        {
            KeyVaultResponseViewModel result = new();
            result.Value = await _keyVaultService.GetApiKey();
            if (result == null)
            {
                return NotFound();
            }
            return Ok(result);
        }
    }
}
```
{% endcode %}



{% code fullWidth="true" %}
```
// UI

export const constants = {
  export enum PortalStatus {
    Normal,
    NearlyExpired,
    Expired,
  }
}

// key-vault.servie.ts
@Injectable({
  providedIn: 'root',
})
export class KeyVaultService {
  rootURL = environment.backEndAPIEndpoint;
  baseURL = `${this.rootURL}api/keyvault/`;
  reqHeader = new HttpHeaders({
    'content-type': 'application/json',
    charset: 'utf-8',
  });

  private apiKeyVaultSubject = new BehaviorSubject<string>('');
  public apiKeyVaultSubject$ = this.apiKeyVaultSubject.asObservable();

  constructor(private httpClient: HttpClient, public router: Router) {}

  getApiKey(): Observable<any> {
    return this.httpClient.get<any>(`${this.baseURL}apikey`, {
      headers: this.reqHeader,
    });
  }
  updateApiValue(newValue: string) {
    this.apiKeyVaultSubject.next(newValue);
  }
}

///environment.ts
{
  customerAPIEndpoint: 'https://portal-api.azurewebsites.net/',
  // Change this based on the code in the customer portal
  portalCode: '001',
  settingCode: 'S001',
  apiKeyHeaderName: 'Api-Key',
};


//expired-verification.service.ts
@Injectable({
  providedIn: 'root'
})
export class ExpiredVerificationService {
  rootURL = `${environment.customerAPIEndpoint}`;
  constructor(private httpClient: HttpClient, public router: Router) {}

  getExpiredVerificationInfo(apiKey: string): Observable<ExpiredVerification> {
    let baseUrl = `${this.rootURL}api/v2/Portal/expirydate/`;
    const headers = new HttpHeaders().set(environment.apiKeyHeaderName, apiKey);
    return this.httpClient
      .get<any>(`${baseUrl}${environment.portalCode}`, { headers })
      .pipe(map((res) => res));
  }

  getSettingDueNumberOfDays(apiKey: string): Observable<any> {
    let baseUrl = `${this.rootURL}api/v2/Setting/`;
    const headers = new HttpHeaders().set(environment.apiKeyHeaderName, apiKey);
    return this.httpClient
      .get<any>(`${baseUrl}${environment.settingCode}`, { headers })
      .pipe(map((res) => res));
  }
}


```
{% endcode %}



{% code fullWidth="true" %}
````
// ts
import { PortalStatus } from 'src/app/constants';
import { KeyVaultService } from 'src/app/services/key-vault.service';
 
@Component({
  selector: 'app-expired-warning',
  templateUrl: './expired-warning.component.html',
  styleUrls: ['./expired-warning.component.scss'],
})
export class ExpiredWarningComponent implements OnInit {
  portalStatus = PortalStatus;
  expiryStatus = PortalStatus.Normal;
  expiryNumberOfDaysSubject = new BehaviorSubject<number>(0);

  constructor(
    private expiredVerificationService: ExpiredVerificationService,
    private keyVaultService: KeyVaultService
  ) {}

  ngOnInit(): void {
    // Get the API key first
    this.getApiKey();
    this.keyVaultService.apiKeyVaultSubject$.pipe(skip(1)).subscribe({
      next: (res) => {
        // Call the other API that need the API Key
        this.getExpiredVerificationInfo(res);
        this.getSettingDueNumberOfDays(res);
      },
    });
  }
  
  getExpiredVerificationInfo(apiKey: string) {
    // skip the initial value from behavior subject
    this.expiryNumberOfDaysSubject.pipe(skip(1)).subscribe((value) => {
      this.expiredVerificationService
        .getExpiredVerificationInfo(apiKey)
        .subscribe({
          next: (res) => {
            let endDate = res.endDate;
            var expiryDate = new Date(endDate);
            let currentDate = new Date();
            if (currentDate > expiryDate) {
              this.expiryStatus = PortalStatus.Expired;
            } else {
              // two weeks
              const twoWeeksInMs = value * 24 * 60 * 60 * 1000;
              if (
                expiryDate.getTime() - currentDate.getTime() <=
                twoWeeksInMs
              ) {
                this.expiryStatus = PortalStatus.NearlyExpired; // < two weeks
              }
            }
          },
          error: (error) => {
            console.log(
              'Error: retrive expired verification info error' + error
            );
          },
        });
    });
  }

  getSettingDueNumberOfDays(apiKey: string) {
    this.expiredVerificationService
      .getSettingDueNumberOfDays(apiKey)
      .subscribe({
        next: (res) => {
          this.expiryNumberOfDaysSubject.next(Number(res.value));
          // this.expiryNumberOfDaysSubject.next(res[0].value);
        },
        error: (error) => {
          console.log('Error: retrive expired verification info error' + error);
        },
      });
  }
  getApiKey() {
    this.keyVaultService.getApiKey().subscribe({
      next: (res) => {
        // Allows the other API to be called once the API key is retrieved.
        this.keyVaultService.updateApiValue(res.value);
      },
      error: (error) => {
        console.log('Error: retrive expired verification info error' + error);
      },
    });
  }
}

```
````
{% endcode %}

{% code fullWidth="true" %}
````
// Some code

```html
@if(expiryStatus == portalStatus.NearlyExpired) {
  <div>
      <div class="card" style="background: orange; color:rgb(16, 91, 161)">
          <a href="#/dashboard" class="btn btn-block btColor-black" style="padding-top: 30px;">
              <div><i class="fa fa-balance-scale p"></i></div>
              <div>Our records indicate your Portal subscription is due to expire. </div>
              <div>Please contact your local service center representative to renew.</div>
          </a>
      </div>
  </div>
  } @else if(expiryStatus == portalStatus.Expired) {
  <div>
      <div class="card" style="background: rgb(235, 57, 13); color:yellow">
          <a href="#/dashboard" class="btn btn-block btColor-black" style="padding-top: 30px; padding-bottom:200px">
              <div><i class="fa fa-balance-scale p"></i></div>
              <div>Our records indicate your Portal subscription has expired. </div>
              <div>Please contact your local service center representative to renew </div>
          </a>
      </div>
  </div>
  }

```
````
{% endcode %}

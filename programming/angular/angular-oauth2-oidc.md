# Angular oauth2 OIDC

&#x20;install oauth2 in Angular&#x20;

```
npm i angular-oauth2-oidc --save
npm i angular-oauth2-oidc-jwks --save
```

add service --AuthGuard

&#x20;\app ng g service shared/services/authguard --skip-tests

```
import { Injectable } from '@angular/core';
import { OAuthService } from 'angular-oauth2-oidc';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';

@Injectable({
  providedIn: 'root'
})
export class AuthguardService implements CanActivate {

  constructor(
	private oauthService: OAuthService,
	private router: Router) { }

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): boolean {
	// Check to see if we have an application user
	// If we dont, we navigate to [/]
	if (this.oauthService.hasValidAccessToken()) {
	  return true;
	}

	this.router.navigate(['/']);
	return false;
  };
}
```



add service&#x20;

ng g service shared/services/globals --skip-tests

```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class GlobalsService {

  constructor() { }

  IdentityServer_Http_URI: string = 'http://localhost:5000';
  IdentityServer_Https_URI: string = 'https://localhost:5001';
  WebAPI_Http_URI: string = 'http://localhost:5002';
  WebAPI_Https_URI: string = 'https://localhost:5003';
  WebApp_URI: string = 'http://localhost:4200';
  WebApp_Redirect_URI: string = 'http://localhost:4200';
  WebApp_Post_Logout_Redirect_URI: string = 'http://localhost:4200';

  Client_Id: string = 'WWWebUI';    ///<<<<
  Client_Scopes: string = 'openid profile roles api.resource.scope';
}
```



add service&#x20;

ng g service shared/services/http.service --skip-tests

```
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable, Subject } from 'rxjs';
import { GlobalsService } from './globals.service';

@Injectable({
  providedIn: 'root'
})
export class HttpService {
  response: string = '';
  private $apiSubject = new Subject<string>();

  constructor(private httpClient: HttpClient,
	private globalsService: GlobalsService) {
  }

  requestWeatherForecast() {
	this.httpClient.get<string>(`${this.globalsService.WebAPI_Https_URI}/WeatherForecast`)
	  .subscribe((response: string) => {
		this.$apiSubject.next(JSON.stringify(response));
	  });
  }

  getWeatherForecast(): Observable<string> {
	return this.$apiSubject.asObservable();
  }
}
```



importing the NgModule&#x20;

app.module.ts

```
@NgModule({
  imports: [
	// etc.
	HttpClientModule,
	OAuthModule.forRoot()   <<<<  add this
```



login / logout - app.component.ts&#x20;

app.component.ts

```
import { Component, OnDestroy, OnInit } from '@angular/core';
import { Router } from '@angular/router';
import { OAuthService, OAuthEvent } from 'angular-oauth2-oidc';
import { JwksValidationHandler } from 'angular-oauth2-oidc-jwks';
import { Subscription } from 'rxjs';
import { AuthConfig } from 'angular-oauth2-oidc';
import { GlobalsService } from './services/globals.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent implements OnInit, OnDestroy {
  claims: any;
  accessToken = '';

  $oauthSubscription: Subscription;

  authConfig: AuthConfig = {
	issuer: this.globalsService.IdentityServer_Https_URI,
	redirectUri: window.location.origin,
	clientId: this.globalsService.Client_Id,
	scope: this.globalsService.Client_Scopes,
	postLogoutRedirectUri: this.globalsService.WebApp_Post_Logout_Redirect_URI
  }

  constructor(private oauthService: OAuthService,
	private router: Router,
	private globalsService: GlobalsService) {
	this.$oauthSubscription = this.oauthService.events.subscribe((event: OAuthEvent) => {
	  this.accessToken = this.oauthService.getAccessToken();
	});
  }

  ngOnInit() {
	this.configureWithNewConfigApi();
  }

  ngOnDestroy() {
	this.$oauthSubscription.unsubscribe();
  }

  private configureWithNewConfigApi() {
	this.oauthService.configure(this.authConfig);
	this.oauthService.tokenValidationHandler = new JwksValidationHandler();   //<<
	
    // skipping login
	this.oauthService.loadDiscoveryDocumentAndTryLogin().then(_ => {
	  if (!this.oauthService.hasValidIdToken() ||
		!this.oauthService.hasValidAccessToken()) {
		this.oauthService.initImplicitFlow();          //<< initialize
	  } else {
		this.router.navigate(['/']);
	  }
	})
  }

  logout() {
	this.oauthService.logOut();    //<<<<< logout
  }

  get isAuthenticated(): boolean {
	this.claims = this.oauthService.getIdentityClaims();
	return this.claims !== undefined &&
	  this.claims !== null;
  }
}
```



login / logout - app.component.html&#x20;

```
<div *ngIf="isAuthenticated">
  <button [routerLink]="['/data']" class="btn btn-primary">Get Data from WebAPI</button>
  <button (click)="logout()" class="btn btn-primary">Log Out</button>
  <h1>Welcome to your Angular Application!</h1>
  <p>access token: {{accessToken}}</p>
</div>

<router-outlet></router-outlet>
```



app-routing.module.ts

```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { DataComponent } from './data/data.component';
import { HomeComponent } from './home/home.component';
import { AuthguardService } from './services/authguard.service';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'data', component: DataComponent, canActivate: [AuthguardService], runGuardsAndResolvers: "always" },
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```



References

```
Documentation:
   https://openbase.com/js/angular-oauth2-oidc/documentation
   
Community-provided sample implementation
   https://github.com/manfredsteyer/angular-oauth2-oidc

Angular Authentication with OpenID Connect and Okta in 20 Minutes
   https://developer.okta.com/blog/2017/04/17/angular-authentication-with-oidc
   
Example angular-oauth2-oidc with AuthGuard
	https://github.com/jeroenheijmans/sample-angular-oauth2-oidc-with-auth-guards/
	
Configuring for Implicit Flow
	https://manfredsteyer.github.io/angular-oauth2-oidc/docs/additional-documentation/using-implicit-flow.html
```

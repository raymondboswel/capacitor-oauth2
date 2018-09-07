# Capacitor OAuth 2 client plugin 

[![npm](https://img.shields.io/npm/v/@teamconductor/capacitor-oauth2.svg)](https://www.npmjs.com/package/@teamconductor/capacitor-oauth2) 
[![npm](https://img.shields.io/npm/dt/@teamconductor/capacitor-oauth2.svg?label=npm%20downloads)](https://www.npmjs.com/package/@teamconductor/capacitor-oauth2)

This is a simple OAuth 2 client letting you configure everything instead of using 
the OAuth providers SDKs in favor of better performance because especially on the web 
no additional javascript must be loaded.

## Installation

`npm i @teamconductor/capacitor-oauth2`

## Api

## Web

The following example is part of a **Angular 6** application authenticating against Facebook and Google.

```typescript
import {OAuth2AuthenticateResult, OAuth2Client} from '@teamconductor/capacitor-oauth2';

facebookLogin() {
    OAuth2Client.authenticate({
        appId: environment.fbAppId,
        authorizationBaseUrl: "https://www.facebook.com/v2.11/dialog/oauth",
        resourceUrl: "https://graph.facebook.com/v2.11/me",
        web: {
            redirectUrl: this.getRedirectUrl(),
            windowOptions: this.OAUTH_WINDOW_OPTIONS
        }
  }).then(result => {
        this.authenticateBackend("FACEBOOK", result);
  }).catch(reason => {
        console.error("FB OAuth rejected", reason);
  });
}

googleLogin() {
    OAuth2Client.authenticate({
        appId: environment.googleAppId,
        authorizationBaseUrl: "https://accounts.google.com/o/oauth2/auth",
        scope: 'https://www.googleapis.com/auth/userinfo.profile',
        resourceUrl: "https://www.googleapis.com/userinfo/v2/me",
        web: {
            redirectUrl: this.getRedirectUrl(),
            windowOptions: this.OAUTH_WINDOW_OPTIONS
        },
    }).then(result => {
        this.authenticateBackend("GOOGLE", result);
    }).catch(reason => {
        console.error("Google OAuth rejected", reason);
    });
}

private authenticateBackend(provider: string, result: OAuth2AuthenticateResult) {
    let oauthData = new DemoLoginData();
    oauthData.provider = provider;
    oauthData.oauthId = result.id;
    oauthData.name = result.name;
    if ("GOOGLE" === provider) {
        oauthData.email = result["email"];
        oauthData.fn = result["given_name"];
        oauthData.ln = result["family_name"];
    }
    
    this.backendService.processOAuth(oauthData).subscribe(
        jwtToken => {
        
        },
        error => {
            this.loginFailed = true;
        }
    );
}

private getRedirectUrl(): string {
  return window.location.protocol+"//"+window.location.hostname+":"+window.location.port;
}

```

## Android

The simple [ScribeJava](https://github.com/scribejava/scribejava) OAuth client library for android is used as only dependency.
 

## iOS

 - ETA October 2018
 
## Contribute

### Fix a bug or create a new feature

Please do not mix more than one issue in a feature branch. Each feature/bugfix should have its own branch and its own Pull Request (PR).

1. Create a issue and describe what you want to do at [Issue Tracker](https://github.com/moberwasserlechner/capacitor-oauth2/issues)
2. Create your feature branch (`git checkout -b feature/my-feature` or `git checkout -b bugfix/my-bugfix`)
3. Test your changes to the best of your ability. 
5. Commit your changes (`git commit -m 'Describe feature or bug'`)
6. Push to the branch (`git push origin feature/my-feature`)
7. Create a Github pull request

### Code Style

This repo includes a .editorconfig file, which your IDE should pickup automatically.

If not please use the sun coding convention. Please do not use tabs at all!

Try to change only parts your feature or bugfix requires.
 
## License

MIT. Please see [LICENSE](https://github.com/moberwasserlechner/capacitor-oauth2/blob/master/LICENSE).

## Team Condcutor

This feature is powered by [Team Conductor](https://team-conductor.com/en/) - Next generation club management platform.

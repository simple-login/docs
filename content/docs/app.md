# App

## What is a SimpleLogin App?

A SimpleLogin App represents a **user database**. For example, if you have an iOS, Android and, a Web app for a single product, then you just need a single SimpleLogin App. The same applies if you have 2 different products that share the same user database.

## Create an App

Please head to https://app.simplelogin.io/developer/ and click on **Create new app** to create a new app. You only need to provide a name.

![](/images/dev1.png)

All app fields are optional. However, we recommend that you provide as much information as possible so that users can easily recognize your app. Teaser: a **showcase** feature is in progress and once the feature is ready, you can submit your app to the showcase to promote it to all SimpleLogin users.

Depending on the flow you choose (`code` or `implicit`), you would need to store either:

- both `OAuth client id` and `OAuth client secret` if you decide to use `code flow`

- or just `OAuth client id` if you choose `implicit flow`
For conciseness, `AppID` and `OAuth client id` are interchangeable and so are `AppSecret` and `OAuth client secret`.

## App Redirect URI

This is the most important field in your app! By default, SimpleLogin **whitelists** all `http[s]://localhost:*` address to facilitate local development but you need to set your app `Redirect URI` before deploying your app to production: in effect, SimpleLogin will refuse to present the **Authorization Page** to users if it doesn't recognize the `redirect_uri` query passed in the `url`. You can add as many `redirect uri` queries as you want. Please note that all `Redirect URI`s except localhost need to be **https** for security reason.

![](/images/redirect-uri.png)

When errors happen during the authorization flow, SimpleLogin redirects users back to `redirect_uri?error={error_code}` so that your app can act accordingly. Please find the list of all errors in [Error Codes]({{< relref "/docs/errors.md" >}}).

## Libraries/Framework

SimpleLogin supports the following popular libraries (more libraries are planned):

- Vanilla JS (`Implicit Flow`): [hello.js](https://github.com/MrSwitch/hello.js), [JSO](https://github.com/andreassolberg/jso)
- Python (Flask and other non-Django frameworks): [Requests-OAuthlib](https://github.com/requests/requests-oauthlib)
- Django: [social-auth-app-django](https://github.com/python-social-auth/social-app-django)
- NodeJS [Passport](https://github.com/jaredhanson/passport)

It's impossible to cover all social login libraries for all languages. As SimpleLogin fully implements the OAuth2/OpenID Connect standard, any library compatible with these standards should be able to handle SimpleLogin with the following OAuth2/OpenID Connect endpoints:

- Authorization endpoint: https://app.simplelogin.io/oauth2/authorize
- Token endpoint: https://app.simplelogin.io/oauth2/token
- UserInfo endpoint: https://app.simplelogin.io/oauth2/userinfo

You can find dedicated guides for some frameworks/languages in the **Guides** section.

You can also use libraries that don't support OAuth/OpenID Connect too! Please head to

- [Code Flow - The raw way]({{< relref "/docs/code-flow.md" >}}) for implementing the `Code Flow`
- [Implicit Flow - The raw way]({{< relref "/docs/implicit-flow.md" >}}) for implementing the `Implicit Flow`





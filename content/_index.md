---
title: Introduction
type: docs
url: "/"
---

# SimpleLogin Docs

Welcome to SimpleLogin, the most developer-friendly social login solution!

Built by developers who are frustrated with the confusing and unnecessarily complicated experiences provided by giants like Google, Facebook, Twitter, etc , SimpleLogin strikes to have the best experiences for developers. And of course, protect user privacy at the same time. For more information, please consult our website at https://simplelogin.io

Here you'll find handy documentation about how to add the **Login with SimpleLogin** button to your app or website using either SimpleLogin SDKs or third-party libraries.

If you think there is something missing, please <a href="mailto:hi@simplelogin.io">let us know!</a>

## Quick Start

Let's start by a quick example using SimpleLogin SDK.

a. Create a `index.html` file that has a **Login with SimpleLogin** button

```html
<button id="btn-simplelogin">
    Login with SimpleLogin
</button>

<div id="user-info"></div>

<!-- Include SimpleLogin JS SDK -->
<script src="https://simplelogin.io/sdk/sdk.js"></script>

<script>
// Init SimpleLogin. "quickstart" is the SimpleLogin AppID
SL.init("quickstart");

// Call SL.login when user clicks on the login button
// Upon successful login, you will have access to the user information
document.getElementById("btn-simplelogin").onclick = function(e) {
  SL.login(function(user) {
    console.log("got user from SL SDK", user);

    document.getElementById("user-info").innerHTML = `
    email: ${user.email} <br>
    name: ${user.name} <br>
    avatar: <img src="${user.avatar_url}">
    `
  })
}
</script>
```

b. Run a local server, you can use any static server, for example Python `http.server` module:

> python3 -m http.server

or NodeJS `http-server` module

> npx http-server -p 8000

c. Visit http://localhost:8000, you should now be able to login via SimpleLogin!

## User Info

Using SimpleLogin you will get the following user information:

- `email`: either user's personal email address or an email **alias** user creates on SimpleLogin. All emails sent to an alias will be forwarded to user personal email so you can safely use this email to send important information to users. And please make sure to not send too much emails (aka spam) to users 😉.
- `name`: usually has the format `first last` but this is not guaranteed ([some countries do not have surname](https://www.quora.com/Which-countrys-people-dont-have-a-surname)).
- `avatar_url` (optional): only if user has uploaded their profile picture. Please note that this url is only **temporary** and will be expired in 1 week. We recommend downloading and storing the avatar. You can also just ask user to re-login in less than a week.

The next section quickly introduces OAuth2 and what `flow` should be used. If you already have strong understanding on `OAuth2`, please head directly to [App]({{< relref "/docs/app.md" >}})

## Overview

From the user point of view, the login experience is consisted of 2 steps:

1. User clicks on **Login with SimpleLogin** button, gets redirected to SimpleLogin authorization page. User will be asked if they want to share their information with your app/website.

2. User accepts, gets redirected back to your application and are authenticated.

![](/images/user-flow.png)

## Flow

From your app's point of view, the flow is the following:

1. User clicks on **Login with SimpleLogin**, you app generates a redirection url to SimpleLogin that contains information about your website/app, e.g. `https://app.simplelogin.io/oauth2/authorize?client_id={AppID}&redirect_uri={your_callback_url}`
where `{AppID}` is your SimpleLogin AppId and `{your_callback_url}` is the url that user will be redirected back in the next step.

2. User approves sharing data with your app, gets redirected back with `{your_callback_url}` along with a special `grant` that allows you to get user information. At this point there are 2 possibilities:

   * the `grant` is an `access token`, in this case it's the `implicit flow`: you can use `access token` to call `user info endpoint` at https://app.simplelogin.io/oauth2/user_info and gets the user info.
   * the `grant` is a `code`, in this case it's the `code flow`: you need an additional step to get the `access token` by calling the `token endpoint` at https://app.simplelogin.io/oauth2/token using `code`. The next step is the same as the previous case where you can get user info using the `access token`.

## Code Flow

If you have access to your back-end (Php, Python, etc), we recommend using `Code Flow` which is more secure: the `code` is transferred using browser redirection but the `access token` is exchanged in a backend-to-backend call.

The url that user is redirected to after the first step would be `{your_callback_url}?code={code}`

## Implicit Flow

If you don't have access to your back-end or it is difficult to modify back-end code, you should use the `Implicit Flow` where  the `access token` is sent via browser redirection.

The url that user is redirected after the first step would be `{your_callback_url}#access_token={access_token}`. Please note that here `#` (fragment) is used instead of `?` (query) to avoid the `access token` from hitting your back-end which should have nothing to do with it.

Please note that the `implicit flow` will probably be replaced soon by the [PKCE](https://tools.ietf.org/html/draft-ietf-oauth-security-topics-11#section-2.1.1). Support for `PKCE` is on SimpleLogin roadmap. Its use will be transparent in SimpleLogin SDK, meaning that you don't need to change anything.

## Standing on the shoulders of giants

SimpleLogin follows the protocols OAuth2 and OpenID Connect, the same standards that power Facebook/Google/Apple/etc login. SimpleLogin is therefore compatible with almost all libraries that support these protocols. The integration consists most of the time of putting the right endpoints 😉

Now please head directly to [App]({{< relref "/docs/app.md" >}}) to create your first SimpleLogin App!



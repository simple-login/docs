---
title: Introduction
type: docs
url: "/"
---

# SimpleLogin Docs

Welcome to SimpleLogin, the most developer-friendly social login solution!

Built by developers who are frustrated with the confusing and unnecessarily complicated experiences provided by giants such as Google, Facebook, Twitter, etc , SimpleLogin strikes to have the best experiences for developers. And of course, protect user privacy at the same time. For more information, please consult our website at https://simplelogin.io

Here you'll find handy documentation about how to add the **Login with SimpleLogin** button to your app or website using either SimpleLogin SDKs or libraries.

If you think there is something missing, please <a href="mailto:hi@simplelogin.io">let us know!</a>

## Quick Start

Let's start by a quick example using SimpleLogin SDK.

a. Create a `index.html` file that has a "Login with SimpleLogin" button

```html
<button id="btn-simplelogin">
    Login with SimpleLogin
</button>

<div id="user-info"></div>

<!-- Include SimpleLogin JS SDK -->
<script src="https://simplelogin.io/sdk/sdk.js"></script>

<script>
// Init SimpleLogin. "quickstart" is the appID
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

b. Run a local server, you can use any static server:

Python `http.server` module:

> python3 -m http.server

or NodeJS `static-server` tool, can be installed via `npm -g install static-server`

> static-server -p 8000

c. Visit http://localhost:8000, you should be able to login via SimpleLogin.

The next section presents quickly about OAuth2 and what `flow` should be used. If you already know very well `OAuth2`, please head directly to [App]({{< relref "/docs/app.md" >}})

## Overview

From the user point of view, the login experience is consisted of 2 steps:

1. user clicks on **Login with SimpleLogin** button, gets redirected to SimpleLogin authorization page.

2. user authorizes your application, gets redirected back to your application and is authenticated. User can now have access to content restricted to authenticated user in your application.

![](/images/user-flow.png)


## Flow

From your app's point of view, the flow is the following:

1. User clicks on **Login with SimpleLogin**, you app generates a redirection url to SimpleLogin that contains information about your app, e.g. `https://app.simplelogin.io/oauth2/authorize?client_id={your_app_id}&redirect_uri={your_callback_url}
where `{your_app_id}` is your SimpleLogin AppId and `{your_callback_url}` is the url that user will be redirected back in the next step.

2. User approves sharing data with your app, gets redirected back with `{your_callback_url}` along with a special `grant` that allows you to get user information such as user email, user name, etc. At this point there are 2 possible cases:

   * the `grant` is an `access token`, in this case it's the `implicit flow`: you can use `access token` to call `user info endpoint` at https://app.simplelogin.io/oauth2/user_info and gets the user info.
   * the `grant` is a `code`, in this case it's the `code flow`: you need an additional step to get the `access token` by calling the `token endpoint` at https://app.simplelogin.io/oauth2/user_info using `code`. The next step is the same as the previous case where you can get user info using the `access token`.

## Code Flow

If you have access to your back-end (Php, Python, etc), we recommend using `Code Flow` which is more secure: the `code` is transferred using browser redirection but the `access token` is exchanged in a backend-to-backend call.

The url that user is redirected after the first step would be `{your_callback_url}?code={code}`

## Implicit Flow

If you don't have access to your back-end or it is difficult to modify back-end code, you should use the `Implicit Flow` where  the `access token` is sent via browser redirection.

The url that user is redirected after the first step would be `{your_callback_url}#access_token={access_token}`. Please note that here `#` (fragment) is used instead of `?` (query) to avoid the `access token` from hitting your back-end which should have nothing to do with it.

SimpleLogin follows the protocols OAuth2 and OpenID Connect, the same standards that power Facebook/Google/Apple/etc login. SimpleLogin is therefore compatible with almost all libraries that support these protocols. The integration consists most of the time of putting the right endpoints 😉

Now please head directly to [App]({{< relref "/docs/app.md" >}}) to create your first SimpleLogin App!



---
layout: doc
title: Payment Providers
permalink: /payment-providers
---
# Payment Provider API
**NOTE:** This page is under heavy development and is subject to change.

Sileo provides a secure and seamless payment API to improve user and developer experience. Learn about implementing it now. This page is split into two sections: API's that have to be implemented on the repo base URL and API's that have to be implemented to process payments.

---

## For Repos:
### Payment Vendor
`GET /payment_endpoint`: Returns plain text base URL for payment vendor
**Note: Payment Vendors must use HTTPS**

```
https://my.chariz.io/
```

### Download Considerations

When a download of a package on a payment-providing repo is requested by an authenticated user in Sileo, the default download URL provided by your `Packages` file will not be immediately used. Read about [payment vendor downloads](#downloads) to see what is performed.

---

## For Payment Vendors:
### Vendor Info
```GET /info```: Get Information about payment Vendor

**Response:**
```json
{
   "name": "Chariz",
   "icon": "https://my.chariz.io/Icon.png",
   "description": "Chariz Pay",
   "authentication_banner": {
      "message": "Sample sign in prompt text",
      "button": "Sign in"
   }
}
```

**Authentication banner:** You can provide a two strings for the authentication banner, one for the sign in button, and one for the main body message. These will be displayed to signed out Sileo users viewing your repo screen. This section should be provided whether or not the user is signed in, as Sileo will decide whether or not to display it. If left out, a banner will not be shown, and users must authenticate from user settings or when tapping buy. Your button text will be capitalized by Sileo.

Here's what that will look like in Sileo:

<picture>
  <source
      srcset="/img/loginbanner-dark.png"
      media="(prefers-color-scheme: dark)"
  />
  <img
      src="/img/loginbanner.png"
      width="400"
      height="auto"
  />
</picture>

<br>

### User Info
`POST /user_info`: Get the user's information and a list of purchased items available for display

**Request:**
```json
{
    "token": "BEARER f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2"
}
```

**Response:**

```json
{
   "items": ["com.23aaron.zeze", "xyz.royalapps.jellyfish"],
   "user": {
      "name": "Ayden Panhuyzen",
      "email": "ayden@example.com"
   }
}
```

### Sign Out
`POST /sign_out`: Invalidate authentication token and log out

**Request:**
```json
{
    "token": "BEARER f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2"
}
```

**Response:**

```json
{
   "success": true
}
```

### Authenticate/Device Link
**In Safari Auth Session:**

`GET /authenticate?udid=4e1243bd22c66e76c2ba9eddc1f91394e57f9f83&model=iPhone7%2C2`: Begin login and/or authentication

**Callback:**

`sileo://authentication_success?token=BEARER%20f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2&payment_secret=jr38tgh9t832gew89gt3j8y4hjgmf92r1jt38gfhrq5jtwyhsgfekart0gh9fet8yhrgw89e3qw6h4gfn5ty5hgrfgh34ty5894g`

#### Payment Secret: Client Side Implementation Details

This payment secret should be stored in the keychain and require authentication (such as the device passcode, Touch ID, or Face ID) to be retrieved, to prevent the user from unauthorized transactions by tweaks or other malware. You can [read more about how that's done here](https://developer.apple.com/documentation/localauthentication/accessing_keychain_items_with_face_id_or_touch_id?language=objc).

### Package Info

`POST /package/:package_id/info`: 

**Note:** token is _optional_

**Note 2:** To make Sileo do the POST request (for a paid package), be sure that you added the tag `cydia::commercial` inside the `Tag` key of the `control` file of your tweak or to your repo's `Packages` file. Here's a (shortend) example package entry for ShortLook.

```
Package: co.dynastic.ios.tweak.shortlook
Name: ShortLook
...
Tag: cydia::commercial
```

**Request:**
`/package/org.coolstar.classicfolders2/info`
```json
{
    "token": "BEARER f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",
    "udid": "4e1243bd22c66e76c2ba9eddc1f91394e57f9f83",
    "device": "iPhone7,2"
}
```

**Response (Success):**
```json
{
   "price": "$1.99",
   "purchased": false,
   "available": true
}
```

**Response (Error):**
```json
{
   "available": false,
   "error": "Error string here",
   "recovery_url": "https://my.chariz.io/restricted_package/"
}
```

### Initiate Purchase (Post-Touch ID/Face ID)

`POST /package/:packageid/purchase`: 

**Request:**
`/package/org.coolstar.classicfolders2/purchase`
```json
{
    "token": "BEARER f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",
    "payment_secret": "jr38tgh9t832gew89gt3j8y4hjgmf92r1jt38gfhrq5jtwyhsgfekart0gh9fet8yhrgw89e3qw6h4gfn5ty5hgrfgh34ty5894g"
}
```

**Possible Responses:**

**Immediate Success:**
```json
{
   "status": 0
}
```

**Action Required: (See purchase action)**
```json
{
   "status": 1,
   "url": "https://my.chariz.io/transaction/stripe/start?package_id=org.coolstar.classicfolders2&sess=6184e57450f1b20ec097dfa2493c2c8ee22d87a04065e7e596f8557d78a4dd2f"
}
```

**Failure:**
```
{
   status: -1
}
```

### Payment Action

Initial Request (in Safari View Controller):

`GET https://my.chariz.io/transaction/stripe/start?package_id=org.coolstar.classicfolders2&sess=6184e57450f1b20ec097dfa2493c2c8ee22d87a04065e7e596f8557d78a4dd2f`

(See `Initiate Purchase -> Action Required` above for configuring the URL)

Callback:
`sileo://payment_completed`

### Downloads

When a user downloads a package from a repo with a payment provider in Sileo, instead of immediately downloading said package, we make a request to the payment provider.

`POST /package/:package_id/authorize_download`: 

**Request:**
`/package/org.coolstar.classicfolders2/authorize_download`
```json
{
    "token": "BEARER f2ca1bb6c7e907d06dafe4687e579fce76b37e4e93b7605022da52e6ccc26fd2",
    "udid": "4e1243bd22c66e76c2ba9eddc1f91394e57f9f83",
    "device": "iPhone7,2",
    "version": "1.0.1",
    "repo": "https://repo.example.com"
}
```

**Response:**
```json
{
   "url": "https://payment-vendor.example.com/download-secure/org.coolstar.classicfolders2?v=1.0.1&download-key=ndwqiufegregt4wghriur2hefgurwtqh35tegiruhgwhjteqrwuhj3utjgrwuiferejhtiut53t4j5ur3tgrwtu5teshriuw5q3utwgqrutwu"
}
```

The response of this request provides the URL that Sileo should download the package from. This request allows you to create a secure link for the download to be completed without allowing the repository to access the token.

**This URL must be HTTPS. HTTP URLs will fail.**

#### Security Considerations

Even with this method of authenticating downloads, payment vendors need to make sure they implement this properly to avoid exposing user tokens or unauthorized downloads.

- Never provide important information such as the user's token in your download URL.
- This link should only work one time, and then expire immediately.
- This link should have a reasonable automatic expiry time, such as a minute or two. It is only called right before the package is to be downloaded, so the download URL will be called within seconds to when it is provided by the vendor (unless some obscure error occurs).
- This link should not provide any important user information. At most, it should contain data the equivilant of a key allowing a limited download for one user. You can leave how this is done up to your server implementation.

---

### Errors:

Every error must consist of a few things:

- `error`: A string to show the user why the request has failed. If this is present, Sileo will automatically classify a response as a failure (or if `success` = false).
- `recovery_url`: An optional URL, so that if the request failed because of something the user must take care of, or you want to show more info about, providing this key will create a "More Info" button in the alert that navigates to your URL when tapped.
- `invalidate`: An optional boolean that decides whether or not Sileo should delete the user's current payment credentials. For example, if Sileo calls `user_info` with invalid credentials, if the error returned is `invalidate`, it will forget these credentials for future requests. Some parts of Sileo, like the payment button, will detect this boolean and prompt a sign in dialog again if necessary.
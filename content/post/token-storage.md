---
title: Analysis on secure session token storage 
description: 
date: 2020-02-05
categories:
  - "Write-up"
tags:
  - "Cookies"
  - "Storage"
---

Where should you store my session tokens to keep them secure ?<!--more-->

# Prelude 

While browsing the Web to search for an answer to the above, I quickly realised that most answers will advocate against using Web Storage. They practically all assert that it is highly insecure and should not be used for session token storage.

> *The core argument used against Web Storage says because Web Storage doesn’t support cookie-specific features like the Secure flag and the HttpOnly flag, it’s easier for attackers to steal it.*

Let’s see which storage medium is available to us and list Pros & Cons to see what makes most sense (from a security perspective).

## Cookies

![](https://cdn-images-1.medium.com/max/2000/0*evTpJnPasn_APxIx)

## The HttpOnly flag

HttpOnly is a an option which specifies that the cookie (i.e: session token etc…) should not be accessed from the application DOM.

`Set-Cookie: SESSIONID=[token]; HttpOnly`

* Introduced in 2002 to prevent XSS from stealing session tokens making it impossible to access from the DOM.

* Not an XSS mitigation mechanism and does not prevent XSS in any way.

* Can be bypassed with TRACE method, see [Cross Site Tracing (XST)](http://deadliestwebattacks.com/2010/05/18/cross-site-tracing-xst-the-misunderstood-vulnerability/) attack. (not exploitable in modern servers)

While it will indeed prevent the cookiejar’s content from being accessed from the DOM, it does not prevent an attacker from performing an attack similar to CSRF **if an XSS is found**.

In fact:

>  *The only and most effective way to attack when having a XSS hole is to launch an attack right on place when the payload is evaluated*

If an attacker would truly intent to leverage an XSS on your application, he could aim for much more than steal your session token.

## The secure flag

Cookies need the secure flag because they do not properly adhere to the Same Origin Policy (SOP). A browser will never send a cookie with a secure flag set over a unencrypted HTTP request

For example:

    Cookies set on https://example.com will be transmitted to and accessible via http://example.com by default.

## The Same-Site attribute

SameSite is a new attribute proposed in 2016 (see [RFC](https://tools.ietf.org/html/draft-west-first-party-cookies-07)) which allows servers to assert that a cookie ought not to be sent along with cross-site requests.

Set-Cookie: key=value; HttpOnly; secure; SameSite=strict|lax|none

As per the RFC:

* If the SameSite attribute's value is Strict, the cookie will only be sent along with "same-site" requests.

* If the value is Lax, the cookie will be sent with same-site requests, and with "cross-site" top-level navigations.

* If the value is None, the cookie will be sent with same-site and cross-site requests.

In other words:

* Strict will prevent any Cross Origin attacks (e.g.: CSRF, XS-Search) entirely.

* Lax will prevent state changing Cross Origin attacks but does not offer a robust defense against CSRF as a general category of attack.

Chrome is pushing for Same-Site: Lax to become default as part of Chrome 80. Moreover, people who decide to opt-out of the functionnality and use SameSite=none will **have** to setup the secure flag.

## Web Storage (localStorage and sessionStorage)

![](https://cdn-images-1.medium.com/max/2000/0*mXTkKhVof8nf7gPX)

Introduced as a HTML5 feature in 2016, Web Storage is an alternative to cookies for storing name-value pairs on the client side. It offers multiple advantages from a network architecture point of view, for example (non exhaustive):

* Cookies can be read both server-side and client-side, web storage only on client-side.

* Do not have to be sent with each request like Cookies

* Subsequently, offers bigger storage space

* More flexible as it is not limited to strings and can contain object-like data such as json.

## Web Storage security

While it is acceptable to say that Web Storage was not designed with security in mind, it is important to see that it does not suffer from the same aged (and flawed) design of cookies in their default state.

* Follows Same Origin Policy

* Browsers do not automatically attach the contents of Web Storage to HTTP requests

* Immune to Cross Origin attacks (CSRF, XS-Search, ...) by design (see above)

Other noteworthy attributes:

* Can be accessed freely by Javascript on the client

* localStorage only expires when deleted via Javascript

* sessionStorage is cleared when closing a Tab or via Javascript

## Storage Wars, which will you choose ?

In this section we will go over common statements seen on the Web and offer a perspective relative to all points discussed above

## “Cookies have a secure attribute !"

It is true that cookies can be protected against MITM attacks using the secure flag. Now, few things are worth mentioning.

First, one could argue that the secure flag is not default to cookies; therefore it is not any different than Web Storage. But what if you configured you cookies properly ?

Then, I would most likely ask why are you using unencrypted communications in the first place ? You could just as well sniff the credentials of the whole network and not have to care about session tokens at all. In summary, if you are using non HTTPS traffic in your application — I think you have other problems to solve first.

## “Web Storage offers no defense against XSS !”

The only “safety measure” offered by cookies against XSS is the HttpOnly flag. In reality, HttpOnly is not preventing XSS from happening in any way, and should not be considered as an XSS mitigation. You are still required to make sure that your site/application is XSS free. It does prevent the content of your cookies from being read by Javascript, which leads us to the next point.

## “You should never store sensitive information in any form of local storage !”

This point is equally applicable to both Web Storage **and cookies**. Sensitive informations should never be stored in any of two since this is not what they are meant for. Storing a user’s password or server-side secrets in a cookie is a very bad idea, regardless of HttpOnly.

## “Web storage doesn’t support automatic expiry !”

Expiry of session tokens should be done server-side or is already containted within the token itself (think JWT’s exp claim).

## Conclusion

The points made against the usage of Web Storage tend to focus on specific areas while forgetting the big picture.

Cookies are insecure by default, but can be hardened using a variety of flags and attributes. One very promising addition to cookie’s security would be SameSite by default. Nevertheless, it will take some time to be enabled across all major browser.

Web storage offers numerous architectural advantages (if your application benefits from those) and shouldn’t be discarded altogether.

## Links

- [https://www.gnucitizen.org/blog/why-httponly-wont-protect-you/](https://www.gnucitizen.org/blog/why-httponly-wont-protect-you/)
- [https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage](https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage)
- [https://portswigger.net/blog/web-storage-the-lesser-evil-for-session-tokens](https://portswigger.net/blog/web-storage-the-lesser-evil-for-session-tokens)
- [https://wpreset.com/localstorage-sessionstorage-cookies-detailed-comparison](https://wpreset.com/localstorage-sessionstorage-cookies-detailed-comparison/)
- [https://www.w3.org/TR/webstorage/](https://www.w3.org/TR/webstorage/)
- [https://scotthelme.co.uk/csrf-is-really-dead/](https://scotthelme.co.uk/csrf-is-really-dead/)
- [https://www.chromestatus.com/feature/5088147346030592](https://www.chromestatus.com/feature/5088147346030592))
- [https://www.chromestatus.com/feature/5633521622188032](https://www.chromestatus.com/feature/5633521622188032)

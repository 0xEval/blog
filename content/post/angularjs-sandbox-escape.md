---
title: Learning Weekly 01 - AngularJS sandbox escapes
description: 
date: 2020-02-05
categories:
  - "Learning"
tags:
  - "AngularJS"
  - "Template Injection"
  - "XSS"
  - "Learning Weekly"
---

This post will be the starting point on a new series *Learning Weekly* where I will spend a week learning about a new topic and summarize the outcome in a single page format. 

For this first episode, we will talk about **AngularJS sandbox escapes**.
<!--more-->

# Prelude

[AngularJS](https://angularjs.org/) is a popular MVC (*Model View Controller*) client-side framework for building dynamic web apps. It lets you use HTML templates which can contain [expressions](https://docs.angularjs.org/guide/expression) - JavaScript-like code snippets that will be executed by Angular.

In the following snippet, we can see that Angular will parse the content of the expression in-between curly braces `{{1+1}}` and output the result `2`.

{{< jsfiddle "2zs2yv7o" "html,result" "light" >}}

Similarly to [server-side template injections](https://portswigger.net/research/server-side-template-injection), anyone able to inject data inside an Angular expression will be able to execute arbitrary JavaScript. However, this only holds true under certain circumstances.

# Angular template injection #1

From version `1.0` to `1.1.5`, Angular did not have any kind of sandbox (we'll talk about this in the next section). 

> **Scope** is an object that refers to the application model. It is an execution context for **expressions**. Scopes are arranged in hierarchical structure which mimic the DOM structure of the application. Scopes can watch expressions and propagate events.

Angular expressions are scoped to an object - the `Scope` object - defined by the developer. For an attacker, this means that Angular will prevent you from calling functions such as `alert` on the `Window` object as the call would instead be made on the `Scope` object where it would not be defined.

For example, let's see what happens to the following expression `{{ alert(1) }}` in Angular `v1.0.8`. *[Step-by-step video explanation from (@LiveOverflow)](https://youtu.be/DkL3jaI1cj0?t=40)*

```js
// s == scope, k == locals
(function anonymous(s, k) {
    var l, fn, p;
    if (s === null || s === undefined)
        return s;
    l = s;
    // Checks if Scope object has property defined in expression
    // Note that alert is showing up twice (for later-on)
    s = ((k && k.hasOwnProperty("alert")) ? k : s)["alert"];
    if (s && s.then) {
        if (!("$$v"in s)) {
            p = s;
            p.$$v = undefined;
            p.then(function(v) {
                p.$$v = v;
            });
        }
        s = s.$$v
    }
    return s;
})
```

Later-on, Angular will try to execute the expression-derivated function on the `Scope` object. However, since it is not defined by the developer - the call will fail. As an attacker, we are out of luck. It seems we cannot call any interesting functions, right ?

It turns out, there is a way around this restriction: **the constructor property**.

> The **constructor property** returns a reference to the Object constructor function that created the instance object. Note that the value of this property is a reference to the **function itself**, not a string containing the function's name.

In other words, it is possible to achieve arbitrary code execution in expressions by accessing the `Function` constructor and creating a function containing the payload we are trying to execute.

We can easily verify this in our browser's console (`Command+Option+J` keybind for Chrome on MacOS):

```js
> constructor.prototype
Window {TEMPORARY: 0, PERSISTENT: 1, Symbol(Symbol.toStringTag): "Window", constructor: ƒ}
```
 
```js
> constructor.constructor
ƒ Function() { [native code] }
```

The following payload was found by Mario Heiderich and will effectively bypass the scope object restrictions and trigger an alert.

```js
{{ constructor.constructor('alert(1)')() }}
```

You can try it yourself, clicking on the `Result` tab below will pop an alert box originating from the JSFiddle domain.

{{< jsfiddle "6gjqhtz5" "html,result" "light" >}}

You might notice the alert box is popping up twice, why is that ? It is because of how Angular is rewritting our expression into a `Function`.

# Introducing the Angular sandbox

The sandbox restricts AngularJS parser from evaluating unsafe expressions. These can attempt to access the Function constructor, window object, DOM element, global variables, or the Object constructor. This is not an exhaustive list as each version of Angular contained different checks, but that is the general idea.

The following example is taken from Angular `v1.4.6` parser logic (ref: [https://github.com/angular/angular.js/blob/v1.4.6/src/ng/parse.js](https://github.com/angular/angular.js/blob/v1.4.6/src/ng/parse.js)):

{{< gist 0xEval 607d04bb5c0446f9b7199adf8328fe7b >}}

If an expression contains a to call the `__proto__` accessor property of an `Object`, an exception will be thrown and the expression will not be evaluated.

{{< figure src="/media/2020-02-06-15-42-56.png" title="Angular sandbox error demonstration" >}}

However, as mentioned in the following quote from Angular development team:

> AngularJS's expressions are sandboxed **not for security reasons,** but instead to maintain a proper separation of application responsibilities. For example, access to window is disallowed because it makes it easy to introduce brittle global state into your application. However, this sandbox is not intended to stop attackers who can edit the template before it's processed by Angular. It **may be possible to run arbitrary JavaScript** inside double-curly bindings if an attacker can modify them.

After the sandbox was introduced, many security researchers worked on finding ways to bypass the parser safety check functions. We will see a few examples to try and understand how the concept works.

# Breaking out of the sandbox


Portswigger researcher Gareth Heyes gave a great talk explaining multiple of these bypasses. It goes with the blog-post linked in the references. *(Warning: it goes fast, and its hard to understand !)*:

{{< youtube jlSI5aVTEIg >}}

# Remediations


# References

- https://portswigger.net/research/xss-without-html-client-side-template-injection-with-angularjs
- https://portswigger.net/research/dom-based-angularjs-sandbox-escapes
- https://sites.google.com/site/bughunteruniversity/nonvuln/angularjs-expression-sandbox-bypass
- https://www.synopsys.com/blogs/software-security/angularjs-1-6-0-sandbox/
- https://docs.angularjs.org/guide/security
- https://ryhanson.com/angular-expression-injection-walkthrough/
- https://ryhanson.com/stealing-session-tokens-on-plunker-with-an-angular-expression-injection/
# CSP useful, a collection of scripts, thoughts about CSP

I'm testing and using CSP (Content Security Policy), and here are some thoughts, resources, scripts and ideas on it.


## What CSP is really good for

### In development

I use __CSP to clean up some bad old contents__ (with inline-styles for example).

1. Just activate CSP on a site with a report-uri
2. Ask your boss/collegues/grandma to browse the website
3. All notifications will come without doing anything (yes, I’m lazy)
4. Yay, you know where you have to make some cleanup

Moreother, if you don't have the time to clean it, setting up CSP policy will avoid bad old styles from breaking the nice/clean new design. Or it will tell you when contributors are doing shit on the website.

### Progressive enhancement and orthogonality

As far as I can see, using CSP on my jQuery plugins helped me a lot to design them without inline styles/js. See for example: http://a11y.nicolas-hoffmann.net/

So it is a great help for progressive enhancement, orthogonality and clean front-end.


## How to see easily CSP directives on a website

For Firefox: make Maj+F2 and type "security csp". It will show you directives and advices.

If you have webdevelopper toolbar, go into infos - HTTP headers.



## About plugins

JS/jQuery plugins should provide the CSP requirements they need to work (especially inline-styles or inline-js), so:

- we will know what they need instead of having to discover it
- we would be able to choose a plugin according to its capabilities to respect orthogonality (see http://openweb.eu.org/articles/orthogonality-with-css)



## Scripts

### Report-URI folder

In [folder "report-uri"](https://github.com/nico3333fr/CSP-useful/tree/master/report-uri), you may find examples of CSP parsers you can use for report-uri.

- csp-parser-basic.php 	: the most basic one, it sends an e-mail.
- csp-parser-enhanced.php :	avoids some bugs (listed below)
- csp-parser-with-database.php : put notifications in a database, then you can do whatever you want with all these informations! :)




### CSP directives for third-party services

In [folder "CSP for third party services"](https://github.com/nico3333fr/CSP-useful/tree/master/csp-for-third-party-services), you may find examples of directives you need to use for some services.




### CSP Check folder

In [folder "csp-check"](https://github.com/nico3333fr/CSP-useful/tree/master/csp-check), you may find the source of a proof of concept: this script was a quick and dirty way to reproduce a bug in Firefox, you can see it in action here: http://csp.nicolas-hoffmann.net/

Basically, the page generates an unique id, notifications sent to report-uri are put in database, the page makes an AJAX call to database, and the unique id helps to find CSP errors in database.

This is useful to prove bugs, not only for Firefox. ^^

To reproduce the bug:

1. Open http://csp.nicolas-hoffmann.net/
2. The page is going to generate a unique id, ex http://csp.nicolas-hoffmann.net/?id=foo
3. Wait some seconds. The page doesn't find any notification in the database.
4. Now inspect the page with Firefox inspector, please highlight some elements.
5. Close the inspector
6. Refresh the page with the id you have : http://csp.nicolas-hoffmann.net/?id=foo
7. It is going to find a lot of CSP errors.

At the beginning, I've made it to prove that some Chrome extensions are sending notifications to report-uri (while they should not), and it helped to find/prove a bug in Firefox Inspector.

Here is the reported bug: https://bugzilla.mozilla.org/show_bug.cgi?id=1195302

It <del>should be</del> is fixed with Firefox 42 https://bugzilla.mozilla.org/show_bug.cgi?id=1185351 :)


### CSP WTF???

In [folder "CSP WTF"](https://github.com/nico3333fr/CSP-useful/blob/master/csp-wtf/README.md), you may find examples of strange notifications you may receive. Feel free to add/explain some.




## Small tips and tricks

### Multiple domains

Be careful if you have multiple domain names (foo.com, foo.net) pointing to a single website while using ```'self' ``` as value. Example: if a user is using a full url for an image, let's say ```http://foo.com/image.jpg```, using ```'self' ``` won't be enough if the user is on foo.net. Be sure to allow all necessary domains.

### Generate a hash

If you really have to use some inline scripts/css, for example:

```
<script>alert('Hello, world.');</script>
```

You might add <code>'sha256-qznLcsROx4GACP2dm0UCKCzCG-HiZ1guq6ZZDob_Tng='</code> as valid source in your <code>script-src</code> directives. The hash generated is the result of:

<pre><code>
base64_encode(hash('sha256', "alert('Hello, world.');", true))
</code></pre> 

in PHP for example.



## Bugs I've found

- <del> Firefox: https://bugzilla.mozilla.org/show_bug.cgi?id=1195302 (inspector, extensions?) </del> (fixed in Firefox 42)
- Firefox: https://bugzilla.mozilla.org/show_bug.cgi?id=1215108 (bookmarklets)
- <del>Chrome/Blink : https://code.google.com/p/chromium/issues/detail?id=524356 (extensions)</del> (fixed in last versions of Chrome)
- Chrome: https://bugs.chromium.org/p/chromium/issues/detail?id=595004 (bookmarklets too, see too https://bugs.chromium.org/p/chromium/issues/detail?id=233903)
- Safari/Webkit : https://bugs.webkit.org/show_bug.cgi?id=149000 (extensions)
- <del>Edge (no URL tracker yet, same workaround than for Firefox)</del> (seems to be fixed on Edge)

These bugs are just annoying, they are not critical. They provide false-positives notifications on report-uri.


## Resources

### Resources

- http://content-security-policy.com/
- https://www.smashingmagazine.com/2016/09/content-security-policy-your-future-best-friend/
- http://cspisawesome.com/
- http://www.cspplayground.com/home
- https://csp-evaluator.withgoogle.com/
- https://scotthelme.co.uk/csp-cheat-sheet/
- http://www.w3.org/TR/CSP/
- http://www.html5rocks.com/en/tutorials/security/content-security-policy/
- http://websec.io/2012/10/02/Intro-to-Content-Security-Policy.html
- http://content-security-policy.com/presentations/
- https://developer.mozilla.org/en-US/docs/Web/Security/CSP
- https://developer.chrome.com/extensions/contentSecurityPolicy
- http://www.sitepoint.com/improving-web-security-with-the-content-security-policy/
- https://www.nicolas-hoffmann.net/content-security-policy-parisweb-2015 (slides of my talk in Paris Web 2015, in french)
- https://chloe.re/tag/web/
- [dotSecurity 2016 - Scott Helme - Content Security Policy: The application security Swiss Army Knife (video) ](https://www.youtube.com/watch?v=d0D3d0ZM-rI)
- [InfoShare 2016 - Scott Helme - Content Security Policy: The application security Swiss Army Knife (video)](https://www.youtube.com/watch?v=d6Clpj_KfFo)
- [Paris Web 2015 - Nicolas Hoffmann - Content Security Policy (video in french-speaking)](http://www.paris-web.fr/2015/conferences/csp-content-security-policy.php)


### About collecting and filtering reports

- https://report-uri.io/
- https://blogs.dropbox.com/tech/2015/09/on-csp-reporting-and-filtering/
- https://oreoshake.github.io/csp/twitter/2014/07/25/twitters-csp-report-collector-design.html


### Why you should use CSP

- https://www.aaron-gustafson.com/notebook/more-proof-we-dont-control-our-web-pages/


### Interesting posts on how to deploy CSP

- https://blog.twitter.com/2011/improving-browser-security-csp
- https://github.com/blog/1477-content-security-policy
- https://blogs.dropbox.com/tech/2015/09/unsafe-inline-and-nonce-deployment/ 
- https://blogs.dropbox.com/tech/2015/09/csp-the-unexpected-eval/ 
- https://blogs.dropbox.com/tech/2015/09/csp-third-party-integrations-and-privilege-separation/
- http://githubengineering.com/githubs-csp-journey/
- https://www.wired.com/2016/05/wired-first-big-https-rollout-snag/
- https://corner.squareup.com/2016/05/content-security-policy-single-page-app.html

### Other

- https://github.com/nico3333fr/CSP-useful (yes you are here)

### Future of CSP

- https://www.w3.org/TR/CSP3/
- https://speakerdeck.com/mikispag/making-csp-great-again-michele-spagnuolo-and-lukas-weichselbaum

### Online tools that test CSP

- https://cspvalidator.org/
- https://www.dareboost.com/
- https://securityheaders.io/
- https://report-uri.io/home/tools

### Add-ons Navigator
- https://addons.mozilla.org/en-us/firefox/addon/newusercspdesign/

Enjoy!

[Nicolas Hoffmann](http://www.nicolas-hoffmann.net/) - [@Nico3333fr](https://twitter.com/Nico3333fr)

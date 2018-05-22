---
layout: page
title: Privacy
permalink: /privacy/
comments: true
---

This site uses [Google Analytics](https://www.google.com/analytics/) and [Disqus](https://disqus.com/).

- For Google Analytics (also called GA), anonymous, statistical data on page view is collected to let me know how many people are looking at my blog. I've turned off advertising and marketing features, and has done the following to ensure that this is truly anonymous:
    - **IP** has been anonymized by stripping out the last octet at the client side.
    - **Cookie** has been disabled using the `storage: 'none'` settings.
    - **Client ID** has also been disabled - I generate a random, 10 digit number for each page load (to fulfill GA's technical requirement) that is not saved or sent anywhere else.
- Disqus is used for comments and discussions. See [here](https://blog.disqus.com/update-on-privacy-and-gdpr-compliance) for their updates to [privacy policy](https://help.disqus.com/terms-and-policies/disqus-privacy-policy). Since this is a third party system, we need your **explicit consent** to enable it on this site.
    - Once you've given your consent (or refusal), we store your preference using *[Local Storage](https://en.wikipedia.org/wiki/Web_storage)* [^1].
    - You may change your mind later by clicking on the **Clear Privacy Settings** button at the bottom left part of this website. Doing so will reset your preference so that you may choose again eitherway.
    - Alternatively, you may also delete Local Storage directly in your browser.
    - If your browser does not support Local Storage, then the choice will still work, but your preference will not be remembered and effectively, you will have to consent (or deny) for *each* page visit.

Aside from leaving a comment using Disqus, you may also send me an email for any further questions. Thanks!

----

[^1]: Which I consider to be better than Cookie in terms of privacy if there is no actual need to send this information to server - which is obviously the case here since this is a static site.

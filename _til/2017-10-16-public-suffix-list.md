---
title:  "Public Suffix List"
date:   2017-10-16 09:11:00
category: til
tags: ["dns", "domain", "url", "ssl", "cookies", "heroku", "ssl", "https", "secure", "cookie"]
---

TIL about the public suffix list, which is a list of domains in which browsers will not allow secure cookies to be set

I came across the [`public suffix list`][psl]{:target="_blank"} when I was attempting to set a secure cookie for CashBackHero on Heroku. No matter what I tried, I could not set a secure cookie in the domain `cashbackhero.herokuapp.com`.

I came across [this article][article]{:target="_blank"} from heroku's documentation, which explained that `herokuapp.com` was part of the `public suffix list`. For security purposes **it is impossible to set a secure cookie in a domain listed in the public suffix list**

[Click here][list]{:target="_blank"} to check out the list itself. Search for `herokuapp.com` on that page to confirm that it is part of the `public suffix list`.

## What is a cookie?

`Cookies` are used to track small and sometimes sensitive pieces of information (like authenticated status) about repeat visitors to domains. They are stored in your browser, and often have an expiry date associated with them. When you visit a site, and are forced to re-authenticate, is generally because your cookie has expired.

The setting and accessing of cookies set in a particular domain (like `facebook.com`) **is restricted to sites served from that domain**. In other words: facebook can only set/access cookies from sites served in the `*.facebook.com` domain. Google cannot set/access cookies in facebook's domain, and vice versa.

## The TLD problem

In early browser implementations, browsers prevented cookies from being set at the Top level domain (TLD) i.e. the `.com`, `.org`, `.edu` level. This prevented developers from setting/accessing cookies set for every website that is hosted on a `.com` TLD.

To make this clear, it would be a security concern if the cookies your bank set for managing your session online were accessible by any other site you visited in the `.com` domain space!

Historically, browsers allowed cookies to be set at a secondary domain level and higher. For example on sites like `facebook.com`, `google.com`, or `bambielli.com`. This rule proved problematic, though, in situations where only third level domains are allowed for registration. For example, domains in the UK are registered under `co.uk`, which already has a top level and second level domain as part of the domain suffix.

## Public Suffix List

As of writing, **there is no algorithmic way to detect under which domain level cookies should be permitted by simply examining the URL**. A list is the best option we have come up with: the `public suffix list`.

This list dictates domains under which cookies are not permitted to be set, because **they are considered public domains that are not owned by one organization or person.** It would be a security concern if browsers honored secure cookies on apps hosted on any of the domains in this list, since malicious developers could publish an app to the domain that accesses cookies set by legitimate apps.

For this reason, **it is impossible to set secure cookies on heroku without providing a custom domain and paying for SSL for that domain.**

[psl]: https://publicsuffix.org/learn/
[article]: https://devcenter.heroku.com/articles/cookies-and-herokuapp-com
[list]: https://publicsuffix.org/list/public_suffix_list.dat

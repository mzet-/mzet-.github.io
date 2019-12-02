---
layout: post
title: "Burp Suite Pro real-life tips & tricks: Looking for hidden attack surface and leaked secrets"
tags: [toolbox, web-security]
---

Burp has some nice export features which (combined with some 3rd party tools) can greatly aid in the process of looking for additional/hidden endpoints and/or hardcoded secrets.

### \#1: Look for secrets (credentials, keys) in JS sources

![]({{ site.url }}/assets/saveScripts.png)

Now it is possible to grep thru the sources with simple `grep`. Here is the open ended list of phrases that could be looked for:

```
bearer
secret
APP_SECRET
consumerkey
JIRA_Password
authorization
authentication
auth_key
consumer_secret
security-signature
signature
X-API
X-Paypal
secret_key
JWK
JWT
SSO_LOGIN
access_key
accountKey
AWS_Secret
aws_secret_access_key
api_key
...
```

### \#2: Look for hidden endpoints in JS sources with LinkFinder

Getting URLs from Burp:

![]({{ site.url }}/assets/saveSelectedItems.png)

Discovery of endpoints with LinkFinder:

```
$ linkfinder -i $HOME/PEN_TESTING/<TARGET>/savedItems.out -b -o cli | sort -u
```

### \#3: Look for hidden endpoints in JS sources with JSParser

Getting URLs from Burp:

![]({{ site.url }}/assets/saveURLs.png)

Discovery of endpoints with JSparser:

```
$ jsparser
firefox -> http://localhost:8008
(paste links form Burp)
```

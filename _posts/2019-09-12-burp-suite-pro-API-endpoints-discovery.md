---
layout: post
title: "Burp Suite Pro real-life tips & tricks: Looking for hidden endpoints"
tags: [toolbox, web-security-testing, adversary-simulation]
---

### \#1: Look for hidden/old endpoints in JavaScript sources

#### LinkFinder

Getting URLs from Burp:

![]({{ site.url }}/assets/saveSelectedItems.png)

Discovery of endpoints with LinkFinder:

```
$ linkfinder -i $HOME/PEN_TESTING/<TARGET>/savedItems.out -b -o cli | sort -u
```

#### JSparser

Getting URLs from Burp:

![]({{ site.url }}/assets/saveURLs.png)

Discovery of endpoints with JSparser:

```
$ jsparser
firefox -> http://localhost:8008
(paste links form Burp)
```

#### Additional URLs

TODO: https://github.com/tomnomnom/waybackurls

### \#2: Brute-force (server side) endpoints discovery 

TODO: Burp Content Discovery feature

TODO: https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/api

TODO: skipfish

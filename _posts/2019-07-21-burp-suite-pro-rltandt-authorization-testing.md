---
layout: post
title: "Burp Suite Pro real-life tips & tricks: Authorization testing"
tags: [toolbox, web-security-testing]
---

In the past, I found already quite old (presented in 2013) slide deck [Burp Suite Pro real-life tips & tricks](https://www.agarri.fr/docs/HiP2k13-Burp_Pro_Tips_and_Tricks.pdf) authored by [Nicolas Grégoire](https://www.agarri.fr/en/index.html) very useful which gave me inspirational motivation for more advanced (and therefore effective) usage of Burp Suite Pro. Since that time Burp has significantly evolved (version 2.x was released) and lots of interesting 3rd party extensions were developed.

So I've decided to "borrow" the title from Nicolas and publish (as a reference for me and others) some tips and tricks which I find useful during my web application penetration testing engagements. Will begin with testing for authorization/IDOR issues.

### Testing with AutoRepeater

There are bunch of Burp extensions for assisting in authorization testing but I find [AutoRepeater](https://github.com/nccgroup/AutoRepeater) by NCCGroup most convenient.

Let's consider the application that  

![AutorRepeater defining replacement]({{ site.url }}/assets/autoRepeater1.png)

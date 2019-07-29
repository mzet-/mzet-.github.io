---
layout: post
title: "Burp Suite Pro real-life tips & tricks: Authorization testing"
tags: [toolbox, web-security-testing]
---

In the past, I found (already quite old - presented in 2013) slide deck [Burp Suite Pro real-life tips & tricks](https://www.agarri.fr/docs/HiP2k13-Burp_Pro_Tips_and_Tricks.pdf) authored by [Nicolas Grégoire](https://www.agarri.fr/en/index.html) very useful which gave me motivation to constantly look for effciencies in my day to day Burp Suite Pro usage. Since that time Burp has significantly evolved (major milestone - version 2.x was released) and lots of interesting 3rd party extensions were developed.

So I've decided to "borrow" the title from Nicolas and publish (as a reference for me and others) a few tips and tricks which I find useful during my web application penetration testing engagements. Will begin with testing for authorization/IDOR issues.

### Testing with AutoRepeater

There are bunch of Burp extensions for assisting in authorization testing but I find [AutoRepeater](https://github.com/nccgroup/AutoRepeater) by NCCGroup most convenient.

Let's consider typical web application that uses `Authorization: Bearer` token for request authentication purposes and it defines following types of users (i.e. permission levels):

1. administrator user
2. regular user
3. guest (unauthenticated) user

each user has other permission set and therefore can access other functionality and resources in the application. Site returns HTTP `401` in case of unauthorized access attempt. 

To optimally configure AutoRepeater

![AutorRepeater defining replacement]({{ site.url }}/assets/autoRepeater1.png)


At **#1** we prepare 3 tabs for every type of user we have: regular user, 2nd administrator user (the first administrator user is the one we will browse our target application) and guest user. At **#2** we're instructing the extension to replace value of `Authorization` header - we need to do this 3 times (for each user in different tab) providing the respective token value (for guest user we're setting empty value).Finally, at **#3** we're skipping "boring" requests to filter out noise.

Now when we're browsing site as an administrator user (let's call him "Admin A user") AutoRepeater replays each and every request we have sent 3 times - for: regular user, admin B user and for the guest user - every time replacing value of `Authorization` header. This gives us nice, streamlined way to look for:

1. vertical privileges escalation/authorization bypass (AutoRepeater attempts to access resources/functionality available for "Admin A user" using token of regular user and without any authorization token (guest user)
2. limited horizontal authorization bypass (attempt to access "Admin A user" specific resources with admin's B token)

in one pass thru the application. What would be left is looking for horizontal authorization bypass attempts from standpoint of given regular user to resource's of other regular user.

If you use (and want to share) any other cool tricks for testing for IDORs, let me ([@_mzet_)](https://twitter.com/_mzet_)) know.

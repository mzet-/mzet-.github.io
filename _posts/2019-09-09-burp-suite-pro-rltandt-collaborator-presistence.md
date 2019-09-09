---
layout: post
title: "Burp Suite Pro real-life tips & tricks: Persistent Access to Collaborator"
tags: [toolbox, web-security-testing]
---

It was already discussed [here](https://0x00sec.org/t/achieving-persistent-access-to-burp-collaborator-sessions/14311). Putting here for quick reference.

1. In Burp go `Project options -> Misc` and check `Poll over unencrypted HTTP`
2. Open Collaborator: `Burp menu -> Burp Collaborator client`
3. Run tshark:

```
$ sudo tshark -Y http -T fields -e http.request.method -e http.request.uri -e http.host -e http.request.uri
```

4. 'Poll' interactions in the Collaborator client and observe following request in tshark:

```
GET		polling.burpcollaborator.net	/burpresults?biid=KEY
```

5. Acquire one or more (depending on your needs) Collaborator's hostnames (number to generate & 'copy to clipboard')

6. Now you can retreive (also after closing the Collaborator client) interactions with your Collaboarator's hostnames by requesting:

```
$ curl http://polling.burpcollaborator.net/burpresults?biid=KEY
```

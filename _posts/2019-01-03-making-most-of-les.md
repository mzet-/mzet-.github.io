---
layout: post
title: Making most of linux-exploit-suggester
tags: [exploitation, priv_esc]
---

[linux-exploit-suggester.sh](https://github.com/mzet-/linux-exploit-suggester) (commonly called just `les.sh`) implemented and maintained by me is the next generation version of the tool designed to assist in privilege escalation testing on Linux servers during the penetration testing.

Too often during the penetration testing drills/maneuvers and real-life engagements I observe peer operators just running `./les.sh` (without any parameters) then doing cursory analysis of results and moving forward to other testing techniques if none of the exploit stands out of from the crowd. Yet, the tool has much more to offer, then that. But first, let's do a little deep dive into how the `les.sh` works.

## Under the hood

Automating the process of selecting candidate exploits for the given Linux box is not an easy task. The major obstacles that makes it challenging andevor are:

- practically every Linux distro I'm aware of has 
- security fix backports
- functionality backports

That being said, to its job and handle many different Linux distributions `les.sh` has employed number of heuristic method(s) to achieve its main objectives: generate the list of candidate exploits for a given Linux box in the same time minimazing false postives and false negatives. Additionally - for the sake of practicallity and smooth maintenance - following assumptions has been made while implementing (and maintaining) the tool:

- 'tagging' subsystem (more on it later) focuses on server-based distros Deb, Ubuntu, RHEL/CentOS
- userspace analysis subsytem focuses on server-based distros ...

Orginally to achieve this, `linux-exploit-suggester.sh` was processing solely the vanilla kernel version, this approach was inherited from `les.sh` predecessor [Linux_Exploit_Suggester](https://github.com/InteliSecureLabs/Linux_Exploit_Suggester) script. However, this method wasn't very effective as it was susceptible to false positives problems, yielding the long list of exploits for manual analysis.

So currently ...:

1. Generating list based on kernel version

2. Checking for tags

3. Discarding exploits that are not applicable

To further condense the canditate exploits list, additional set of requirements have been introduced to `les.sh`: now when adding the exploit to the tool, one can define additional conditions that are required to be met, for example:

    Reqs: pkg=linux-kernel,ver>=3.2,ver<=4.10.6,CONFIG_USER_NS=y,sysctl:kernel.unprivileged_userns_clone==1 

Requirements set from above states that the kernel version needs to be > 3.2 and <= 4.10.6 but also kernel needs to have usernamespace fucntionality compiled in (`CONFIG_USER_NS=y`) and enabled (`sysctl:kernel.unprivileged_userns_clone==1`).

For less typical checking, also the ability to run arbitrary Bash command(s) was provided to see if the exploit is applicables for given system, for example:

    Reqs: pkg=linux-kernel,ver>=4.4.0,ver<=4.4.0,cmd:grep -qi ip_tables /proc/modules

In above scenario command `grep -qi ip_tables /proc/modules` is run to verify if `ip_tables` module is loaded (as this is required for the exploit to work).

4. Sorting list based on dynamically generated rank

## Functionality

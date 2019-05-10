---
layout: post
title: Making most of linux-exploit-suggester
tags: [exploitation, priv_esc]
---

[LES](https://github.com/mzet-/linux-exploit-suggester) security tool, developed and maintained by [Z-Labs]() is the next generation version of the tool designed to assist in privilege escalation testing on Linux servers during the penetration testing. In this post I will try to describe how it works and how to use it more effectively.

Too often during the penetration testing engagements or testing drills/maneuvers I observe peer operators just running `./les.sh` (without any parameters) then doing cursory analysis of results and moving forward to other testing techniques if none of the exploit stands out from the crowd. Yet, the tool has much more to offer, then that. But first, let's do a little deep dive into how the `les.sh` actually works. It will help us understand how to use it more effectively.

Automating the process of selecting candidate exploits for the given Linux box is not an easy task. The major obstacles that makes it challenging endeavour are:

- practically every Linux distro I'm aware of has double versioning
- security fix backports
- functionality backports

## Under the hood

That being said, to do its job and handle many different Linux distributions `les.sh` has employed number of heuristic method(s) to achieve its main objectives: generate the list of candidate exploits for a given Linux box in the same time minimazing false postives and false negatives. Additionally - for the sake of practicallity and smooth maintenance - following assumptions has been made while implementing (and maintaining) the tool:

- 'tagging' subsystem (more on it later) focuses on server-based distros Deb, Ubuntu, RHEL/CentOS
- userspace analysis subsytem focuses on server-based distros ...

Orginally to achieve this, `linux-exploit-suggester.sh` was processing solely the vanilla kernel version, this approach was inherited from `les.sh` predecessor [Linux_Exploit_Suggester](https://github.com/InteliSecureLabs/Linux_Exploit_Suggester) script. However, this method wasn't very effective as it was susceptible to false positives problems, yielding the long list of exploits for manual analysis.

So currently the approach, `les.sh` takes to generate list of candidate exploits looks like this:

1. Generating initial exploits list based on vanilla kernel version
2. Checking for 'Tags' hits for every exploit
3. Discarding exploits that are not applicable based on 'additional checks'
4. Calculating internal metric ('Rank') for each exploit and sorting final list 

See below for details of each step:

#### Generating list based on kernel version

#### Checking for tags

#### Discarding exploits that are not applicable

To further condense the canditate exploits list, additional set of requirements have been introduced to `les.sh`: now when adding the exploit to the tool, one can define additional conditions that are required to be met, for example:

```
Reqs: pkg=linux-kernel,ver>=3.2,ver<=4.10.6,CONFIG_USER_NS=y, \
sysctl:kernel.unprivileged_userns_clone==1 
```

Requirements set from above states that the kernel version needs to be > 3.2 and <= 4.10.6 but also kernel needs to have usernamespace fucntionality compiled in (`CONFIG_USER_NS=y`) and enabled (`sysctl:kernel.unprivileged_userns_clone==1`).

For less typical checking, also the ability to run arbitrary Bash command(s) was provided to see if the exploit is applicables for given system, for example:

    Reqs: pkg=linux-kernel,ver>=4.4.0,ver<=4.4.0,cmd:grep -qi ip_tables /proc/modules

In above scenario command `grep -qi ip_tables /proc/modules` is run to verify if `ip_tables` module is loaded (as this is required for the exploit to work).

#### Sorting list based on dynamically generated rank

## Functionality

## Contributing

LES is useful tool used by the security professionals around the globe during their red team/pentest engagements. To make it even better and more effective you can help in 4 ways:

- Add newly published Linux privilege escalation exploits to it.
- Test existing exploits on various Linux distributions with multiple kernel versions, then document your findings in a form of `Tags` in LES, e.g. of a tag: `ubuntu=12.04{kernel:3.(2|5).0-(23|29)-generic}` which states: `tagged exploit was verifed to work correctly on Ubuntu 12.04 with kernels: 3.2.0-23-generic, 3.2.0-29-generic, 3.5.0-23-generic and 3.5.0-29-generic;`. With this tag added LES will automatically highlight and bump dynamic `Rank` of the exploit when run on Ubuntu 12.04 with one of listed kernel versions. This will help you (and others) during pentests to rapidly identify critically vulnerable Linux machines.
- Published exploits are often written only for PoC purposes only for one (or couple of) specific Linux distributions and/or kernel version(s). Pick sources of the exploit of choice and customize it to run on different kernel version(s). Then add your customized version of exploit as `ext-url` entry to LES and modify `Tags` to reflect newly added targets.
- Conduct analysis of chosen kernel hardening security measure (add it to the `FEATURES` array) and publish your analysis at: `https://github.com/mzet-/les-res/blob/master/features/<feature-name>.md`
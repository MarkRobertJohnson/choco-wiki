# Chocolatey.org Packages Disclaimer

**Bottom line** - As an individual using Chocolatey, you are more likely okay if something breaks when setting up your personal machines - the Community Package Repository is typically fine for you. When it comes to using Chocolatey in an organizational context, you want reliability, control, and trust. You can gain trust over some packages on the community repository, and possibly some reliability if the binaries are included in the package. However there is a limiting factor with a public repository - distribution rights. This creates a large failure point that organizations just don't have when they are hosting their own internal packages.

<!-- TOC -->

- [Organizations](#organizations)
  - [Summary](#summary)
  - [Reliability](#reliability)
  - [Trust / Control](#trust--control)
  - [Distro-provided Repositories](#distro-provided-repositories)
- [Excessive Use](#excessive-use)
  - [How To Avoid Excessive Use](#how-to-avoid-excessive-use)
  - [What To Do When You Are Blocked For Excessive Use](#what-to-do-when-you-are-blocked-for-excessive-use)
- [Community Provided Packages Are Not Supported](#community-provided-packages-are-not-supported)
- [Put It Another Way](#put-it-another-way)

<!-- /TOC -->

## Organizations

### Summary

As an organization, you want 100% reliability (or at least that potential), and you may want full trust and control as well. This is something you can get with internally hosted packages, and you are unlikely to achieve from use of the Community Package Repository. If your use of Chocolatey is for an organization/business, you likely have a low tolerance for production breakages and/or low trust for the greater internet. You likely would not want to give control of your infrastructure over to community members and volunteers. Organizational use of the community repository is not recommended.

### Reliability

Chocolatey is the best option for software management, as long as you are using package repository sources you can rely on.

A huge thing in Windows ecosystem is copyright law and how it plays into distribution rights. Most software in POSIX-land (Linux) is open source friendly, which has friendly redistribitution rights (redist). So you can embed that software directly in the package and publicly offer it. In Windows that is not the case - an organization like Microsoft would get very upset if the Office 365 packages on the public Community Package Repository actually contained the Office 365 binaries. That creates an enormous failure point that is outside of the package's control when it needs to download files at install time (that dependency on internet-available files that are hoped to always be available, but in practice they are not).

You can build a 100% reliable pipeline/workflow within the Chocolatey framework, just not with the community package repository. Building a reliable pipeline is huge. If you are a Windows admin wanting to trust a framework like Chocolatey, you are not going to use the Community Package Repository. Not when your reputation/job is on the line for picking the best options.

**NOTE:** You can also achieve reliability when reusing community packages, as long as you [[internalize|How-To-Recompile-Packages]] them. **Internalizing is not the same thing as caching the nupkg files like Artifactory, Nexus, ProGet, etc can do.**

### Trust / Control
Windows admins typically need to do everything internally with no outside internet access. There is quite a bit more hush-hush, non-use of publicly available things without bringing it internal for absolute control and trust. There is a huge (semi-healthy, but maybe even unhealthy) lack of trust for anything reaching out to the internet. I'm not saying this is exclusive to Windows admins, but it is very much the norm. So using the community package repository is a non-starter for these kinds of folks.

There are options for organizations to be able to reuse packaging from the community repository but make the package 100% offline and reliable. Please look at those options on the pricing page if you are interested.

### Distro-provided Repositories

There is another psychology aspect to this - Debian/RPM are nearly the ONLY way you install software on those machines, so it is expected that you would go that route. There are organizations using POSIX environments (typically RedHat) where it is becoming more typical that they pull those repositories local as well so they can trust everything will always work and be reliable.

However Windows doesn't have a distro-provided repo. Chocolatey Software does not support use of the community repo for organizational use that doesn't also benefit the community (providing and maintaining packages). Reliability plays a huge part in that. If something breaks within the context of a package, then Chocolatey gets blamed (even though it is not Chocolatey's fault).

## Excessive Use

***Please note that individuals (even organizations) using the community repository are unlikely to hit excessive use numbers under normal usage scenarios.***

Another aspect to keep in mind is that the community package repository is meant for the community. Perceived abuses of the community package repository that affect it in a detrimental way for the rest of the community will not be allowed. By abusive, it ***may*** mean more than **100 installs per hour on average over an internally determined amount of time** (it could be more, could be less) - this is not queries, this is **installs, upgrades** where actual package downloads are occurring. Let's say that is 30 days - that would mean 72,000+ package downloads over 30 days. When that is seen, our community team will make attempts to warn folks if we have known contacts (keep in mind it's highly unlikely we will have your contact information), and implement a temporary block to ensure your usage does not affect the community in a detrimental way. Many times this is due to a misconfiguration and can be corrected quickly.

**Blocks are meant to be temporary bans, but require you to act to remedy the situation.** If you have been blocked, please see the next sections for corrective actions.

**NOTE**: If you or your organization feels you will need to go over this limit and need whitelisted, please reach out at https://chocolatey.org/contact, choose "Blocked IP Address". As we have limited information, please include your name, email address, phone number, and the IP addresses you believe are blocked so we can contact you and verify if there is a ban. Once you have resolved any issues on your side, we can lift the ban.

### How To Avoid Excessive Use
To avoid excessive use, please see our [[organizational deployment guide|How-To-Setup-Offline-Installation]]. There are even ways to automate caching / [[internalizing|How-To-Recompile-Packages]] (caching and internalizing are entirely different concepts) packages so you still get a pretty good hands off experience.

If you are not able to take advantage of [[internalizing|How-To-Recompile-Packages]] packages, you can still cache them locally (using package repository solutions like Artifactory, Nexus, ProGet, MyGet, etc), which will reduce your direct usage of the community repository. **NOTE:** Caching doesn't make the packages you are using from the community repository any more reliable, they may still need to download things from the internet at runtime - but it doesn't put you in a worse place than you already are at because you are already using the community repository directly which has issues identified in this document. If you want to achieve reliability when reusing community packages, you would need to [[internalize packages|How-To-Recompile-Packages]].

### What To Do When You Are Blocked For Excessive Use
**NOTE: A block will not automatically expire, you will need to contact our team to resolve the block.**

If you have found that you have gone over the limit and have been warned/blocked, please reach out at https://chocolatey.org/contact (send message to "Blocked IP Address" in the drop down - you may need to do this from a different IP address) or go to https://gitter.im/chocolatey/choco to contact the community team. You can also send an email to community at chocolatey dot io. As we have limited information (only an IP address), please include your name, email address, phone number, and the IP addresses you believe are blocked so we can contact you and verify if there is a block.

See the section above on avoiding excessive use - the expectation is that organizations would not use the community repository directly. As part of addressing any misconfigurations you might have, you will also need to see about addressing the previous section on "How To Avoid Excessive Use".

Once you have resolved any issues on your side, we can lift the block. A block will be reimplemented later if we find excessive use again.

## Community Provided Packages Are Not Supported

The packages contained at https://chocolatey.org/packages (the community repository) are collectively known as the Community Feed or Community Repository. Packages found on the community repository may not be supported by the original software vendor. If you have an issue with a package, contact the package maintainers through the community repository package page, Do not contact the software vendor directly. It bears repeating, DO NOT contact the software vendor directly.

The packages you find on the community repository are completely unsupported. By using the packages on the community repository, you assume all risk for any issues or damages that may occur. The packages found in the community repository are built, moderated, and maintained by the community. While the packages are moderated by community moderators to help ensure safety and reliability (at the time of moderation), package maintainers and moderators assume no support nor liability for the packages. Neither package moderators nor package maintainers will be held liable for any issues, downtime, or damages that may occur from your use of the packages on the community feed. It is also not the responsibility of the package maintainers and/or package moderators to ensure that the package works in all scenarios. They are volunteers and thus are unable to support you and/or your business needs.

Support is not guaranteed by package maintainers, but should you encounter issues, please work directly with the maintainers to get the package(s) corrected. Do NOT contact the software vendor directly.

Transferring a package to another repository (even internal) with or without changes does not change the support level by the maintainers of Chocolatey, by the moderators nor maintainers, and not by Chocolatey Software, Inc.

## Put It Another Way

Due to software distribution rights, many of the packages contained on the community repository must download actual software from official distribution points, thus creating a dependency on those internet locations not changing (and from time to time they do, thus breaking a package until it is updated). This introduces a failure point for the community repository that is not found with internal packages. When hosting your own internal packages you would not be subject to software distribution rights, so you could embed the software directly in the package or pull from an internal share location, thus guaranteeing a reliable and repeatable software management process.

For folks in the community that are not using Chocolatey for production purposes, with an understanding of the disclaimers set forth here, it is fine to use the community repository. For organizations that have a low tolerance for breakages and require a higher level of reliability, security, trust, and control, a self-hosted Chocolatey server is the recommended option. This guarantees that your installs, upgrades, and uninstalls will always work every time. This gives you complete control over what software gets installed. Also because you are vetting newer versions, you control when those newer versions are available for upgrade. Please see [Host Your Own Server](How-To-Host-Feed) for more information.

<!-- Please see [Terms of Use] for any additional considerations.-->

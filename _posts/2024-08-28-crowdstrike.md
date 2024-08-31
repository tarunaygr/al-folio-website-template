---
layout: post
title: The Billion Dollar Bug
date: 2024-08-28 
description: The crowdstrike incident that took down the internet
tags: crowdstrike2024
categories: tech-news
---
<!-- TOC -->

1. [What is Crowdstrike Falcon?](#what-is-crowdstrike-falcon)
2. [The Incident](#the-incident)
3. [The Bug](#the-bug)
4. [The Effects and Conclusion](#the-effects-and-conclusion)

<!-- /TOC -->
How was your day on Friday, 19th July 2024? Most of the world had a pretty terrible day, especially if you were travelling. If you were at work, you probably got an early weekend. This all stemmed from a bug in a Endpoint Detection and Response (EDR) software called Crowdstrike Falcon. Let's talk about what really happened.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0 col-lg-12">
        {% include figure.html path="assets/img/crowdstrike/logo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# What is Crowdstrike Falcon?

<a href="https://www.crowdstrike.com/en-us/">Crowdstrike</a> is a USA based cybersecurity company that provides a wide range of security services such as consulting services, incident response and even training courses. Their cloud based all-in-one platform, `Crowdstrike Falcon` is ranked #1 with approximately 23.46% marketshare according to <a href="https://6sense.com/tech/endpoint-protection/crowdstrike-market-share">6sense</a> as of writing this blog post.

# The Incident

On 19th July 2024, some of the biggest airlines, TV broadcasters, banks and other essential services were faced with a Windows Blue Screen of Death (BSOD). According to Microsoft, approximately <a href="https://www.eccouncil.org/cybersecurity-exchange/incident-handling/crowdstrike-incident/">8.4 million</a> Windows devices across the world were affected by the outage. The systems were down for most of the day. Damanges to businesses are estimated to be more than <a href="https://www.cnn.com/2024/07/21/business/crowdstrike-outage-cost/index.html"> USD $1 billion</a>. This is in addition to the millions of people whose jobs, travel and other activities were distruped to the outage. So what exactly happened?

# The Bug
First thing to clarify - It was NOT a cyber attack. It was due to a BUG. Since the incident, Crowdstrike has released a <a href="https://www.crowdstrike.com/wp-content/uploads/2024/08/Channel-File-291-Incident-Root-Cause-Analysis-08.06.2024.pdf">Root Cause Analysis</a> of the incident. The report explains how the software works and how/why it failed, leading to the system crashes.

I will attempt to explain the bug without getting too techincal.

The Crowdstrike Falcon platform uses machine learning models and on-sensor AI to detect and remediate threats. A part of this is the `Sensor Detection Engine`. The Sensor Detection Engine uses built in sensor data and Rapid Response Content which is delivered from the cloud to detect malicious behaviour. The Rapid Response Content is delivered through channel files and are interpreted by the Content Interpreter using regular expressions (regex). Each Rapid Response file is a reference to which the live sensor data is to be compared against to help identify a particular type of attack.

Earlier in February 2024, Crowdstrike created a new template type to detect attacks that abuse the Windows Inter Process Communication (IPC) machanisms. This template was delivered as `C-00000291*.sys` (`Channel File 291`) files. The problem lies in these particular files. Channel File 291 defined 21 input parameters. However, the content interpreter only invoked the Template code with 20 parameters. This template used wildcard matching for the 21st parameter, so it worked fine without needing the last parameter to be passed.

On July 19, 2024, Crowdstrike pushed an update that added 2 more templates to Channel File 291. One of these templates did not use wildcard matching for the 21st parameter. Therefore, when the template was evaluated, the content interpreter only expected 20 parameters. When the template tried to access the 21st parameter, it lead to an out-of-bounds memory read and that resulted in a system crash. 

What differentiates this from a normal Windows crash which can usually be fixed with a restart, is that anitivirus drivers are usually <a href="https://learn.microsoft.com/en-us/windows-hardware/drivers/install/installing-a-boot-start-driver">boot-start drivers</a>. These drivers are loaded in before Windows is. These drivers are mandatorily loaded for Windows to boot up to a normal state. Since the issue was in a boot-start driver, users were unable to even log into windows and were stuck in an infinte BSOD loop.

The temporary fix that users soon discovered  was to boot into Windows Safe Mode. In Safe Mode, Windows loads only the bare minimum operating system which are strictly required for Windows operation (none of the additional applications are loaded). Users could then delete the responsible Channel 291 files which were causing the crashes. Running the following command in powershell or command prompt while in safe mode would delete all the faulty files
```bash
del "C:\Windows\System32\drivers\CrowdStrike\C-00000291*.sys"
```
Doing this removed the broken templates and Windows would continue normal operation as if that template did not exist.

The <a href="https://www.crowdstrike.com/wp-content/uploads/2024/07/Building-CrowdStrike-Bootable-Recovery-Images-2.pdf">official fix </a>released by Crowdstrike instructs users to download their recovery tool, create a minimal driver ISO image, create a bootable USB drive and boot off of the drive. The recovery tool will automatically delete the faulty channel files.

# The Effects and Conclusion

The bug resulted in one of the largest Denial of Service (DoS) in recent memory. Crowdstrike reported that it had 24,000 customers at the time. A part of their clientele includes <a href="https://techcrunch.com/2024/07/19/faulty-crowdstrike-update-causes-major-global-it-outage-taking-out-banks-airlines-and-businesses-globally/">60% of the Fortune 500 and more than half of the Fortune 1000 companies </a>. The losses faced by the these companies were in the billions. Additionally, <a href="https://www.bbc.com/news/live/cnk4jdwp49et">around 5,078 flights </a>across the world were cancelled. Numerous other industries such a healthcare, retail and media also faced outages.

In their RCA, Crowdstrike highlights the series of internal testing failures that lead to the bug escaping their notice and lists a number of fixes including a check on the number of parameters, memory bounds checking etc. I will skip over the rest of the list in this blog. Readers are welcome to learn more in the RCA. 

My concluding thoughts on this - Trusting anyone or any company with kernel level access to your Windows will always carry an inherent risk. Of course, a company as reputed as Crowdstrike can be trusted (mostly). However, this could easily have been a spyware that you downloaded without knowing. We were "lucky" in a sense that it was only a bug that caused a system crash. If it was a vulnerability that could have allowed for Remote Code Execution (RCE) or any other major vulnerability and if discovered by a malicious party, it could leave a huge number of machines on the internet completely helpless. Always be careful and only install software from verified sources (even those are not always safe).

That concludes this blog post of the Crowdstrike incident. I hope you found it informative and stay tuned for more such interesting topics!! Thank you for reading, please feel free to share your thoughts and suggestions :)
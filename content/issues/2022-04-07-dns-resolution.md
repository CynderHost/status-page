---
section: issue
title: DNS Resolution
date: 2022-04-07T03:38:27.275Z
resolved: true
draft: false
informational: false
resolvedWhen: 2022-04-07T03:38:27.283Z
affected:
  - DNS 1 (hp1.cynderhost.com)
  - DNS 2 (hp2.cynderhost.com)
  - DNS 3 (hp3.cynderhost.com)
severity: disrupted
---
*Investigating* - We are investigating a potential issue that might affect the uptime of one our of services. We are sorry for any inconvenience this may cause you. This incident post will be updated once we have more information.

*Mitigating -* We are currently working on mitigating this issue for affected domains.

*Mitigating -* We have mitigated the issue for affected domains for a large majority of clients. We continue to work to resolve remaining issues.

*Mitigated*  - At this time, all systems have recovered through a temporary mitigation. We'll continue to work towards a permanent resolution in a non-impacting manner.

*Resolved -* At this time, a permanent solution has been implemented and this issue should not happen again. We apologize for any impact caused by this.

**Post Mortem:**

On Wednesday, April 5th @ 8:38 PM, a bug in the system that synchronizes DNS records between our edge DNS servers and main control panel resulted in corruption of records for certain domains being pushed to our edge resolvers. Due to the nature of this bug, we had to work in tandem with our third-party vendors to implement a permanent mitigation. 

To restore service and DNS resolution, our nameservers were temporarily failed-over to our backup DNS system. This may have resulted in increased latency, but mitigated the issue for all users and restored resolution service. The scope of this outage lasted ~25 minutes -- many sites likely saw little to no disruption as DNS records are cached by intermediate resolvers, and locally by browsers and operating systems.

After working closely with our vendors, we've now deployed a permanent fix to: 

a) Restore previously corrupted DNS records across our DNS network

b) Resolve the bug that initially led to corruption

c) Implement more concrete deployment checks to ensure any DNS record issues are not deployed globally

We understand that any issue - regardless of scope or length - greatly impacts many of our client's essential operations. We apologize for any disruption that occurred, and we work to eliminate any causes of similar issues in the future, include revisiting our DNS deployment systems to make them more redundant. Please reach out to us at support@cynderhost.com with any questions or concerns.
---
title: October 13th Proton Outage Post Mortem
draft: false
---
## **October 13th Proton Outage Post Mortem**

At 00:45 AM PST, our team responded to multiple alerts indicating elevated backend error rates. After triaging the incident, we isolated the issue to a problem with Apache (HTTPD) failing and causing backend unreachability, and subsequently, 5XX errors.

Upon further investigation, we identified the root cause of the issue as a WAF rule channel vendor timing out on HTTPD startup, causing the server to be killed before starting up. One of our WAF rules vendors hosts their rules feed on OVH's network, which experienced a global backbone failure at the time and brought their entire network offline. Our team disabled the rulesets in question and brought the server online.

Resolution time was compounded by the fact that our primary telemetry instances were also hosted on OVH's network and affected by the outage, requiring us to use a backup platform.

We build our infrastructure to be extremely resilient, implementing multiple levels of redundancy and self-healing checks. While we don't utilize OVH's network for end clients and such outage should not have caused any noticeable issues, we sincerely regret that this was not the case and apologize for any inconvenience or impact caused.

Moving forward, we’ll first and foremost be stripping Apache’s startup process of any external dependencies. Fetching an external rule on startup provides several advantages. By combining these updates with startup, we save unnecessary restarts, while being able to deploy rules with an extremely low time-to-live as needed. There are built-in failsafes to ensure that an unavailable distribution channel is simply ignored instead of blocking. However, due to a behavior of Apache, this failsafe contains edge cases which cannot be resolved. Therefore, to ensure continued stability, we’ll be developing an alternative.

Furthermore, we’ll be auditing our reliance on third-party networks, to ensure similar incidents of this kind do not affect overall availability of our core platform, in addition to further training with various failover and incident recovery scenarios.

We’d like to thank everyone for their understanding, and as always, encourage anyone with questions or feedback to reach us at support@cynderhost.com
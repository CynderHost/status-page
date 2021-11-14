---
title: November 10th Proton Outage Post Mortem
draft: false
---
## November 10th Proton Outage Post Mortem

We take a lot of pride in our High Performance platform - offering unrivaled performance, scalability, and security at unheard of prices. At the same time, building such a platform without upholding stringent standards of reliability is pointless: fast websites are no use offline.

On November 10th, at 3:32 PM PST, we failed to meet this standard and suffered nearly 1 hour, and 30 minutes of backend downtime. This, compounded with other recent incidents (eg, October 13th service issue) have resulted in a less-than-stellar year of uptime for our High Performance platform, hovering at around 99.95% actual uptime - a distinct deviance from our internal benchmark of 99.99% that we aim to achieve. 

To say we’ve learned a lot through these incidents would be a massive understatement. Yet, this isn’t a beta platform, and everything we deploy should be reliably production ready. 

Throughout these incidents, a commonality emerges in many: backend issues as a result of upstream storage, or network issues. Running a highly redundant and reliable infrastructure platform is not an easy task; we’ve made significant investments in upstream providers, in addition to extensive evaluations to ensure our providers are highly competent. The result of this was our migration to “[cloud](https://blog.cynderhost.com/the-migration-to-cloud/)” many months back. This reliability of this infrastructure provider has obviously been subpar in recent months, and for that, we are to blame. It was, after all, our choice.

We won’t bore you with how we’re sorry about what the service disruption caused to you and your business in the recent year -- that goes without saying, and we take every incident with the utmost seriousness -- publishing in-depth post-mortems in the interest of transparency.

What most clients are probably concerned with are: 

1. What happened on 11/10?
2. How do we ensure those events truly do not recur again?

**To address #1:**

At 3:30 PM, PST, we received multiple server unresponsive alerts and our engineering teams were paged. The cause was quickly triaged to a provider issue impacting complete network availability.

Our next step was to establish scope and a timeline for the incident, so we reached out to our upstream. Working with them, the issue was isolated, and a mitigation was put in place, which brought systems back online by 4:58 PM PST. During this time, uncached requests returned 5XX errors, while cached requests were normal. We saw approximately 25-40% of all HTTP requests return an error code, though these errors were not evenly dispersed across sites on our network (as sites have varying degrees of cacheability).

The cause of this network failure was related to an internal routing loop that took down the entire network of redundant routers. The root cause of the routing loop is currently unknown, and all resources possible are being dedicated to identify, and resolve the issues in question to ensure that they do not happen again. Naturally, this post-mortem will be updated as we get more details (we hope to have a concrete picture by the end of next week).

**To address #2:**

We are considering a variety of solutions to address this issue. This first priority is remediating the root cause above to ensure it doesn’t happen again.

Yet we also want to do more to hedge against any potential, inevitable backend issues (such issues would be a problem anywhere - even big providers (Google Cloud, Azure, AWS) have issues). These include: hourly, offsite server replication failover, real-time server replication failover, and pre-generated HTML site fallback. 

Obviously, these changes require significant investment, planning, and pose logistical challenges (such as re-synchronizing servers after a failover event). As such, this isn’t something we can immediately make a decision on - we expect more information forthcoming in the following weeks.

**Compensation**

We’re going to take precedence from our previous policy and offer 20% credit for anyone wishing to claim the SLA - just open a ticket in our client billing area. For reference, this is \~24x our normal 8x SLA (\~196x compensation total).

We understand this is probably not the post-mortem clients are looking for as of now, and there are many parts that will be updated further down the line. However, we wanted to address the issue in a timely manner and relay what we have so far.

As always, please reach out with any questions

The CynderHost Team
Issues can be reported 
1. manually (e.g., through support, a team member, or by a customer directly)
2. automatically through an [observability solution](https://grafana.com/products/cloud/incident/?pg=blog&plcmt=body-txt)

#### Severity levels

- **Critical:** Urgently requires immediate attention when any of the following is true
    - System is down and unable to function with no workaround available
    - All or a substantial portion of data is at significant risk of loss or corruption
    - Business operations may have been severely disrupted
- **Major:** Significant blocking problem that requires help
    - Operations can continue in a restricted fashion, although long-term productivity may be impacted
    - A major milestone, product, or customer is at risk
- **Minor:** May be affecting customers but no one is blocked
    - No notable impact, or only a minor impact on operations and customers

If you have a globally distributed team, you might want to consider a _follow-the-sun_ rotation in which you’re only on-call during regular working hours. For smaller, geographically centralized teams, rotating weekly is a good starting point.

#### Who does what?

##### Investigator

Runbooks are documents outlining the identification and resolution of known or anticipated problems (e.g. a disk filling up, certificates not renewing).

> _Tip: Prioritize mitigation of the symptoms of the incident over fixing the issue in a permanent way within the handling of the incident. The goal is the shortest time to mitigate the impact that the incident produces whilst preserving evidence that aids the investigation._
##### Commander

The commander communicates the status of the incident to other teams (and customer support, if applicable) and helps the investigator to prioritize. They can also offer debugging ideas and hypotheses but only in a hands-off manner. Another responsibility is extracting the important information found by the investigator to document the incident as it progresses.
##### Other people

 Declaring an incident gives the incident commander authority to commandeer any person within the company at any level to help contribute towards the resolution of the incident.
 
> The investigator remains the owner of the investigation, and other individuals involved should work to support the investigator.

#### What happens afterward?

As soon as the user-facing impact is mitigated, the incident can be considered _resolved_.

1. **Creating a post-incident review**: as an incident commander, you’re tasked with creating a post-incident review (PIR) document. The PIR is used to document what happened during the incident and how it was resolved.
2. **Creating follow-up tasks**: it is important to create follow-up tasks directly after resolving the incident. Whether this task is fixing a bug, changing the configuration or rewriting the whole application from scratch depends on the root cause of the incident.



# References:

1. [SLA, SLO, and SLI](https://medium.com/@karan99/system-design-sla-slo-sli-4b1e575d7f94)
2. ~~[Call me, maybe: designing an incident response process](https://grafana.com/blog/2024/03/28/call-me-maybe-designing-an-incident-response-process/)~~
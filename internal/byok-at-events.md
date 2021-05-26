---

copyright:
  years: 2020, 2021
lastupdated: "2021-04-01"

keywords: crypto erasure, enable BYOK, onboard to Key Protect, Key Protect onboarding, internal, dek rewrapping, BYOK

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Log {{site.data.keyword.keymanagementserviceshort}} events using {{site.data.keyword.at_full_notm}}
{: #log-kp-events}

Log {{site.data.keyword.keymanagementserviceshort}} actions by using Activity
Tracker.
{: shortdesc}

This content is currently being developed and reviewed. For questions about BYOK
and {{site.data.keyword.at_full_notm}}, reach out to Mala Anand (`@manand`).
{: note}

## Impacts to your service
{: #byok-at-impacts}

When a customer calls the {{site.data.keyword.keymanagementserviceshort}} API,
{{site.data.keyword.keymanagementserviceshort}} creates a `correlation-id`
that's used to correlate and track the transaction.
{{site.data.keyword.keymanagementserviceshort}} uses the `correlation-id` for
all the logs that are related to processing of these APIs to {{site.data.keyword.at_full_notm}}.

Because {{site.data.keyword.keymanagementserviceshort}} passes this correlation
ID as part of a key lifecycle event notification to services, all integrated
services must also use the same correlation ID to log their processing of the
events.

As an integrated service, be sure to log the actions that are taken for the
resources that are impacted by root key actions, such as key deletion or
rotation. This will allow customers to track key lifecycle events and all the
 actions that are triggered by an event, including successes and failures.

- To view of a list of {{site.data.keyword.keymanagementserviceshort}} actions
that generate an event, see
[{{site.data.keyword.at_full_notm}} events](/docs/key-protect?topic=key-protect-at-events).

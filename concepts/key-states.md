---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-16"

keywords: encryption key states, encryption key lifecycle, manage key lifecycle

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:term: .term}

# Monitoring the lifecycle of encryption keys
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} follows the security guidelines by [NIST SP 800-57 for key states](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}.
{: shortdesc}

## Key states and transitions
{: #key-transitions}

Cryptographic keys, in their lifetime, transition through several states that are a function of how long the keys are in existence and whether data is protected. 

{{site.data.keyword.keymanagementserviceshort}} provides a graphical user interface and a REST API for tracking keys as they move through several states in their lifecycle. The following diagram shows how a key passes through states between its generation and its destruction.

![The diagram shows the same components as described in the following definition table.](../images/key-states_min.svg)

| State | Description |
| --- | --- |
| Pre-activation | Keys are initially created in the _pre-activation_ state. A pre-active key cannot be used to cryptographically protect data.|
| Active | Keys move immediately into the _active_ state on the activation date. This transition marks the beginning of a key's cryptoperiod. Keys with no activation date become active immediately and remain active until they expire or are destroyed. |
| Deactivated | A key moves into the _deactivated_ state on its expiration date, if one is assigned. In this state, the key is unable to cryptographically protect data and can only be moved to the _destroyed_ state.|
| Destroyed | Deleted keys are in the _destroyed_ state. Keys in this state are not recoverable. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database. |
{: caption="Table 1. Describes key states and transitions." caption-side="top"}

After you add a key to the service, use the {{site.data.keyword.keymanagementserviceshort}} dashboard or the {{site.data.keyword.keymanagementserviceshort}} REST APIs to view your key's transition history and configuration. For audit purposes, you can also monitor the activity trail for a key by integrating {{site.data.keyword.keymanagementserviceshort}} with the {{site.data.keyword.cloudaccesstrailfull}}. After both services are provisioned and running, activity events are generated and automatically collected in a {{site.data.keyword.cloudaccesstrailshort}} log when you create and delete keys in {{site.data.keyword.keymanagementserviceshort}}. 

For more information, see [Monitoring {{site.data.keyword.keymanagementserviceshort}} activity](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: external}.
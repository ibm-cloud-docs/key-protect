---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Monitoring the lifecycle of encryption keys
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} follows the security guidelines by [NIST SP 800-57 for key states ![External link icon](../../../icons/launch-glyph.svg "External link icon")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}.
{: shortdesc}

## Key states and transitions
{: #key_transitions}

Cryptographic keys, in their lifetime, transition through several states that are a function of how long the keys are in existence and whether data is protected. 

{{site.data.keyword.keymanagementserviceshort}} provides a graphical user interface and a REST API for tracking keys as they move through several states in their lifecycle. The following diagram shows how a key passes through states between its generation and its destruction.

![The diagram shows the same components as described in the following definition table.](../images/key-states_min.svg)

<table>
  <tr>
    <th>State</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Pre-activation</td>
    <td>Keys are initially created in the <i>pre-activation</i> state. A pre-active key cannot be used to cryptographically protect data.</td>
  </tr>
  <tr>
    <td>Active</td>
    <td>Keys move immediately into the <i>active</i> state on the activation date. This transition marks the beginning of a key's cryptoperiod. Keys with no activation date become active immediately and remain active until they expire or are destroyed.</td>
  </tr>
  <tr>
    <td>Deactivated</td>
    <td>A key moves into the <i>deactivated</i> state on its expiration date, if one is assigned. In this state, the key is unable to cryptographically protect data and can only be moved to the <i>destroyed</i> state.</td>
  </tr>
  <tr>
    <td>Destroyed</td>
    <td>Deleted keys are in the <i>destroyed</i> state. Keys in this state are not recoverable. Metadata that is associated with a key, such as the key's transition history and name, is kept in the {{site.data.keyword.keymanagementserviceshort}} database. </td>
  </tr>
  <caption style="caption-side:bottom;">Table 1. Describes key states and transitions.</caption>
</table>

After you add a key to the service, use the {{site.data.keyword.keymanagementserviceshort}} dashboard or the {{site.data.keyword.keymanagementserviceshort}} REST APIs to view your key's transition history and configuration. For audit purposes, you can also monitor the activity trail for a key by integrating {{site.data.keyword.keymanagementserviceshort}} with the {{site.data.keyword.cloudaccesstrailfull}}. After both services are provisioned and running, activity events are generated and automatically collected in a {{site.data.keyword.cloudaccesstrailshort}} log when you create and delete keys in {{site.data.keyword.keymanagementserviceshort}}. 

For more information, see [Monitoring {{site.data.keyword.keymanagementserviceshort}} activity ![External link icon](../../../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: new_window}.
---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-16"

keywords: monitor deletion, data associated with key, monitor data deletion

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
{:preview: .preview}
{:term: .term}

# Monitoring the crypto-shredding of data
{: #monitor-crypto-shredding}

Monitor crypto-shredding activity in your account by using
{{site.data.keyword.at_full}} and
{{site.data.keyword.keymanagementservicelong_notm}}.
{: shortdesc}

## This content is being developed and reviewed
{: #monitor-crypto-shredding-review}

This content is currently being developed and reviewed.

Still needs updates. Besides monitoring for crypto-shredding activity, would be
good to add info about monitoring for rotation events, disable/enable events,
etc. I think I had an issue in the backlog for that one.

## End this content is being developed and reviewed
{: #monitor-crypto-shredding-review-end}

Cryptographic shredding (or crypto-shredding) is the concept of invalidating
data through the destruction of the cryptographic keys that protect the data.
Without the decryption keys, the encrypted data is unusable, much like a safe
without the combination. The data itself is not destroyed, but no one can access
the encrypted data because the decryption keys are destroyed.

| Benefit | Description |
| ------- | ----------- |
| Cryptographic shredding of data | When you delete a {{site.data.keyword.keymanagementserviceshort}} key that cryptographically protects an {{site.data.keyword.cloud_notm}} resource, such as a storage bucket or database deployment, you want assurance that the key's associated data can no longer be accessed or decrypted. |
| Auditing and monitoring | By integrating your {{site.data.keyword.keymanagementserviceshort}} service with [{{site.data.keyword.at_full}}](/docs/activity-tracker?topic=activity-tracker-getting-started){: external}, you can monitor crypto-shredding activity of your resources and create alerts that notify you when crypto-shredding is complete. |

## Planning ahead for key deletion
{: #plan-ahead-deletion}

Keep the following considerations in mind when you're ready to delete a key
that's used across {{site.data.keyword.cloud_notm}} services.

- **Determine what data is protected by the key**.
A single root key can cryptographically protect more than one
{{site.data.keyword.cloud_notm}} resource. Before you delete a key,
[review associations between the key and your {{site.data.keyword.cloud_notm}} apps and services](/docs/key-protect?topic=key-protect-view-protected-resources).
- **Ensure that you no longer need the associated data.**
Keep in mind that after you delete a key, the action cannot be reversed, and any
data that is associated with the key is immediately lost at the moment the key
is deleted. Before you delete a key, ensure that you no longer require access to
it. Do not delete a key that is actively protecting data in your production
environments.
- **Monitor activity by using {{site.data.keyword.at_full_notm}} with {{site.data.keyword.la_full}}.**
For compliance posterity, you can set up an {{site.data.keyword.at_full_notm}} instance in the
same region as your {{site.data.keyword.keymanagementserviceshort}} instance to
monitor for crypto-shredding events.

To learn more, see
[{{site.data.keyword.at_full_notm}} events](/docs/key-protect?topic=key-protect-at-events).

## Monitoring crypto-shredding activity in your account
{: #monitor-cryptoerasure-activity}

After you delete a key, {{site.data.keyword.keymanagementserviceshort}} creates
{{site.data.keyword.at_full_notm}} events that you can track and audit in the {{site.data.keyword.at_full_notm}} web
UI.

Don't have an {{site.data.keyword.at_full_notm}} instance? To view crypto-shredding activity,
[create an {{site.data.keyword.at_full_notm}} for {{site.data.keyword.la_full}} instance](/docs/activity-tracker?topic=activity-tracker-provision){: external}
in the same region as your {{site.data.keyword.keymanagementserviceshort}}
instance.
{: tip}

### Tracking successful crypto-shredding events
{: #track-success-events}

When you force deletion on a key that is protecting one or more cloud resources,
{{site.data.keyword.keymanagementserviceshort}} notifies the cloud services that
use the key. The notification triggers actions in services that invalidate the
key's associated data encryption keys (DEKs).

After {{site.data.keyword.keymanagementserviceshort}} receives confirmation from
those services that all associated DEKs are crypto-shredded, you receive an
event in your {{site.data.keyword.at_full_notm}} web UI to show that the erasure is complete.

After you delete a key, allow up to 4 hours before the corresponding
crypto-shredding event is displayed in the {{site.data.keyword.at_full_notm}} web UI. To find out
more about viewing {{site.data.keyword.at_full_notm}} events, see
[Viewing events](/docs/activity-tracker?topic=activity-tracker-view_events){: external}.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Observability** to access your
    [Observability dashboard](/observe){: external}.

3. Select the `Activity Tracker` link to gain insight into your services.

4. Select an {{site.data.keyword.at_full_notm}} instance, and click **View {{site.data.keyword.la_full_notm}}** to launch the
    web UI.

5. Search for successful crypto-shredding events by using the following query.

    ```plaintext
    action:kms.secrets.endtoenddelete outcome:success
    ```
    {: codeblock}

    The following {{site.data.keyword.at_full_notm}} log shows that a crypto-shredding sequence
    completed successfully.

    ```json
    {
        "typeURI": "http://schemas.dmtf.org/cloud/audit/1.0/event",
        "type": "ActivityTracker",
        "eventType": "activity",
        "eventTime": "2019-12-20T16:38:06Z",
        "outcome": "success",
        "message": "keymonitor deactivate",
        "action": "kms.secrets.endtoenddelete",
        "initiator": {
            "id": "iam-ServiceID-",
            "name": "ServiceID-",
            "typeURI": "service/security/account/serviceid",
            "credential": {
                "type": "apikey"
            }
        },
        "target": {
            "id": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:12e8c9c2-a162-472d-b7d6-8b9a86b815a6:key:02fd6835-6001-4482-a892-13bd2085f75d",
            "name": "Key Protect",
            "typeURI": "key-protect/keys"
        },
        "observer": {
            "name": "ActivityTracker",
            "typeURI": "security/edge/activity-tracker"
        },
        "requestPath": "/keymonitor/deletion",
        "reason": {
            "reasonCode": 200
        },
        "severity": "normal",
        "responseBody": {
            "keyId": "2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:30372f20-d9f1-40b3-b486-a709e1932c9c:key:2291e4ae-a14c-4af9-88f0-27c0cb2739e2",
            "KeyDeletionDate": "2019-12-20T16:38:06Z",
            "message": "End to end key deletion for this key is complete.",
            "correlationID": "436901cb-f4e4-45f4-bd65-91a7f6d13461"
        },
        "meta": {
            "serviceProviderName": "kms",
            "serviceProviderProjectId": "production-project-id"
        }
    }
    ```
    {: screen}

    You can check the `reason.reasonCode` field to determine the success or
    failure of a crypto-shredding sequence. A `200` response indicates that the
    crypto-shredding of any data that was associated with a key completed
    successfully. The `responseBody` object contains details of the event, such
    as the identifying information of the key and the timestamp of the key
    deletion.

### Tracking unsuccessful crypto-shredding events
{: #track-unsuccessful-events}

You also receive {{site.data.keyword.at_full_notm}} events when crypto-shredding is still pending
for cloud resources that are associated with a deleted
{{site.data.keyword.keymanagementserviceshort}} key.

For example, if you delete a key, but a cloud service isn't able to confirm
within 4 hours that the associated data was crypto-shredded, you receive an
{{site.data.keyword.at_full_notm}} event that lists which cloud resources are pending deletion.

After you delete a key, allow up to 4 hours before the corresponding
crypto-shredding event is displayed in the {{site.data.keyword.at_full_notm}} web UI. To find out
more about viewing {{site.data.keyword.at_full_notm}} events, see
[Viewing events](/docs/activity-tracker?topic=activity-tracker-view_events){: external}.
{: note}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.

2. Go to **Menu** &gt; **Observability** to access your
    [Observability dashboard](/observe){: external}.

3. Select the `Activity Tracker` link to gain insight into your services.

4. Select an {{site.data.keyword.at_full_notm}} instance, and click **View {{site.data.keyword.la_full_notm}}** to launch the
    web UI.

5. Search for unsuccessful crypto-shredding events by using the following query.

    ```plaintext
    action:kms.secrets.endtoenddelete outcome:failure
    ```
    {: codeblock }

    The following {{site.data.keyword.at_full_notm}} log shows that a crypto-shredding sequence
    did not complete successfully.

    ```json
    {
        "typeURI": "http://schemas.dmtf.org/cloud/audit/1.0/event",
        "type": "ActivityTracker",
        "eventType": "activity",
        "eventTime": "2019-12-11T16:00:58-06:00",
        "outcome": "failure",
        "message": ": keymonitor deactivate",
        "action": "kms.secrets.endtoenddelete",
        "initiator": {
            "id": "iam-ServiceID-",
            "name": "ServiceID-",
            "typeURI": "service/security/account/serviceid",
            "credential": {
                "type": "apikey"
            }
        },
        "target": {
            "id": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:6ba8beba-a85b-4b7a-9dbd-97db8e1eea83:key:51eb34cd-93ef-4795-a32d-638632f1f070",
            "name": "Key Protect",
            "typeURI": "key-protect/keys"
        },
        "observer": {
            "name": "ActivityTracker",
            "typeURI": "security/edge/activity-tracker"
        },
        "requestPath": "/keymonitor/deletion",
        "reason": {
            "reasonCode": 408
        },
        "severity": "warning",
        "responseBody": {
            "keyId": "7438a498-a487-4a05-ae0b-02ed8b1353ab",
            "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8ccee289-acef-48a7-bd78-67d95688e2ac:key:7438a498-a487-4a05-ae0b-02ed8b1353ab",
            "KeyDeletionDate": "2019-12-20T16:38:06Z",
            "message": "End to end deletion for this key is not yet complete. Check the outstandingResourceCRNs field to review which cloud resources have registrations to this key that are pending deactivation.",
            "outstandingResourceCRNs": "crn:v1:bluemix:public:cloud-object-storage:global:a/f047b55a3362ac06afad8a3f2f5586ea:90db9b77-936f-4411-ae25-ad97252a0260:bucket:test-bucket,crn:v1:bluemix:public:cloud-object-storage:global:a/f047b55a3362ac06afad8a3f2f5586ea:a84ed5e7-5fe5-477d-a77b-26a2a253ca72:bucket:another-test-bucket",
            "correlationID": "b32a1e29-16b2-47ef-b89a-49a15cd18715"
        },
        "meta": {
            "serviceProviderName": "kms",
            "serviceProviderProjectId": "production-project-id"
        }
    }
    ```
    {: screen}

    In the `reason.reasonCode` field, the `408` response indicates that the Key
    Protect request to cloud services timed out. The `outstandingResourceCRNs`
    value shows a list of cloud resources that are pending invalidation by the
    cloud service.

    If you receive an unsuccessful crypto-shredding event, you can
    [create a support case](/unifiedsupport/cases/form){: external}
    with the respective cloud service. In the list of `outstandingResourceCRNs`,
    see the `crn:v1:bluemix:public:<service-name>` segment to understand which
    cloud service has not yet confirmed the cryptoerasure of resources.

## Creating alerts for crypto-shredding activity in your account
{: #create-cryptoerasure-alerts}

With {{site.data.keyword.at_full_notm}} for {{site.data.keyword.la_full_notm}}, you can configure alerts to monitor activity
in your account. After you delete a key, you can send alerts to a specified
notification channel to notify you when the crypto-shredding of data is
complete.

To configure alerts for your account, you must have a paid service plan for
{{site.data.keyword.at_full_notm}} with {{site.data.keyword.la_full_notm}}.

To learn more, see
[Service plans](/docs/activity-tracker?topic=activity-tracker-service_plan){: external}.
{: note}

To configure an alert for monitoring cryptoerasure activity:

1. Ensure that you have an {{site.data.keyword.at_full_notm}} instance provisioned in the same
    region as your {{site.data.keyword.keymanagementserviceshort}} instance.

2. [Create a custom view](/docs/activity-tracker?topic=activity-tracker-views#views_step3){: external}
    that searches for `kms.secrets.endtoenddelete` actions in your account.

3. [Attach an alert to your custom view](/docs/activity-tracker?topic=activity-tracker-alerts#alerts_step3_view){: external}
    by specifying an alert notification channel.




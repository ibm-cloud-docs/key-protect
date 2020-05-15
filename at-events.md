---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-14"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- TODO: update toc file
  Include your AT events file in the Reference nav group in your toc file.
  Make sure that the AT events file has the H1 ID set to: {: #at_events}
-->

# Activity Tracker events
{: #at-events}

As a security officer, auditor, or manager, you can use the Activity Tracker
service to track how users and applications interact with {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change
the state of a service in {{site.data.keyword.cloud_notm}}. You can use this
service to investigate abnormal activity and critical actions and to comply with
regulatory audit requirements. In addition, you can be alerted about actions as
they happen. The events that are collected comply with the Cloud Auditing Data
Federation (CADF) standard.

For more information, see the
[getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started).

<!-- TODO: re-format table
  If you have multiple events that might not be related, you can create
  different sections to group them.

  The following actions create a table with 4 tabs, which is not easy to
  navigate. Consider 4 tables, vertically, to make is easier to read.
-->

## List of events
{: #at-actions}

The following table lists the {{site.data.keyword.keymanagementserviceshort}}
actions that generate an event:

| Action                            | Description                  |
| --------------------------------- | ---------------------------- |
| `kms.secrets.create`              | Create a key                 |
| `kms.secrets.read`                | Retrieve a key               |
| `kms.secrets.readmetadata`        | Retrieve details about a key |
| `kms.secrets.head`                | Retrieve key total           |
| `kms.secrets.list`                | List keys                    |
| `kms.secrets.wrap`                | Wrap a key                   |
| `kms.secrets.unwrap`              | Unwrap a key                 |
| `kms.secrets.rewrap`              | Rewrap a key                 |
| `kms.secrets.rotate`              | Rotate a key                 |
| `kms.secrets.setkeyfordeletion`   | Authorize deletion for a key  |
| `kms.secrets.unsetkeyfordeletion` | Cancel deletion for a key    |
| `kms.secrets.delete`              | Delete a key                 |
{: #table-1}
{: caption="Table 1. {{site.data.keyword.keymanagementserviceshort}} actions that generate Activity Tracker events" caption-side="top"}
{: tab-title="Keys"}
{: tab-group="AT-events"}
{: class="comparison-tab-table"}

| Action                       | Description            |
| ---------------------------- | ---------------------- |
| `kms.policies.read`          | List key policies      |
| `kms.policies.write`         | Set key policies       |
| `kms.instancepolicies.read`  | List instance policies |
| `kms.instancepolicies.write` | Set an instance policy |
{: #table-2}
{: caption="Table 2. {{site.data.keyword.keymanagementserviceshort}} actions that generate Activity Tracker events" caption-side="top"}
{: tab-title="Policies"}
{: tab-group="AT-events"}
{: class="comparison-tab-table"}

| Action                   | Description              |
| ------------------------ | ------------------------ |
| `kms.importtoken.create` | Create an import token   |
| `kms.importtoken.read`   | Retrieve an import token |
{: #table-3}
{: caption="Table 3. {{site.data.keyword.keymanagementserviceshort}} actions that generate Activity Tracker events" caption-side="top"}
{: tab-title="Import tokens"}
{: tab-group="AT-events"}
{: class="comparison-tab-table"}

| Action                                  | Description                                              |
| --------------------------------------- | -------------------------------------------------------- |
| `kms.registrations.create`[^services-1] | Create a registration between a key and a cloud resource |
| `kms.registrations.listforkey`          | List registrations for a key                             |
| `kms.registrations.list`                | List registrations for any key                           |
| `kms.registrations.merge`[^services-2]  | Update a registration between a key and a cloud resource |
| `kms.registrations.write`[^services-3]  | Replace registration between a key and a cloud resource  |
| `kms.registrations.delete`[^services-4] | Delete registration between a key and a cloud resource   |
{: #table-4}
{: caption="Table 4. {{site.data.keyword.keymanagementserviceshort}} actions that generate Activity Tracker events" caption-side="top"}
{: tab-title="Registrations"}
{: tab-group="AT-events"}
{: class="comparison-tab-table"}

[^services-1]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-2]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-3]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

[^services-4]: This action is performed on your behalf by an [integrated service](/docs/key-protect?topic=key-protect-integrate-services) that has enabled support for key registration. [Learn more](/docs/key-protect?topic=key-protect-view-protected-resources)

## Viewing events
{: #at-ui}

<!-- TODO: clarification
  Option 1:
  As in the previous section, there are multiple options. Choose the one that
  best suits your service, and delete the other ones.

  Option 2:
  Location based service: A location-based service generates events in the same
  location where the service instance is provisioned. For example, Certificate Manager.
-->

Events that are generated by an instance of {{site.data.keyword.keymanagementserviceshort}}
are automatically forwarded to the {{site.data.keyword.at_full_notm}} service
instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To
view events, you must access the web UI of the {{site.data.keyword.at_full_notm}}
service in the same location where your service instance is available.

For more information, see
[Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

## Analyzing events
{: #at-events-analyze}

<!-- TODO: clarification
  Provide information about the events in your service that add additional
  information in requestData and responseData. See the IAM Events topic for a
  sample topic that includes this section:
  https://cloud.ibm.com/docs/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.
-->

Due to the sensitivity of the information for an encryption key, when an event
is generated as a result of an API call to the {{site.data.keyword.keymanagementserviceshort}}
service, the event that is generated does not include detailed information about
the key.

The event includes a correlation ID that you can use to identify the
key internally in your cloud environment. The correlation ID is a field that is
returned as part of the `responseBody.content` field.

You can use this
information to correlate the {{site.data.keyword.keymanagementserviceshort}} key
with the information of the action that is reported through the
{{site.data.keyword.cloudaccesstrailshort}} event.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloudaccesstrailshort}} events
{: #at-events}

Use the {{site.data.keyword.cloudaccesstrailfull}} service to track how users and applications interact with {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

The {{site.data.keyword.cloudaccesstrailfull_notm}} service records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. 

For more information, see the [{{site.data.keyword.cloudaccesstrailshort}} documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla){: new_window}.

## List of events
{: #events}

The following table lists the actions that generate an event:

<table>
    <tr>
        <th>Action</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>kms.secrets.create</td>
        <td>Create a key</td>
    </tr>
    <tr>
        <td>kms.secrets.read</td>
        <td>Retrieve a key by ID</td>
    </tr>
   <tr>
        <td>kms.secrets.delete</td>
        <td>Delete a key by ID</td>
    </tr>
    <tr>
        <td>kms.secrets.list</td>
        <td>Retrieve a list of keys</td>
    </tr>
    <tr>
        <td>kms.secrets.head</td>
        <td>Retrieve the number of keys</td>
    </tr>
     <tr>
        <td>kms.secrets.wrap</td>
        <td>Wrap a key</td>
    </tr>
     <tr>
        <td>kms.secrets.unwrap</td>
        <td>Unwrap a key</td>
    </tr>
     <tr>
        <td>kms.secrets.rotate</td>
        <td>Rotate a key</td>
    </tr>
    <caption style="caption-side:bottom;">Table 1. Actions that generate {{site.data.keyword.cloudaccesstrailfull_notm}} events</caption>
</table>

## Where to view the events
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}} events are available in the {{site.data.keyword.cloudaccesstrailshort}} **account domain** that is available in the {{site.data.keyword.cloud_notm}} region where the events are generated.

For example, when you create, import, delete, or read a key in {{site.data.keyword.keymanagementserviceshort}}, an {{site.data.keyword.cloudaccesstrailshort}} event is generated. These events are automatically forwarded to the {{site.data.keyword.cloudaccesstrailshort}} service in the same region where the {{site.data.keyword.keymanagementserviceshort}} service is provisioned.

To monitor the API activity, you must provision the {{site.data.keyword.cloudaccesstrailshort}} service in a space that is available in the same region where the {{site.data.keyword.keymanagementserviceshort}} service is provisioned. Then, you can view events through the account view in the {{site.data.keyword.cloudaccesstrailshort}} UI if you have a lite plan, and through Kibana if you have a premium plan.

## Additional information
{: #info}

Due to the sensitivity of the information for an encryption key, when an event is generated as a result of an API call to the {{site.data.keyword.keymanagementserviceshort}} service, the event that is generated does not include detailed information about the key. The event includes a correlation ID that you can use to identify the key internally in your cloud environment. The correlation ID is a field that is returned as part of the `responseHeader.content` field. You can use this information to correlate the {{site.data.keyword.keymanagementserviceshort}} key with the information of the action reported through the {{site.data.keyword.cloudaccesstrailshort}} event.
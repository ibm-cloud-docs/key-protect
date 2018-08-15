---

copyright:
  years: 2017, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Troubleshooting
{: #troubleshooting}

General problems with using {{site.data.keyword.keymanagementservicefull}} might include providing the correct headers or credentials when you interact with the API. In many cases, you can recover from these problems by following a few easy steps.
{: shortdesc}

## Unable to access the user interface
{: #unable-to-access-ui}

When you access the {{site.data.keyword.keymanagementserviceshort}} user interface, the UI does not load as expected.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You see the following error: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

On 15 December 2017, we added new features, such as [envelope encryption](/docs/services/key-protect/concepts/envelope-encryption.html), to the {{site.data.keyword.keymanagementserviceshort}} service. You can now provision the {{site.data.keyword.keymanagementserviceshort}} service globally, without needing to specify a Cloud Foundry organization and space.
{: tsCauses}

These changes affected the user interface for older instances of the service. If you created your instance of {{site.data.keyword.keymanagementserviceshort}} before 28 September 2017, the user interface might not work as expected.

We're working to fix this issue on our end. As a temporary solution, you can continue to manage your keys by using the {{site.data.keyword.keymanagementserviceshort}} API.
{: tsResolve}

You can use the legacy `https://ibm-key-protect.edge.bluemix.net` endpoint to interact with the {{site.data.keyword.keymanagementserviceshort}} service.

**Example request**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Unable to create or delete keys
{: #unable-to-create-keys}

When you access the {{site.data.keyword.keymanagementserviceshort}} user interface, you do not see the options to add or delete keys.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You can see a list of keys, but you do not see options to add or delete keys. 

You do not have the correct authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions.
{: tsCauses} 

Verify with your administrator that you are assigned the correct role in the applicable resource group or service instance. For more information about roles, see [Roles and permissions](/docs/services/key-protect/manage-access.html#roles).
{: tsResolve}

## Getting help and support
{: #getting-help}

If you have problems or questions when you are using {{site.data.keyword.keymanagementserviceshort}}, you can check {{site.data.keyword.cloud_notm}}, or get help by searching for information or by asking questions through a forum. You can also open a support ticket.
{: shortdesc}

You can check whether {{site.data.keyword.cloud_notm}} is available by going to the [status page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/status?tags=platform,runtimes,services).

You can review the forums to see whether other users ran into the same problem. When you are using the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.

- If you have technical questions about {{site.data.keyword.keymanagementserviceshort}}, post your question on [Stack Overflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} and tag your question with "ibm-cloud" and "key-protect".
- For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} forum. Include the "ibm-cloud"
and "key-protect" tags.

See [Getting help ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window} for more details about using the forums.

For more information about opening an {{site.data.keyword.IBM_notm}} support ticket, or about support levels and ticket severities, see [Contacting support ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.

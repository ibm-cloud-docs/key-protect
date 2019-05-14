---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-14"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

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
{:note: .note}
{:important: .important}

# Troubleshooting
{: #troubleshooting}

General problems with using {{site.data.keyword.keymanagementservicefull}} might include providing the correct headers or credentials when you interact with the API. In many cases, you can recover from these problems by following a few easy steps.
{: shortdesc}

## Unable to create or delete keys
{: #unable-to-create-keys}

When you access the {{site.data.keyword.keymanagementserviceshort}} user interface, you do not see the options to add or delete keys.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You can see a list of keys, but you do not see options to add or delete keys. 

You do not have the correct authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions.
{: tsCauses} 

Verify with your administrator that you are assigned the correct role in the applicable resource group or service instance. For more information about roles, see [Roles and permissions](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Getting help and support
{: #getting-help}

If you have problems or questions when you are using {{site.data.keyword.keymanagementserviceshort}}, you can check {{site.data.keyword.cloud_notm}}, or get help by searching for information or by asking questions through a forum. You can also open a support ticket.
{: shortdesc}

You can check whether {{site.data.keyword.cloud_notm}} is available by going to the [status page ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/status?tags=platform,runtimes,services).

You can review the forums to see whether other users ran into the same problem. When you are using the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.

- If you have technical questions about {{site.data.keyword.keymanagementserviceshort}}, post your question on [Stack Overflow ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} and tag your question with "ibm-cloud" and "key-protect".
- For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/key-protect/){: new_window} forum. Include the "ibm-cloud"
and "key-protect" tags.

See [Getting support ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window} for more details about using the forums.

For more information about opening an {{site.data.keyword.IBM_notm}} support ticket, or about support levels and ticket severities, see [Contacting support ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.

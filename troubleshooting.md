---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-16"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

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

You can check whether {{site.data.keyword.cloud_notm}} is available by going to the [status page](https://{DomainName}/status?tags=platform,runtimes,services){: external}.

You can review the forums to see whether other users ran into the same problem. When you are using the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.

- If you have technical questions about {{site.data.keyword.keymanagementserviceshort}}, post your question on [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} and tag your question with "ibm-cloud" and "key-protect".
- For questions about the service and getting started instructions, use the [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external} forum. Include the "ibm-cloud"
and "key-protect" tags.

See [Getting support](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external} for more details about using the forums.

For more information about opening an {{site.data.keyword.IBM_notm}} support ticket, or about support levels and ticket severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support){: external}.

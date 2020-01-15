---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-15"

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
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:term: .term}

# Troubleshooting
{: #troubleshooting}

General problems with using {{site.data.keyword.keymanagementservicefull}} might include providing the correct headers or credentials when you interact with the API. In many cases, you can recover from these problems by following a few easy steps.
{: shortdesc}

## Unable to create or delete keys
{: #unable-to-create-keys}
{: troubleshoot}

When you access the {{site.data.keyword.keymanagementserviceshort}} user interface, you do not see the options to add or delete keys.

From the {{site.data.keyword.cloud_notm}} dashboard, you select your instance of the {{site.data.keyword.keymanagementserviceshort}} service.
{: tsSymptoms}

You can see a list of keys, but you do not see options to add or delete keys. 

You do not have the correct authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions.
{: tsCauses} 

Verify with an administrator that you are assigned the correct role in the applicable service instance. For more information about roles, see [Roles and permissions](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Unable to authenticate through the API
{: #unable-to-authenticate-api}
{: troubleshoot}

When you call the {{site.data.keyword.keymanagementserviceshort}} API, the system returns a `401 Unauthorized` error, and you're unable to make the API request.

You call any {{site.data.keyword.keymanagementserviceshort}} API method. You see an error response similar to the following JSON object:
{: tsSymptoms}

```
{ 
  "metadata":
  {
    "collectionType":"application/vnd.ibm.kms.error+json",
    "collectionTotal":1
  },
  "resources":[
    {
      "errorMsg":"Unauthorized: The user does not have access to the specified resource"
    }
  ]
}
```

You do not have the correct authorization to perform {{site.data.keyword.keymanagementserviceshort}} actions in the specified service instance.
{: tsCauses} 

Verify with an administrator that you are assigned the correct platform and service access roles in the applicable service instance. For more information about roles, see [Roles and permissions](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Unable to view or list keys
{: #unable-to-list-keys-api}
{: troubleshoot}

When you try to list keys by using the {{site.data.keyword.keymanagementserviceshort}} API, you're unable to view any keys in a service instance that you have access to.

You call `GET api/v2/keys` to list the keys that are available in your service instance. The system returns a response similar to the following JSON object:
{: tsSymptoms}

```
{
    "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 0
    }
}
```

You do not have the correct authorization to view the requested range of keys.
{: tsCauses}

Contact an administrator to check your permissions. If the service instance contains keys that you're unable to view, verify that you're assigned the applicable [level of access to keys](/docs/services/key-protect?topic=key-protect-manage-access-key) in the service instance. If the service instance contains more than 200 keys, you need to use the [`offset` and `limit` parameters](/docs/services/key-protect?topic=key-protect-view-keys#retrieve-subset-keys-api) to list another subset of keys. 
{: tsResolve}

For example, if you want to list keys 201 - 210 that are available in a service instance, you use `../keys?offset=200&limit=10` to skip the first 200 keys.

## Unable to view or list specific keys
{: #unable-to-list-specific-keys}
{: troubleshoot}

When you call the {{site.data.keyword.keymanagementserviceshort}} API, you're unable to list specific keys that you have access to.

You call `GET api/v2/keys` to list the keys that are available in your service instance.
{: tsSymptoms}

You can see a list of keys, but you can't find a specific key that's stored in the instance. You verify with your administrator that you're assigned the applicable [level of access to the keys](/docs/services/key-protect?topic=key-protect-manage-access-key) that you're unable to view. You also verify with your admin that the key belongs to the service instance that you're targeting.

The service instance contains a large number of keys, and the specific keys that you're looking for aren't returned by default when you call `GET api/v2/keys` to list keys.
{: tsCauses}

Check with an admin to understand the total number of keys that are stored in the instance. By default, `GET api/v2/keys` returns the first 200 keys. If the service instance contains more than 200 keys, you need to use the [`offset` and `limit` parameters](/docs/services/key-protect?topic=key-protect-view-keys#retrieve-subset-keys-api) to list another subset of keys. 
{: tsResolve}

For example, if you want to list keys 201 - 210 that are available in a service instance, you use `../keys?offset=200&limit=10` to skip the first 200 keys.

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

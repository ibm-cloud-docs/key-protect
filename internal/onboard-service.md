---

copyright:
  years: 2019, 2020
lastupdated: "2020-09-10"

keywords: enable BYOK, onboard to Key Protect, Key Protect onboarding, internal, KYOK, BYOK

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

# Onboard your service
{: #onboard-service}

You can enable Bring Your Own Key (BYOK) for your service by integrating with
{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

## Before you begin
{: #onboard-prereqs}

Review our
[common adoption and usage patterns](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/architecture/adoption/){: external}.
Keep in mind the following list of prerequisites before you continue to step 1.

- You must have an {{site.data.keyword.cloud_notm}} account with access to both
    the
    [staging](https://test.cloud.ibm.com/){: external}
    and
    [production](https://cloud.ibm.com/){: external}
    environments.
- You must have a basic understanding of IAM concepts, such as
    [granting service to service access](/docs/get-coding?topic=get-coding-servicetoservice){: external}.

Need help? You can use the
[`#key-protect` Slack channel](https://slack.com/app_redirect?channel=C1UB18PQB){: external}
or
[raise a support ticket](https://github.ibm.com/kms/customer-issues/issues/new/choose){: external}.
For specific questions about your use case, reach out to Mala Anand (Architect)
or Terry Mosbaugh (Offering Manager).
{: tip}

## Step 1. Submit a request to onboard your service
{: #submit-request}

To submit an onboarding request, create a new issue in the
[{{site.data.keyword.keymanagementserviceshort}} GitHub repository](https://github.ibm.com/kms/customer-issues/issues/new/choose){: external}.

In the issue, provide the following details:

- The `service-name` that uniquely identifies your service.

- The {{site.data.keyword.keymanagementserviceshort}} service roles that we must
    assign for your service. These service roles will define the scope of access
    for your service when it accesses
    {{site.data.keyword.keymanagementserviceshort}} on the user's behalf.
    Review the
    [{{site.data.keyword.keymanagementserviceshort}} service roles and actions](https://pages.github.ibm.com/Alchemy-Key-Protect/architecture/architecture/iam-policies/){: external}
    to determine which roles match your use case. For Bring Your Own Key (BYOK)
    entablement, the **Reader** service role provides the necessary permissions
    for performing wrap, unwrap, and list key actions.

- The environment, staging or production, where you need to onboard your
    service. After you onboard in the staging environment, you must submit another
    request to onboard in production.

When you submit a request, the {{site.data.keyword.keymanagementserviceshort}}
team will update its service registrations to include access for your service.
Feel free to connect with us on the `#key-protect` Slack channel if you need
help during this process.

## Step 2. Create a CRN token
{: #retrieve-s2s-token}

To grant access between your service (the "source" service) and
{{site.data.keyword.keymanagementserviceshort}} (the "target" service), we
recommend using a CRN token. This process requires a single additional request
to IAM.

Do not propagate the end user's token through the system because this not good
security hygiene. Instead, use a CRN token from IAM.
{: important}

As you can see in the following table, the subsequent API calls take a CRN
token.

| API call | Target | Token Type |
| -------- | ------ | ---------- |
| [List KMS instances](#discover-kms-instances) | GHoST | CRN Token |
| [Get KMS endpoints](#discover-kms-instances) | GHoST | CRN Token |
| [Register protected resources](/docs/key-protect?topic=key-protect-register-protected-resources) | {{site.data.keyword.keymanagementserviceshort}} | CRN token |
| Wrap / unwrap DEKs | {{site.data.keyword.keymanagementserviceshort}} | CRN token |

See the
[#iam-adopters Slack channel](https://ibm-cloudplatform.slack.com/archives/C0NLB2W3B/p1516206027000901){: external}
or refer to
[IAM service to service documentation](/docs/get-coding?topic=get-coding-servicetoservice){: external}
for official steps and guidance.

## Step 3. Discover KMS instances
{: #discover-kms-instances}

Before you use the {{site.data.keyword.keymanagementserviceshort}} APIs to
protect customer data, you need to know how to perform two important actions:

- Gather a list of all {{site.data.keyword.keymanagementserviceshort}} instances
    that your customer has access to view, which is set by an IAM policy with
    Platform Viewer role

- Discover {{site.data.keyword.keymanagementserviceshort}} regional endpoints
    for each instance

Listing {{site.data.keyword.keymanagementserviceshort}} instances gives your
customers the choice of which root keys protect your service's resource and
which type of KMS provider. The instances that contain root keys may be in any
MZR - even different from the resource's location. We allow this so that
customers can centrally manage keys in a region that the customer desires, while
data is protected and stored elsewhere. Customers choose and own the
responsibility of the location of their keys.

Both list and discover actions can be achieved through the use of the platform
Tagging APIs (i.e GHoST) and a **CRN token** from earlier. Your team can search
for all KMS instances that are associated with a given user by specifying
`sub_type` instead of service name. This is done by querying Global Search and
Tagging (GhoST).

```sh
$ curl -X POST \
    "https://api.global-search-tagging.cloud.ibm.com/v2/resources/search?limit=50&account_id=<account_ID>" \
    -H "authorization: Bearer <IAM_token>" \
    -H "content-type: application/json" \
    -d '{
            "query": "(type:resource-instance AND doc.sub_type:kms)"
        }'
```
{: codeblock}

Although you can create your {{site.data.keyword.keymanagementserviceshort}}
instances by using a user token, and then use the same token to run your
query in GhoST, we recommend that you use your CRN token, which exchanges your
customer's token with one that represents your service.
{: tip}

The following JSON output shows an example
{{site.data.keyword.keymanagementserviceshort}} instance. From the response,
adopting teams can see which endpoint hosts a particular KMS instance.

**Note:** the JSON output contains more content that what is shown here.

```json
{
    "doc": {
        "id": "crn:v1:bluemix:public:hscrypto:us-south:a/80e35ac1582a2b1a7b633e6107f9295a:67be47c6-cac0-415d-b298-0e6d45d6cb51::",
        "sub_type": "kms",
        "extensions": {
            "endpoints": {
                "public": "<public endpoint for the instance>",
                "private": "<optional - private network endpoint for the instance>"
            }
        }
    }
}
```
{: codeblock}

Adopting services must not use the public endpoint as a redundant path in the
event the private endpoint has availability issues because of customer
expectation for security in motion. If you believe this is needed, please raise
it during you CISO baseline review.
{: important}

If your service receives both a public and private endpoint in the response,
favor the private endpoints over public endpoints to enable security by default.
To use the private endpoints, your service needs to be
[VRF enabled](/docs/dl?topic=dl-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud){: external}.

After the endpoints are known, you can store the _url:port_ information to be
used later, when you're ready to perform envelope encryption on your data
encryption keys.
{: tip}

Because your instances are known, you can use the
[{{site.data.keyword.keymanagementserviceshort}} API](/apidocs/key-protect#list-keys){: external}
to reveal which keys can be used to secure your DEKs. Before you can do that, a
root key needs to be created.

## Next steps
{: #onboard-next-steps}

Now that you have an instance and can discover it, let's do something useful!

- [Create a test instance and set up the APIs](/docs/key-protect?topic=key-protect-configure-api)



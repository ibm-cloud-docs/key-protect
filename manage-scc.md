---

copyright:
  years: 2020
lastupdated: "2020-12-10"

keywords: Managing security and compliance with Key Protect, security for Key Protect, compliance for Key Protect, security, compliance

subcollection: key-protect

---

{:external: target="_blank" .external}
{:note: .note}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:term: .term}

# Managing security and compliance with {{site.data.keyword.keymanagementserviceshort}}
{: #manage-security-compliance}

{{site.data.keyword.keymanagementservicefull}} is integrated with the
{{site.data.keyword.compliance_short}} to help you manage security and
compliance for your organization.
{: shortdesc}

With the {{site.data.keyword.compliance_short}}, you can:

* Monitor for controls and goals that pertain to
  {{site.data.keyword.keymanagementserviceshort}}.

* Define rules for {{site.data.keyword.keymanagementserviceshort}} that can help
  to standardize resource configuration.

## Monitoring security and compliance posture with {{site.data.keyword.keymanagementserviceshort}}
{: #monitor-kp}

As a security or compliance focal, you can use the
{{site.data.keyword.keymanagementserviceshort}}
[goals](#x2117978){: term}
to help ensure that your organization is adhering to the external and internal
standards for your industry. By using the {{site.data.keyword.compliance_short}}
to validate the resource configurations in your account against a
[profile](#x2034950){: term}, you can identify potential security or compliance
issues as they arise. Note that all of the goals for
{{site.data.keyword.keymanagementserviceshort}} are added to the
{{site.data.keyword.cloud_notm}} Best Practices Controls 1.0 profile but can
also be mapped to other profiles.

To begin monitoring your {{site.data.keyword.keymanagementserviceshort}}
resources, at least one configuration rule as outlined in the
[Governing {{site.data.keyword.keymanagementserviceshort}} resource configuration](#govern-kp)
section is required. Check out
[Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic-security-compliance-getting-started){: external}
for more information.

All of the goals for Certificate Manager are added to the IBM Cloud Best Practices 
Controls 1.0 profile but can also be mapped to other profiles.
{: note}

### Available goals for {{site.data.keyword.compliance_short}}
{: #kp-available-goals}

* Ensure automated rotation for keys is enabled. (This lifecycle applies to
  {{site.data.keyword.keymanagementserviceshort}} generated keys only)
* Ensure the key management service has high availability

## Governing {{site.data.keyword.keymanagementserviceshort}} resource configuration with config rules
{: #govern-kp}

As a security or compliance focal, you can use the
{{site.data.keyword.compliance_short}} to
[define configuration rules](/docs/security-compliance?topic=security-compliance-rules){: external}
for the {{site.data.keyword.keymanagementserviceshort}} instances that you
create.

This service only supports the ability to view the results of your configuration
scans in the Security and Compliance Center. It is not necessary to set up a
collector to use configuration rules.
{: note}

[Config rules](#x3084914){: term}
are used to monitor and optionally enforce the configuration standards that you
want to implement across your accounts. To learn more about the about the
available properties that you can use to create a rule for
{{site.data.keyword.keymanagementserviceshort}}, review the following table. Note 
that the 

| Resource Kind | Property Name | Operator | Value | Description |
|-|-|-|-|-|
| `instance` | `allowed_network`| `string_equals` | public-and-private<br>private-only | Specifies the type of endpoint the {{site.data.keyword.keymanagementserviceshort}} instance can be accessed from. Refer to <br>[Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies) for more information. |
{: caption="Table 1. Config rule properties and target attributes for {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

To learn more about config rule capabilities, check out
[What is a config rule?](/docs/security-compliance?topic=security-compliance-what-is-rule){: external}.

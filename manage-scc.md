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

* Define rules for {{site.data.keyword.keymanagementserviceshort}} that can help
  to standardize resource configuration.

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
{{site.data.keyword.keymanagementserviceshort}}, review the following table.

| Resource Kind | Property Name | Operator | Value | Description |
| ------------- | ------------- | -------- | ----- | ----------- |
| `instance` | `allowed_network`| `string_equals` | public-and-private<br>private-only | Specifies the type of endpoint the {{site.data.keyword.keymanagementserviceshort}} instance can be accessed from. Refer to <br>[Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies) for more information. |
{: caption="Table 1. Config rule properties and target attributes for {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

To learn more about config rule capabilities, see
[What is a config rule?](/docs/security-compliance?topic=security-compliance-what-is-rule){: external}.

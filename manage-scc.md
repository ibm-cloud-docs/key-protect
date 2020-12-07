---

copyright:
  years: 2020
lastupdated: "2020-12-01"

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
issues as they arise.

All of the goals for {{site.data.keyword.keymanagementserviceshort}} are added
to the {{site.data.keyword.cloud_notm}} Best Practices Controls 1.0 profile but
can also be mapped to other profiles.
{: note}

To begin monitoring your {{site.data.keyword.keymanagementserviceshort}}
resources, at least one configuration rule as outlined in the
[Governing {{site.data.keyword.keymanagementserviceshort}} resource configuration](#govern-kp)
section is required. Check out
[Getting started with {{site.data.keyword.compliance_short}}](/docs/security-compliance?topic-security-compliance-getting-started){: external}
for more information.

This service only supports the ability to view the results of your configuration
scans in the Security and Compliance Center. It is not necessary to set up a
collector to use configuration rules.
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

[Config rules](#x3084914){: term}
are used to monitor and optionally enforce the configuration standards that you
want to implement across your accounts. To learn more about the about the
available properties that you can use to create a rule for
{{site.data.keyword.keymanagementserviceshort}}, review the following table.

| Category      | Description |
| ------------- | ----------- |
| Resource kind | instance |
| Property Name | allowed_network |
| Operator      | string_equals |
| Value         | <ul><li>public-and-private</li><li>private-only</li></ul> |
| Description   | Specifies the type of endpoint the {{site.data.keyword.keymanagementserviceshort}} instance can be accessed from. Refer to [Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies) for more information. |
{: caption="Table 1.  Config rule properties for {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

To apply the config rule to a specific
{{site.data.keyword.keymanagementserviceshort}} resource or set of
{{site.data.keyword.keymanagementserviceshort}} resources, the following
`target.additional_target_attributes` may be added to the rule.

| Category       | Description |
| -------------- | ----------- |
| Resource kind  | instance |
| Attribute Name | **location** |
| Value          | <ul><li>au-syd</li><li>eu-de</li><li>eu-gb</li><li>jp-tok</li><li>us-east</li><li>us-south</li></ul> |
| Description    | {{site.data.keyword.keymanagementserviceshort}} service locations. See [Regions and endpoints](/docs/key-protect?topic=key-protect-regions) for more information |
{: caption="Table 2. Config rule additional target attribute (location) for {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

| Category       | Description |
| -------------- | ----------- |
| Resource kind  | instance |
| Attribute Name | **resource_id** |
| Value          | a valid GUID |
| Description    | {{site.data.keyword.keymanagementserviceshort}} service instance GUID |
{: caption="Table 3. Config rule additional target attribute (resource_id) for {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

To learn more about config rule capabilities, check out
[What is a config rule?](/docs/security-compliance?topic=security-compliance-what-is-rule){: external}.

## Evaluating config rule results
{: #evaluate-kp-scc-results}

After
[rules are created and added to scopes](/docs/security-compliance?topic=security-compliance-rules){: external},
you can view the evaluation results in the
[{{site.data.keyword.compliance_short}}](/docs/security-compliance?topic-security-compliance-getting-started){: external}.
Each rule is shown to be compliant or noncompliant - if a rule shows as being
noncompliant then you can view the specific resources that is in violation of
the rule.

The evaluation results are only available for seven days. It is recommended that
reports are downloaded and organized to maintain a history of compliance for
audit purposes.

For more information on reporting results, see
[Viewing evaluation results](/docs/security-compliance?topic=security-compliance-results){: external}.

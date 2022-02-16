---

copyright:
  years: 2020, 2022
lastupdated: "2022-02-16"

keywords: managing security, managing compliance, best practices

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

As a security or compliance focal, you can use the {{site.data.keyword.compliance_short}} to [define configuration rules](/docs/security-compliance?topic=security-compliance-what-is-governance){: external} for the {{site.data.keyword.keymanagementserviceshort}} instances that you create.


This service only supports the ability to view the results of your configuration scans in the Security and Compliance Center. It is not necessary to set up a collector to use configuration rules.
{: note}

[Config rules](#x3084914){: term} are used to monitor and optionally enforce the configuration standards that you want to implement across your accounts.

To learn more about the available properties that you can use to create a rule for {{site.data.keyword.keymanagementserviceshort}}, check out [Instance resources](#govern-kp-instances) and [Key resources](#govern-kp-keys).

### Instance resources
{: #govern-kp-instances}

* `allowed_network`
    - Operator is `string_equals`.
    - Value is `public-and-private` or `private-only`.
    - Specifies the type of endpoint the {{site.data.keyword.keymanagementserviceshort}} instance can be accessed from. Refer to [Managing network access policies](/docs/key-protect?topic=key-protect-managing-network-access-policies) for more information.
* `dual_auth_delete`
    - Operator is either `is_true` or `is_false`.
    - Require/Disallow enablement of dual authorization to delete keys in the {{site.data.keyword.keymanagementserviceshort}} instance. Requirement applies to subsequently created keys and will not apply to pre-existing keys. Refer to [Managing dual authorization](/docs/key-protect?topic=key-protect-manage-dual-auth) for more information.
* `key_create_and_import.create_root_key`
    - Operator is either `is_true` or `is_false`.
    - Allow/Disallow root keys to be created in the {{site.data.keyword.keymanagementserviceshort}} instance. Refer to [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess) for more information.
* `key_create_and_import.import_root_key`
    - Operator is either `is_true` or `is_false`.
    - Allow/Disallow root keys to be imported into the {{site.data.keyword.keymanagementserviceshort}} instance. Refer to [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess) for more information.
* `key_create_and_import.create_standard_key`
    - Operator is either `is_true` or `is_false`.
    - Allow/Disallow standard keys to be created in the {{site.data.keyword.keymanagementserviceshort}} instance. Refer to [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess) for more information.
* `key_create_and_import.import_standard_key`
    - Operator is either `is_true` or `is_false`.
    - Allow/Disallow standard keys to be imported into the {{site.data.keyword.keymanagementserviceshort}} instance. Refer to [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess) for more information.
* `key_create_and_import.enforce_token`
    - Operator is either `is_true` or `is_false`.
    - Restrict/Allow the import of key material into the {{site.data.keyword.keymanagementserviceshort}} instance without using an import token. Refer to [Managing a key create and import access policy](/docs/key-protect?topic=key-protect-manage-keyCreateImportAccess) for more information.
* `metrics`
    - Operator is either `is_true` or `is_false`.
    - Restrict {{site.data.keyword.keymanagementserviceshort}} instance metrics to be forwarded to instance owner's {{site.data.keyword.monitoringfull}}. Refer to [Managing metrics](/docs/key-protect?topic=key-protect-manage-monitor-metrics) for more information.

### Key resources
{: #govern-kp-keys}

* `dual_auth_delete`
    - Operator is either `is_true` or `is_false`.
    - Require/Disallow dual authorization to delete the given key in the {{site.data.keyword.keymanagementserviceshort}} instance. Refer to [Setting dual authorization policies for keys](/docs/key-protect?topic=key-protect-set-dual-auth-key-policy) for more information.
* `rotation.enabled`
    - Operator is either `is_true` or `is_false`.
    - Require/Disallow active rotation policy on specified key(s). Refer to [Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy) for more information.
* `rotation.interval_month`
    - Operator is either `num_equals`, `num_not_equals`, `num_less_than`, `num_less_than_equals`, `num_greater_than` or `num_greater_than_equals`.
    - Value is 1 ≤ Value ≤ 12
    - Specifies the given key's rotation interval (in months). Automatic rotation policies can only be applied to root keys with non-imported material. Refer to [Setting a rotation policy](/docs/key-protect?topic=key-protect-set-rotation-policy) for more information.

To learn more about constructing **config rules**, see
 [Formatting rules and templates](/docs/security-compliance?topic=security-compliance-formatting-rules-templates).



---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-04"

keywords: automatic key rotation, set rotation policy, retrieve key policy

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
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}

# Setting a rotation policy
{: #set-rotation-policy}

You can set an automatic rotation policy for a root key either by setting an instance policy on your {{site.data.keyword.keymanagementservicefull}} instance or, if you have the correct role, by setting the policy at key creation time.
{: shortdesc}

Recall from [Rotating your root keys](/docs/key-protect?topic=key-protect-key-rotation) that rotating root keys regularly shortens the cryptoperiod of a key and therefore can be used as a general security policy as well as in specific cases such as personnel turnover, process malfunctions, or the detection of a security issue.

If you do not have `Manager` level of permission necessary to set a rotation policy, you can still manually rotate your keys any time as long as you have at least the `Writer` level of permission. Check out [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys) for more information about this process. Also, check out [Understanding user roles and resources](/docs/key-protect?topic=key-protect-manage-access) for more information about the manager, writer, and other roles regarding access to resources like keys, instances, and accounts.
{: tip}

There are a few different ways to set a rotation policy:

* [Set a rotation policy on your {{site.data.keyword.keymanagementservicefull}} instance](#manage-policies-instance). This ensures that every key that is created has the rotation policy unless the policy is overridden at key-creation time by a `Manager`.
* [Set a rotation policy at key creation time](#manage-policies-creation-time). This allows for the adjustment of the rotation policy for a particular key to be applied during the creation of a key.
* [Set a rotation policy after the key has been created](#manage-policies-gui). A rotation policy can be changed on a key at any time, even if a key is carrying a disabled rotation policy or if one was never applied to the key at all.

You can create a rotation policy only for root keys that are generated in {{site.data.keyword.keymanagementserviceshort}}. If you imported the root key initially, you must provide new base64 encoded key material to rotate the key. For more information, see [Manually rotating keys](/docs/key-protect?topic=key-protect-rotate-keys).
{: important}

## Setting a rotation policy on your instance
{: #manage-policies-instance}

If a rotation policy is set on your instance, every root key that is created in that instance carries that policy (unless it is overridden at key-creation time). This is particularly useful in cases where users with the `Writer` role (who cannot by default assign a rotation policy) create most of the keys on an instance but it is desired that root keys carry a rotation policy. Instance policies allow a user with the `Manager` role to set a policy which is then applied to all of the keys created by `Writers` or other `Managers`, unless those `Managers` overwrite the policy.

### Setting or editing a rotation policy on your instance using the console
{: #manage-policies-instance-ui}
{: ui}

To set an instance policy, navigate to the **Instance policies** page in the left navigation and locate the **Key rotation policy** card. If no policy has been set, the button should be **Disabled**. To set a policy:

1. Click the button to enable the policy.
2. Then, set a rotation interval. Note that you can only set an automatic rotation policy in intervals of 30 days (or one month). If you set three months, for example, the key is rotated every 90 days. If a rotation policy has already been enabled for this instance, simply edit the value to the desired number of months.
3. Click **Save**.

Your key policy is now set. If no other action is taken, every root key created in {{site.data.keyword.keymanagementserviceshort}} is issued this policy.

If you only need to edit the policy for a single key, and you are a `Manager`, the best policy is to change the rotation policy of the key at key creation time.

## Setting a rotation policy at key creation time
{: #manage-policies-creation-time}

If you are a `Manager` (or hold an equivalent level of permissions) it is also possible to set the rotation policy for a key during the process of creating a key. Note that it is possible to set a policy at key-creation time whether an instance-level rotation policy exists or not.

### Setting a rotation policy at key creation time using the console
{: #manage-policies-creation-time-ui}

As shown in [Creating root keys in the console](/docs/key-protect?topic=key-protect-create-root-keys#create-root-key-gui), to create a key, navigate to the **Keys** page in your instance and click **Add** to open the key creation side panel. Then, open the **Advanced options** tab to reveal the **Key alias**, **Key ring**, and **Rotation policy** options.

If you do not see the **Rotation policy** option, check your level of permissions to ensure you are a `Manager`.
{: tip}

If an instance-level rotation policy exists, the rotation policy button shows as **Enabled**. The rotation interval enabled on the instance (in months) is visible. If this rotation interval is appropriate for this key, not other action needs to be taken other than to click **Create** to create the key. If you want to change the rotation interval, click **Edit** and set the interval you want. Then click **Save**. Then you can click **Create** to create the key.

## Set a rotation policy after the key has been created
{: #manage-policies-gui}
{: ui}

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** > **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the application details page, use the **Keys** table to browse the keys in your service. If you have many keys, you can narrow your search by using the search bars to only search for enabled keys (since other kinds of keys cannot be rotated), keys in a particular key ring, and keys with a particular alias.

5. Once the key has been found, click the ⋯ icon to open a list of options for the key that you want to rotate.

6. From the options menu, click **Rotate** to open the **Rotation** side panel.

7. If your rotation policy is **Enabled**, you can edit this policy by changing the number of months selected. This will set the 30-day interval for your root key. If a key is set to be rotated every `2` months, for example, it will be rotated every 60 days, regardless of the number of days in a particular month. If your rotation policy is **Disabled**, and the key was created at a time when your instance had a rotation policy, a rotation interval number can be seen. This is the rotation policy that was written to your key in a **Disabled** state at key creation time. You can also change the rotation interval at this time.

8. Click **Set policy**. The policy is now in effect.

If you want to rotate the key immediately, click **Rotate key**. Note: these actions are not mutually exclusive. If your key has an existing rotation policy, the interface displays the key's existing rotation period.

**For imported root keys only**, you must add base64 encoded key material that you want to store and manage in the service. Ensure that the key material is in 128, 192, or 256 bits and that the bytes of data (for example, 32 bytes for 256 bits) are encoded by using base64 encoding.
{: important}

When it's time to rotate the key based on the rotation interval that you specify, {{site.data.keyword.keymanagementserviceshort}} automatically replaces the root key with new key material.





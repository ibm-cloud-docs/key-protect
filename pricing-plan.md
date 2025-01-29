---

copyright:
  years: 2025
lastupdated: "2025-01-29"

keywords: pricing plan, billing, cost

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
{:preview: .preview}
{:term: .term}
{:ui: .ph data-hd-interface='ui'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}

# Pricing for Key Protect on IBM Cloud
{: #pricing-plan}

Pricing in {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}} is based on the number of **key versions** in your account and whether you choose a plan that features enhanced cross regional disaster recovery. For the pricing rates, check out [{{site.data.keyword.keymanagementserviceshort}} in the {{site.data.keyword.cloud_notm}} catalog](/catalog/services/key-protect). There you can see the two plans offered for {{site.data.keyword.keymanagementserviceshort}} on {{site.data.keyword.cloud_notm}}, the **Cross-region resiliency** plan offering enhanced cross-regional disaster recovery, and the **Standard** plan, which is identical except that it does not offer cross-regional disaster recovery.
{: shortdesc}

As of 1 January 2025, five key versions per account are no longer free. You are charged for each key version, starting with the first created key.
{: important}

**What is a key version?**

Creating or importing a key initiates the first **version** of that key. As a result, a newly created or imported key has one version. Each time a key is rotated, a new version is created. A key that has been rotated 10 times, then, has 11 versions (10 versions created by rotations plus the initial version). To discover how many versions you have of each non-deleted key, check out [How many key versions do you have?](#pricing-plan-how-many-keys). For more information about rotating keys, check out [Rotating your root keys](/docs/key-protect?topic=key-protect-key-rotation). Note that only root keys can be rotated.

{{site.data.keyword.keymanagementserviceshort}} charges for all keys that are not in the `Destroyed` state and those that are in the `Destroyed` state but [can be restored](/docs/key-protect?topic=key-protect-restore-keys&interface=ui) (possible for 30 days after being deleted, an action that moves a key into the `Destroyed` state). If for example a key is created and then immediately deleted, you are charged for the key for 30 days (assuming it is not [purged](/docs/key-protect?topic=key-protect-delete-purge-keys&interface=ui) sooner than that). If those 30 days carry over into the following month, you are charged a prorated rate in each month based on the number of days out of 30 that fell in each month. For example, if there are 10 days left in a month when you create and then delete a key, you are charged a prorated monthly rate for those 10 days and then a prorated rate for 20 days in the following month.

`Deactivated` and `Suspended` keys **have not been deleted**, and still therefore count as non-deleted versions. For more information about key states, check out [Key states and transitions](/docs/key-protect?topic=key-protect-key-states#key-transitions).

**What is enhanced cross-region disaster recovery?**

There are three regions, `us-south` (located in Dallas, United States), `jp-tok` (located in Tokyo, Japan), and `eu-de` (located in Frankfurt, Germany) where {{site.data.keyword.keymanagementserviceshort}} has failover support into a separate region where data is replicated into standby infrastructure. The `us-south` region has data replicated into the `us-east` region (located in Washington DC, United States), the `jp-tok` region has data replicated into the `jp-osa` region (located in Osaka, Japan), and the `eu-de` region has data replicated into a datacenter in the Paris region. This means that if service in `us-south`, `jp-tok`, or `eu-de` is disrupted, requests to {{site.data.keyword.keymanagementserviceshort}} in those regions are routed to the region where the data has already been replicated. Note that the standby infrastructure in the region with failover support is completely separate from the {{site.data.keyword.keymanagementserviceshort}} service available in that region, and that failover only goes in one direction. Your data in `us-east`, for example, is not currently replicated to `us-south`.

This enhanced cross-region disaster recovery is only available to instances provisioned under (or [switched to](#pricing-plan-switch-ui)) the Cross-region resiliency pricing plan. You cannot switch to the Cross-region resiliency plan during a disaster to take advantage of its capabilities. You must have the plan already.

The Cross-region resiliency plan includes a non-prorated monthly charge **per region** (as long at least one key has been created in an instance in the region). This regional charge is the same regardless of the number of instances you have in a region and only applies to the Cross-region resliency plan; you are charged the same for 100 instances in a region as you would be for one. This plan also charges double the key version price for each key version.
{: tip}

## Pricing scenarios
{: #pricing-plan-scenarios}

* **Scenario:** You have one non-deleted key that has been rotated 19 times, which means it has 20 total versions.
    - **Pricing:** You are charged for 20 versions per month (your charge in the first month is prorated).

* **Scenario:** You have one non-deleted key with 20 total versions, another key with three versions, and a key in the `Suspended` state with two versions.
    - **Pricing:** Your are charged for 25 versions per month.

* **Scenario:** You have two instances in a single cross region with a total of 10 key versions.
    - **Pricing:** You are charged 1x the regional price and for 10 key versions (charged at double the rate of a single version in the Key versions plan).

* **Scenario:** You have two instances in two cross regions with a total of 10 key versions.
    - **Pricing:** You are charged 2x the regional price and for 10 key versions (charged at double the rate of a single version in the Standard plan).

* **Scenario:** You create a key in the middle of a month.
    - **Pricing:** Key versions are charged based on a proration by the number of days (out of 31) it exists in the monthly billing period. A key that only existed for one day, for example, is charged for 1/31 of the monthly price. A key created in with 15 days left in a month, then, is charged for 15 of the 31 days, (0.5 versions per month, if looking at your billing page).

## How many key versions do you have?
{: #pricing-plan-how-many-keys}

[Key management interoperability protocol (KMIP) symmetric keys](/docs/key-protect?topic=key-protect-kmip#kmip-adapter-view&interface=ui) are counted as a single key version and count toward your overall number of versions. Your KMIP symmetric keys can be located in the **KMIP** tab in the console or [through the API](/docs/key-protect?topic=key-protect-kmip#kmip-adapter-api-view-delete-objects&interface=api).
{: note}

To see how many versions you have of each key you have deployed:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Keys** panel, click the ⋯ icon and select **Key details**. This opens a side panel that shows, among other things, the number of versions of this key you have.

5. Repeat this process for every key in your instance. Note that because only root keys can be rotated, all of your standard keys only have one version.

## Switching to a different plan in the console
{: #pricing-plan-switch-ui}

Users who want enhanced cross regional resiliency must either create a new instance using the new plan, which was introduced on 1 January 2025, or switch an existing instance to the new plan.

This can be done in the console by following these steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the left navigation, select the **Plans** page.

5. To change your pricing plan, select the desired plan and click **Save**. Your pricing plan has now been changed.

## Switching a pricing plan using the API or CLI
{: #pricing-plan-switch-api-cli}

For more information on how to switch pricing plans using the API or CLI, check out [Updating your pricing plan](/docs/account?topic=account-changing&interface=cli).

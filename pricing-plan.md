---

copyright:
  years: 2024
lastupdated: "2024-12-17"

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
{: ui}
{: api}
{: cli}

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

This enhanced cross-region disaster recovery is only available to instances provisioned under (or [migrated to](#pricing-plan-migrate)) the Cross-region resiliency pricing plan. Note that this plan includes a non-prorated monthly charge **per region** (as long at least one key has been created in an instance in the region). This regional charge is the same regardless of the number of instances you have in a region and only applies to the Cross-region resliency plan; you are charged the same for 100 instances in a region as you would be for one. This plan also charges double the key version price for each key version. Note that you cannot switch plans during a disaster event.
{: tip}

## Pricing scenarios
{: #pricing-plan-scenarios}

### For the Standard plan
{: #pricing-plan-scenarios-version}

* **Scenario:** You have one non-deleted key that has been rotated 19 times, which means it has 20 total versions.
  - **Pricing:** You are charged for 20 versions per month (your charge in the first month is prorated).

* **Scenario:** You have one non-deleted key with 20 total versions, another key with three versions, and a key in the `Suspended` state with two versions.
  - **Pricing:** Your are charged for 25 versions per month.

### For Cross-region resiliency plan
{: #pricing-plan-scenarios-cross-region}

* **Scenario:** You have two instances in a single cross region with a total of 10 key versions.
  - **Pricing:** You are charged 1x the regional price and for 10 key versions (charged at double the rate of a single version in the Key versions plan).

* **Scenario:** You have two instances in two cross regions with a total of 10 key versions.
  - **Pricing:** You are charged 2x the regional price and for 10 key versions (charged at double the rate of a single version in the Standard plan).

## How many key versions do you have?
{: #pricing-plan-how-many-keys}
{: ui}

To see how many versions you have of each key you have deployed:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. On the **Keys** panel, click the ⋯ icon and select **Key details**. This opens a side panel that shows, among other things, the number of versions of this key you have.

5. Repeat this process for every key in your instance. Note that because only root keys can be rotated, all of your standard keys only have one version.

## Switching to a different plan in the console
{: #pricing-plan-migrate-ui}
{: ui}

Users who want enhanced cross regional resiliency must either create a new instance using the new plan, which was introduced on 1 January 2025, or switch an existing instance to the plan. This can be done in the console by following these steps:

1. [Log in to the {{site.data.keyword.cloud_notm}} console](/login/){: external}.

2. Go to **Menu** &gt; **Resource List** to view a list of your resources.

3. From your {{site.data.keyword.cloud_notm}} resource list, select your provisioned instance of {{site.data.keyword.keymanagementserviceshort}}.

4. In the left navigation, select the **Plans** page.

5. To change your pricing plan, select the desired plan and click **Save**. Your pricing plan has now been changed.

## Updating a pricing plan by using the CLI
{: #cpricing-plan-migrate-cli}
{: cli}

Complete the following steps to update a pricing plan by using the {{site.data.keyword.Bluemix_notm}} command-line interface (CLI).

1. Check whether the service is enabled with the resource controller.

   ```
   ibmcloud catalog service <service_name>
   ```
   {: codeblock}

   The output lists `RC Compatible true`. Make a note of the ID of the plan that you want to update to.

   ```
   RC Compatible      true
   RC Provisionable   true
   IAM Compatible     true
   Children   Name                      Kind         ID
              lite                      plan         4bcd3fgh-3cf2-47c0-93d4-d2f2289eac28
              standard                  plan         264d0450-996d-4bcd-894d-fc7018dacf1e
    ```

1. Update the pricing plan for your service instance.

   - If the service is enabled with the resource controller, run the [`ibmcloud resource service-instance-update` command](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_update).

     ```
     ibmcloud resource service-instance-update <service_instance_name> --service-plan-id <plan_id>
     ```
     {: codeblock}


## Updating a pricing plan by using the API
{: #pricing-plan-migrate-api}
{: api}

You can programmatically update a pricing plan for a service by calling the [{{site.data.keyword.cloud_notm}} Resource Controller API](/apidocs/resource-controller/resource-controller#update-resource-instance) as shown in the following sample request. The example updates a pricing plan for an instance.

```
curl -X PATCH https://resource-controller.cloud.ibm.com/v2/resource_instances/8d7af921-b136-4078-9666-081bd8470d94 -H 'Authorization: Bearer <>' -H 'Content-Type: application/json' -d '{
    "name": "my-instance-new-binding-1",
    "resource_plan_id": "744bfc56-d12c-4866-88d5-dac9139e0e5d"
  }'
```
{: codeblock}
{: curl}

```java
Map<String, Object> parameters = new HashMap<String, Object>();
parameters.put("exampleProperty", "exampleValue");

UpdateResourceInstanceOptions updateResourceInstanceOptions = new UpdateResourceInstanceOptions.Builder()
  .id(instanceGuid)
  .name(resourceInstanceUpdateName)
  .parameters(parameters)
  .build();

Response<ResourceInstance> response = service.updateResourceInstance(updateResourceInstanceOptions).execute();
ResourceInstance resourceInstance = response.getResult();

System.out.printf("updateResourceInstance() response:\n%s\n", resourceInstance.toString());
```
{: codeblock}
{: java}

```
getAccountUsage(params)
```
{: codeblock}
{: javascript}

```python
parameters = {
    'exampleProperty': 'exampleValue'
}
resource_instance = resource_controller_service.update_resource_instance(
    id=instance_guid,
    name=resource_instance_update_name,
    parameters=parameters
).get_result()

print('\nupdate_resource_instance() response:\n',
      json.dumps(resource_instance, indent=2))
```
{: codeblock}
{: python}

```go
parameters := map[string]interface{}{"exampleProperty": "exampleValue"}
updateResourceInstanceOptions := resourceControllerService.NewUpdateResourceInstanceOptions(
  instanceGUID,
)
updateResourceInstanceOptions = updateResourceInstanceOptions.SetName(resourceInstanceUpdateName)
updateResourceInstanceOptions = updateResourceInstanceOptions.SetParameters(parameters)

resourceInstance, response, err := resourceControllerService.UpdateResourceInstance(updateResourceInstanceOptions)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(resourceInstance, "", "  ")
fmt.Printf("\nUpdateResourceInstance() response:\n%s\n", string(b))
```
{: codeblock}
{: go}

If you want to purchase a larger subscription, contact [{{site.data.keyword.Bluemix_notm}} Sales](https://www.ibm.com/cloud?contactmodule){: external}.

---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-25"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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

# Provisioning the service
{: #provision}

You can create an instance of {{site.data.keyword.keymanagementservicefull}} by using the {{site.data.keyword.cloud_notm}} console or the {{site.data.keyword.cloud_notm}} CLI.
{: shortdesc}

## Provisioning from the {{site.data.keyword.cloud_notm}} console
{: #provision-gui}

To provision an instance of {{site.data.keyword.keymanagementserviceshort}} from the {{site.data.keyword.cloud_notm}} console, complete the following steps.

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://{DomainName}){: external}.
2. Click **Catalog** to view the list of services that are available on {{site.data.keyword.cloud_notm}}.
3. From the All Categories navigation pane, click the **Security and Identity** category.
4. From the list of services, click the **{{site.data.keyword.keymanagementserviceshort}}** tile.
5. Select a service plan, and click **Create** to provision an instance of {{site.data.keyword.keymanagementserviceshort}} in the account, region, and resource group where you are logged in.

## Provisioning from the {{site.data.keyword.cloud_notm}} CLI
{: #provision-cli}

You can also provision an instance of {{site.data.keyword.keymanagementserviceshort}} by using the {{site.data.keyword.cloud_notm}} CLI. 

1. Log in to {{site.data.keyword.cloud_notm}} through the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **Note:** If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.

2. Select the account, region, and resource group where you would like to create a {{site.data.keyword.keymanagementserviceshort}} service instance.

    You can use the following command to set your target region and resource group.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. Provision an instance of {{site.data.keyword.keymanagementserviceshort}} within that account and resource group.

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    Replace `<instance_name>` with a unique alias for your service instance.

4. Optional: Verify that the service instance was created successfully.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## What's next
{: #provision-service-next-steps}

To find out more about programmatically managing your keys, [check out the {{site.data.keyword.keymanagementserviceshort}} API reference doc](https://{DomainName}/apidocs/key-protect){: external}.

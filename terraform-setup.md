---

copyright:
  years: 2026
lastupdated: "2026-03-05"

subcollection: key-protect

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Terraform for {{site.data.keyword.keymanagementserviceshort}}
{: #terraform-setup}

Terraform on {{site.data.keyword.cloud}} enables predictable and consistent creation of {{site.data.keyword.cloud_notm}} services so that you can rapidly build complex, multi-tier cloud environments following Infrastructure as Code (IaC) principles. Similar to using the {{site.data.keyword.cloud_notm}} CLI or API and SDKs, you can automate the creation, update, and deletion of your {{site.data.keyword.keymanagementserviceshort}} instances by using HashiCorp Configuration Language (HCL).
{: shortdesc}

Looking for a managed Terraform on {{site.data.keyword.cloud}} solution? Try out [{{site.data.keyword.bplong}}](/docs/schematics?topic=schematics-getting-started). With {{site.data.keyword.bpshort}}, you can use the Terraform scripting language that you are familiar with, but you don't have to worry about setting up and maintaining the Terraform command line and the {{site.data.keyword.cloud}} Provider plug-in. {{site.data.keyword.bpshort}} also provides pre-defined Terraform templates that you can easily install from the {{site.data.keyword.cloud}} catalog.
{: tip}

## Installing Terraform and configuring resources for {{site.data.keyword.keymanagementserviceshort}}
{: #install-terraform}

Provisioning a new {{site.data.keyword.keymanagementserviceshort}} Dedicated instance is available through the {{site.data.keyword.cloud}} console UI and the IBM Cloud CLI. Creating new {{site.data.keyword.keymanagementserviceshort}} Dedicated instances with Terraform is not supported. you can, however, use Terraform to provision resources like keys. To do this, the environment variable `IBMCLOUD_KP_API_ENDPOINT` must be set to the public or private API endpoint of the specific {{site.data.keyword.keymanagementserviceshort}} Dedicated instance.
{: important}

Before you can create an authorization by using Terraform, make sure that you have completed the following:

* Make sure that you have the [required access](/docs/key-protect?topic=key-protect-manage-access) to create and work with {{site.data.keyword.keymanagementserviceshort}} resources.
* Install the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. For more information, see the tutorial for [Getting started with Terraform on {{site.data.keyword.cloud}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started). The plug-in abstracts the {{site.data.keyword.cloud_notm}} APIs that are used to complete this task.
* Create a Terraform configuration file that is named `main.tf`. In this file, you define resources by using HashiCorp Configuration Language. For more information, see the [Terraform documentation](https://developer.hashicorp.com/terraform/language){: external}.

1. Create a {{site.data.keyword.keymanagementserviceshort}} instance by using the `ibm_resource_instance` resource argument in your `main.tf` file.

   The {{site.data.keyword.keymanagementserviceshort}} instance in the following example is named `my_kp` and is created with the tiered pricing plan in the `us-south` region. The `user@ibm.com` is assigned the Manager role in the IAM access policy. For other supported regions, see [Regions and endpoints](/docs/key-protect?topic=key-protect-regions).

   ```terraform
   resource "ibm_resource_instance" "kms_instance" {
     name     = "my_kp"
     service  = "kms"
     plan     = "tiered-pricing"
     location = "us-south"
   }

   resource "ibm_iam_user_policy" "policy" {
     ibm_id = "user@ibm.com"
     roles  = ["Manager"]

     resources {
       service              = "kms"
       resource_instance_id = element(split(":", ibm_resource_instance.kms_instance.id), 7)
     }
   }
   ```
   {: codeblock}

2. After you finish building your configuration file, initialize the Terraform CLI. For more information, see [Initializing Working Directories](https://developer.hashicorp.com/terraform/cli/init){: external}.

   ```terraform
   terraform init
   ```
   {: pre}

3. Provision the resources from the `main.tf` file. For more information, see [Provisioning Infrastructure with Terraform](https://developer.hashicorp.com/terraform/cli/run){: external}.

   1. Run `terraform plan` to generate a Terraform execution plan to preview the proposed actions.

      ```terraform
      terraform plan
      ```
      {: pre}

   1. Run `terraform apply` to create the resources that are defined in the plan.

      ```terraform
      terraform apply
      ```
      {: pre}

4. From the [{{site.data.keyword.cloud_notm}} resource list](/resources){: external}, select the {{site.data.keyword.keymanagementserviceshort}} instance that you created and note the instance ID.
5. Verify that the access policy is successfully assigned. For more information, see [Reviewing assigned access in the console](/docs/account?topic=account-assign-access-resources&interface=ui#review-your-access-console).

## What's next?
{: #terraform-setup-next}

Now that you successfully created your first {{site.data.keyword.keymanagementserviceshort}} service instance with Terraform on {{site.data.keyword.cloud_notm}}, you can choose between the following tasks:

* [`ibm_kms_key`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key)

* [kms_key_rings](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key_rings)

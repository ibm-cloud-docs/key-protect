---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-16"

keywords: Key Protect CLI plugin, KMS plug-in, KMS plugin

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

# Setting up the CLI
{: #set-up-cli}

You can use the {{site.data.keyword.keymanagementservicelong_notm}} CLI plug-in
to help you create, import, and manage encryption keys.

To find out more about using the {{site.data.keyword.keymanagementserviceshort}}
CLI plug-in, check out the
[{{site.data.keyword.keymanagementserviceshort}} CLI reference doc](/docs/key-protect?topic=key-protect-cli-plugin-key-protect-cli-reference).
{: tip}

## Installing the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
{: #install-cli}

Before you can set up the {{site.data.keyword.keymanagementserviceshort}} CLI
plug-in, install the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

To install the CLIs:

1. Install the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    After you install the CLI, you can run `ibmcloud` commands to interact with your cloud services.

1. Log in to {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

1. To start managing encryption keys, install the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in.

    ```sh
    ibmcloud plugin install key-protect -r "IBM Cloud"
    ```
    {: pre}

1. Set the region to target a specific {{site.data.keyword.keymanagementserviceshort}} endpoint.

    ```sh
    ibmcloud kp region-set -i <INSTANCE_ID>
    ```
    {: pre}

    Replace `<INSTANCE_ID>` with the instance ID representing your {{site.data.keyword.keymanagementserviceshort}}. Learn more about your instance, including choosing regions, at [Provisioning the Key Protect service](/docs/key-protect?topic=key-protect-provision).

    You will be prompted to choose from a list as shown in the results.

    ```
    Select a Region:
        1. au-syd
        2. eu-de
        3. eu-fr2 (available by request)
        4. eu-gb
        5. jp-osa
        6. jp-tok
        7. us-east
        8. us-south
        9. staging (us-south)
        Enter a number:
    ```
    {: screen}

    After you select your region, you can start working with your instance.

1. Optional: Verify that the plug-in was installed successfully.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Updating the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
{: #update-cli}

For best practices, you might choose to update CLI periodically to use new features.

To update the CLI:

1. Log in to {{site.data.keyword.cloud_notm}} with the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Install the update from the plug-in repository.

    ```sh
    ibmcloud plugin update key-protect -r "IBM Cloud"
    ```
    {: pre}

3. Optional: Verify that the plug-in was updated successfully.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Uninstalling the {{site.data.keyword.keymanagementserviceshort}} CLI plug-in
{: #uninstall-cli}

1. Log in to {{site.data.keyword.cloud_notm}} with the
[{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started){: external}.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If the login fails, run the `ibmcloud login --sso` command to try again. The
    `--sso` parameter is required when you log in with a federated ID. If this
    option is used, go to the link listed in the CLI output to generate a
    one-time passcode.
    {: note}

2. Install the update from the plug-in repository.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Optional: Verify that the plug-in was uninstalled successfully.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}



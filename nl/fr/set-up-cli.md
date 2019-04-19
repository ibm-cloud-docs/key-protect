---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: install CLI plug-in, install CLI plugin, update CLI plug-in, update CLI plugin, uninstall CLI plug-in, uninstall CLI plugin, Key Protect CLI plug-in, Key Protect CLI plugin, KMS plug-in, KMS plugin

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de l'interface de ligne de commande
{: #set-up-cli}

Vous pouvez utiliser le plug-in d'interface de ligne de commande d'{{site.data.keyword.keymanagementservicelong_notm}} pour vous aider à créer, importer et gérer des clés de chiffrement.

Pour en savoir plus sur l'utilisation du plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}, consultez la [documentation de référence de l'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-cli-reference).
{: tip}

## Installation du plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Avant de configurer le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}, installez l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}. 

Pour installer les interfaces de ligne de commande :

1. Installez l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    Une fois l'interface de ligne de commande installée, vous pouvez exécuter des commandes `ibmcloud` pour interagir avec vos services cloud.

2. Connectez-vous à {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

3. Pour commencer à gérer les clés de chiffrement, installez le plug-in d'interface de ligne de commande {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. Facultatif : vérifiez que le plug-in a été correctement installé.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Mise à jour du plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #update-cli}

Vous voudrez sans doute mettre régulièrement à jour l'interface de ligne de commande pour pouvoir profiter des nouvelles fonctions.

Pour mettre à jour l'interface de ligne de commande :

1. Connectez-vous à {{site.data.keyword.cloud_notm}} avec l'interface de ligne de commande d'[{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

2. Installez la mise à jour à partir du référentiel de plug-in.

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. Facultatif : vérifiez que le plug-in a été correctement mis à jour.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Désinstallation du plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #uninstall-cli}

1. Connectez-vous à {{site.data.keyword.cloud_notm}} avec l'interface de ligne de commande d'[{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

2. Installez la mise à jour à partir du référentiel de plug-in.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Facultatif : vérifiez que le plug-in a été correctement désinstallé.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

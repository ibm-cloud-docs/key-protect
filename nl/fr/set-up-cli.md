---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

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

Pour plus d'informations sur l'utilisation de l'interface de ligne de commande, voir la documentation de référence de l'interface de ligne de commande de [{{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/cli-reference.html).
{: tip}

## Installation du plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Avant de configurer le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}, installez l'interface de ligne de commande d'[{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/index.html#overview){: new_window}. 

Pour installer les interfaces de ligne de commande :

1. Installez l'interface de ligne de commande d'[{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/index.html#overview){: new_window}.

    Une fois l'interface de ligne de commande installée, vous pouvez exécuter des commandes `ibmcloud` afin d'interagrir avec vos services cloud.

2. Connectez-vous à {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Remarque :** si la connexion échoue, exécutez la commande `ibmcloud login --sso` pour effectuer une nouvelle tentative. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien répertorié dans la sortie d'interface de ligne de commande pour générer un code d'accès unique.

3. Pour commencer à gérer les clés de chiffrement, installez le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}.

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

Vous voudrez sans doute mettre régulièrement à jour l'interface de ligne de commande afin de bénéficier des nouvelles fonctions.

Pour mettre à jour l'interface de ligne de commande :

1. Connectez-vous à {{site.data.keyword.cloud_notm}} à l'aide de l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Remarque :** si la connexion échoue, exécutez la commande `ibmcloud login --sso` pour effectuer une nouvelle tentative. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien répertorié dans la sortie d'interface de ligne de commande pour générer un code d'accès unique.

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

1. Connectez-vous à {{site.data.keyword.cloud_notm}} à l'aide de l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Remarque :** si la connexion échoue, exécutez la commande `ibmcloud login --sso` pour effectuer une nouvelle tentative. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien répertorié dans la sortie d'interface de ligne de commande pour générer un code d'accès unique.

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

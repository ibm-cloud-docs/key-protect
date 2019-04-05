---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# Extraction de l'ID d'instance
{: #retrieve-instance-ID}

Vous pouvez cibler une instance de service {{site.data.keyword.keymanagementservicelong}} individuelle pour vos opérations en incluant son identificateur unique ou son ID d'instance dans les demandes de l'API au service.
{: shortdesc}

## Affichage de votre ID d'instance dans la console {{site.data.keyword.cloud_notm}}
{: #view-instance-ID}

Vous pouvez afficher l'ID d'instance associé à votre instance de service {{site.data.keyword.keymanagementserviceshort}} en accédant à votre liste de ressources {{site.data.keyword.cloud_notm}}.

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}){: new_window}.
2. Accédez à **Menu** &gt; **Liste de ressources** puis cliquez sur **Services** pour parcourir la liste de vos services cloud.
3. Cliquez sur la ligne qui décrit votre instance de service {{site.data.keyword.keymanagementserviceshort}}.
4. Dans la vue détaillée du service, copiez la valeur **GUID**.

    La valeur **GUID** représente l'ID d'instance qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}} de manière univoque.

## Extraction d'un ID d'instance avec l'interface de ligne de commande
{: #retrieve-instance-ID-cli}

Vous pouvez également extraire l'ID d'instance de votre instance de service à l'aide de l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-overview){: new_window}.

1. Connectez-vous à {{site.data.keyword.cloud_notm}} avec l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

2. Sélectionnez le compte, la région et le groupe de ressources contenant votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.

3. Extrayez le nom de ressource de cloud (CRN) qui identifie de manière unique l'instance de service {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Remplacez `<instance_name>` par l'alias unique que vous avez affecté à votre instance de service {{site.data.keyword.keymanagementserviceshort}}. L'exemple partiel suivant présente la sortie de l'interface de ligne de commande.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    La valeur _42454b3b-5b06-407b-a4b3-34d9ef323901_ est un exemple d'ID d'instance.


## Extraction d'un ID d'instance avec l'API
{: #retrieve-instance-ID-api}

Si vous le souhaitez, vous pouvez extraire l'ID d'instance à l'aide d'un programme pour générer et connecter votre application. Vous pouvez appeler l'[API {{site.data.keyword.cloud_notm}} Resource Controller ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/resource-controller), puis acheminer la sortie JSON vers `jq` pour extraire cette valeur.

1. [Extrayez un jeton d'accès {{site.data.keyword.cloud_notm}} IAM](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. Appelez l'[API Resource Controller ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/resource-controller) pour extraire votre ID d'instance.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    Remplacez `<instance_name>` par l'alias unique que vous avez affecté à votre instance de service {{site.data.keyword.keymanagementserviceshort}}. La sortie suivante montre un exemple d'ID d'instance :

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

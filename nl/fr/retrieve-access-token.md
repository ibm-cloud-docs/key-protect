---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# Extraction d'un jeton d'accès
{: #retrieve-access-token}

Commencez à utiliser les API d'{{site.data.keyword.keymanagementservicelong}} en authentifiant vos demandes au service avec un jeton d'accès {{site.data.keyword.iamlong}} (IAM).
{: shortdesc}

## Extraction d'un jeton d'accès avec l'interface de ligne de commande
{: #retrieve-token-cli}

Vous pouvez utiliser l'[interface CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external} pour générer rapidement votre jeton d'accès Cloud IAM personnel.

1. Connectez-vous à {{site.data.keyword.cloud_notm}} via l'[interface CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

2. Sélectionnez le compte, la région et le groupe de ressources contenant votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.

3. Exécutez la commande suivante pour extraire votre jeton d'accès Cloud IAM.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    L'exemple partiel suivant montre un jeton IAM extrait.

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## Extraction d'un jeton d'accès avec l'API
{: #retrieve-token-api}

Vous pouvez extraire votre jeton d'accès à l'aide d'un programme en créant d'abord une [clé d'API d'ID de service](/docs/iam?topic=iam-serviceidapikeys) pour votre application, puis en échangeant votre clé d'API contre un jeton {{site.data.keyword.cloud_notm}} IAM.

1. Connectez-vous à {{site.data.keyword.cloud_notm}} via l'[interface CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si la connexion échoue, exécutez la commande `ibmcloud login --sso`, puis réessayez. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.
    {: note}

2. Sélectionnez le compte, la région et le groupe de ressources contenant votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.

3. Créez un [ID de service](/docs/iam?topic=iam-serviceids#creating-a-service-id) pour votre application.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [Affectez une politique d'accès](/docs/iam?topic=iam-serviceidpolicy) à l'ID de service.

    Vous pouvez affecter des droits d'accès à votre ID de service [à l'aide de la console {{site.data.keyword.cloud_notm}}](/docs/iam?topic=iam-serviceidpolicy#access_new). Pour savoir comment les rôles d'accès _Gestionnaire_, _Editeur_ et _Lecteur_ sont mappés à des actions spécifiques du service {{site.data.keyword.keymanagementserviceshort}}, voir [Rôles et droits](/docs/services/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

5. Créez une [clé d'API d'ID de service](/docs/iam?topic=iam-serviceidapikeys).

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  Remplacez `<service_ID_name>` par l'alias unique que vous avez affecté à votre ID de service à l'étape précédente. Sauvegardez la clé d'API en la téléchargeant dans un emplacement sécurisé. 

6. Appelez l'[API IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api) pour extraire votre jeton d'accès.

    ```cURL
    curl -X POST \
      "https://iam.cloud.ibm.com/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>" > token.json
    ```
    {: codeblock}

    Dans la demande, remplacez `<API_KEY>` par la clé d'API que vous avez créée à l'étape précédente. L'exemple partiel suivant montre le contenu du fichier `token.json` :

    ```
    {
    "access_token": "eyJraWQiOiIyM...",
    "expiration": 1512161390,
    "expires_in": 3600,
    "refresh_token": "...",
    "token_type": "Bearer"
    }
    ```
    {: screen}

    Utilisez l'ensemble de la valeur `access_token` avec le préfixe du type de jeton _Bearer_ pour gérer les clés de votre service à l'aide d'un programme via l'API {{site.data.keyword.keymanagementserviceshort}}. Pour voir un exemple de demande d'API {{site.data.keyword.keymanagementserviceshort}}, veuillez vous reporter à la section [Mise en forme de la demande d'API](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request).

    Les jetons d'accès sont valides pendant 1 heure, mais vous pouvez les régénérer si besoin. Pour maintenir l'accès au service, régénérez régulièrement le jeton d'accès pour votre clé d'API en appelant l'[API IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api).   
    {: note }

    <!--You can also pipe the output to `jq`, and then grab only the `access_token` value `| jq .access_token-->

    <!--You use IBM® Cloud Identity and Access Management (IAM) tokens to make authenticated requests to IBM Watson™ services without embedding service credentials in every call. IAM authentication uses access tokens for authentication, which you acquire by sending a request with an API key.-->

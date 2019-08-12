---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# Création d'une clé de transport
{: #create-transport-keys}

Vous pouvez activer l'importation sécurisée de matériel de clé racine vers le cloud en créant d'abord une clé de chiffrement de transport pour votre instance de service {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Les clés de transport sont utilisées pour chiffrer et importer en toute sécurité le matériel de clé racine dans {{site.data.keyword.keymanagementserviceshort}} en fonction des politiques que vous spécifiez. Pour en savoir plus sur l'importation sécurisée de vos clés dans le cloud, voir [Apport de vos clés de chiffrement au cloud](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

Les clés de transport sont actuellement une fonction bêta. Les fonctions bêta peuvent changer à tout moment et les mises à jour futures peuvent introduire des changements qui sont incompatibles avec la dernière version.
{: important}

## Création d'une clé de transport avec l'API
{: #create-transport-key-api}

Créez une clé de transport associée à votre instance de service {{site.data.keyword.keymanagementserviceshort}} en soumettant un appel `POST` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Définissez une politique pour votre clé de transport en appelant l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
    }'
    ```
    {: codeblock}

    Remplacez les variables de l'exemple de demande conformément au tableau suivant :

      <table>
        <tr>
          <th>Variable</th>
          <th>Description</th>
        </tr>
        <tr>
          <td><varname>region</varname></td>
          <td><strong>Obligatoire.</strong> Abréviation de la région, comme <code>us-south</code> ou <code>eu-gb</code>, représentant la zone géographique dans laquelle votre instance de service {{site.data.keyword.keymanagementserviceshort}} réside. Pour plus d'informations, voir <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Noeud final de service régional</a>.</td>
        </tr>
        <tr>
          <td><varname>IAM_token</varname></td>
          <td><strong>Obligatoire.</strong> Votre jeton d'accès {{site.data.keyword.cloud_notm}}. Incluez l'ensemble du contenu du jeton <code>IAM</code>, y compris la valeur Bearer, dans la demande cURL. Pour plus d'informations, voir <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Extraction d'un jeton d'accès</a>.</td>
        </tr>
        <tr>
          <td><varname>instance_ID</varname></td>
          <td><strong>Obligatoire.</strong> Identificateur unique affecté à votre instance de service {{site.data.keyword.keymanagementserviceshort}}. Pour plus d'informations, voir <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Extraction d'un ID d'instance</a>.</td>
        </tr>
        <tr>
          <td><varname>expiration_time</varname></td>
          <td>
            <p>Temps en secondes depuis la création d'une clé de transport qui détermine pendant combien de temps la clé reste valide.</p>
            <p>La valeur minimale est de 300 secondes (5 minutes), et la valeur maximale est de 86400 (24 heures). La valeur par défaut est 600 (10 minutes).</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>Nombre de fois qu'une clé de transport peut être extraite à l'intérieur de sa durée d'expiration avant de cesser d'être accessible. La valeur par défaut est 1.</td>
        </tr>
          <caption style="caption-side:bottom;">Tableau 1. Description des variables requises pour ajouter une clé racine à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}.</caption>
      </table>

    Une demande `POST api/v2/lockers` qui aboutit crée une clé de transport pour votre instance de service et renvoie sa valeur d'ID, ainsi que d'autres métadonnées. L'ID est un identificateur unique qui est associé à votre clé de transport et est utilisé pour les appels adressés ultérieurement à l'API {{site.data.keyword.keymanagementserviceshort}}.

3. Facultatif : vérifiez que la clé de transport a été créée en exécutant l'appel suivant pour extraire les métadonnées sur votre instance de service.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Etapes suivantes
{: #create-transport-key-next-steps}

- Pour en savoir plus sur l'utilisation d'une clé de transport pour importer des clés racine dans le service, consultez [Importation des clés racine](/docs/services/key-protect?topic=key-protect-import-root-keys).
- Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, voir la [documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.

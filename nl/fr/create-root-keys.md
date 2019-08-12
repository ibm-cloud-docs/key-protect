---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: create root key, create key-wrapping key, create CRK, create CMK, create customer key, create root key in Key Protect, create key-wrapping key in Key Protect, create customer key in Key Protect, key-wrapping key, root key API examples

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

# Création de clés racine
{: #create-root-keys}

Vous pouvez utiliser {{site.data.keyword.keymanagementservicefull}} pour créer des clés racine à l'aide de l'interface graphique {{site.data.keyword.keymanagementserviceshort}} ou d'un programme via l'API {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Les clés racine sont des clés d'encapsulage de clés symétriques qui permettent d'assurer la sécurité des données chiffrées dans le cloud. Pour en savoir plus sur les clés racine, voir [Protection des données avec le chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption). 

## Création de clés racine avec l'interface graphique
{: #create-root-key-gui}

[Après avoir créé une instance du service](/docs/services/key-protect?topic=key-protect-provision), procédez comme suit pour créer une clé standard avec l'interface graphique {{site.data.keyword.keymanagementserviceshort}}.

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}}](https://{DomainName}){: external}.
2. Accédez à **Menu** &gt; **Liste de ressources** pour afficher la liste de vos ressources.
3. Dans la liste de ressources {{site.data.keyword.cloud_notm}}, sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.
4. Pour créer une nouvelle clé, cliquez sur **Ajouter une clé** et sélectionnez la fenêtre **Créer une clé**.

    Indiquez les détails relatifs à la clé :

    <table>
      <tr>
        <th>Paramètre</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Alias unique et lisible permettant d'identifier facilement votre clé.</p>
          <p>Pour protéger votre vie privée, assurez-vous que le nom de clé ne contient pas d'informations identifiant la personne, comme votre nom ou votre emplacement.</p>
        </td>
      </tr>
      <tr>
        <td>Key type</td>
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Type de clé</a> que vous souhaitez gérer dans {{site.data.keyword.keymanagementserviceshort}}. Dans la liste des types de clés, sélectionnez <b>Root key</b>.</td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des paramètres de la fenêtre <b>Créer une clé</b></caption>
    </table>

5. Une fois les détails de la clé indiqués, cliquez sur **Create key** pour confirmer l'opération. 

Les clés qui sont créées dans le service sont des clés symétriques de 256 bits prises en charge par l'algorithme AES-CBC-PAD. Pour renforcer la sécurité, elles sont générées par des modules de sécurité matériels (ou modules HSM) certifiés FIPS 140-2 niveau 3 résidant dans des centres de données {{site.data.keyword.cloud_notm}} sécurisés. 

## Création de clés racine avec l'API
{: #create-root-key-api}

Créez une clé racine en soumettant une demande `POST` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Appelez l'[API {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/apidocs/key-protect){: external} à l'aide de la commande cURL suivante.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<key_alias>",
       "description": "<key_description>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "extractable": <key_type>
       }
     ]
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
        <td><varname>correlation_ID</varname></td>
        <td>Identificateur unique qui est utilisé pour suivre et corréler des transactions.</td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td>
          <p><strong>Obligatoire.</strong> Nom lisible permettant d'identifier facilement votre clé.</p>
          <p>Important : pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</p>
        </td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>
          <p>Description étendue de votre clé.</p>
          <p>Important : pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</p>
        </td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Date et heure d'expiration de la clé dans le système, au format RFC 3339. Si l'attribut <code>expirationDate</code> est omis, la clé n'expire pas. </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Valeur booléenne qui détermine si le matériel de clé peut quitter le service.</p>
          <p>Lorsque vous affectez l'attribut <code>false</code> à <code>extractable</code>, le service crée une clé racine que vous pouvez utiliser pour des opérations <code>wrap</code> ou <code>unwrap</code>.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 1. Description des variables requises pour ajouter une clé racine à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Pour protéger la confidentialité de vos données personnelles, évitez d'entrer des informations identifiant la personne, comme votre nom ou votre emplacement, lorsque vous ajoutez des clés au service. Pour obtenir d'autres exemples d'informations identifiant la personne, veuillez consulter la section 2.2 du document [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
    {: important}

    Une réponse `POST api/v2/keys` qui aboutit renvoie la valeur de l'ID de la clé, ainsi que d'autres métadonnées. L'ID est un identificateur unique qui est affecté à la clé et qui est utilisé pour les appels adressés ultérieurement à {{site.data.keyword.keymanagementserviceshort}}.

3. Facultatif : Vérifiez que la clé a été créée en exécutant l'appel suivant pour parcourir les clés de l'instance de service {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Lorsque vous créez une clé racine avec le service, la clé reste dans les limites du service {{site.data.keyword.keymanagementserviceshort}} et son matériel ne peut pas être extrait.
    {: note} 

## Etapes suivantes
{: #create-root-key-next-steps}

- Pour plus d'informations sur la protection de clés à l'aide du chiffrement d'enveloppe, voir [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, [voir la documentation de référence {{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect){: external}.

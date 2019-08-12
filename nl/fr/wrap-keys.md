---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: wrap key, encrypt data encryption key, protect data encryption key, envelope encryption API examples

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

# Encapsulage de clés
{: #wrap-keys}

Vous pouvez gérer et protéger vos clés de chiffrement avec une clé racine à l'aide de l'API {{site.data.keyword.keymanagementservicelong}}.
{: shortdesc}

Lorsque vous encapsulez une clé DEK (Data Encryption Key) avec une clé racine, {{site.data.keyword.keymanagementserviceshort}} associe la puissance de plusieurs algorithmes pour protéger la confidentialité et l'intégrité des données chiffrées.  

Pour découvrir comment l'encapsulage de clés peut vous aider à contrôler la sécurité des données au repos dans le cloud, reportez-vous à la rubrique [Protection des données avec le chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Encapsulage de clés à l'aide de l'API
{: #wrap-key-api}

Vous pouvez protéger la clé DEK indiquée avec une clé racine que vous gérez dans {{site.data.keyword.keymanagementserviceshort}}.

Lorsque vous indiquez une clé racine pour l'encapsulage, vérifiez qu'il s'agit d'une clé racine de 128, 192 ou 256 bits pour que l'opération aboutisse. Si vous créez une clé racine dans le service, {{site.data.keyword.keymanagementserviceshort}} génère à partir de son module de sécurité matériel une clé 256 bits prise en charge par l'algorithme AES-CBC-PAD.
{: note}

[Après avoir désigné une clé racine dans le service](/docs/services/key-protect?topic=key-protect-create-root-keys), vous pouvez encapsuler une clé DEK avec un chiffrement avancé en soumettant un appel `POST` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=wrap
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copiez le matériel de clé DEK que vous voulez gérer et protéger.

    Si vous avez des privilèges de responsable ou d'auteur pour l'instance de service {{site.data.keyword.keymanagementserviceshort}}, [vous pouvez extraire le matériel de clé en soumettant une demande `GET /v2/keys/<key_ID>`](/docs/services/key-protect?topic=key-protect-view-keys#view-keys-api	).

3. Copiez l'ID de la clé racine que vous voulez utiliser pour l'encapsulage.

4. Exécutez la commande cURL suivante pour protéger la clé avec une opération d'encapsulage :

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "plaintext": "<data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
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
        <td><varname>key_ID</varname></td>
        <td><strong>Obligatoire.</strong> ID unique de la clé racine que vous souhaitez utiliser pour l'encapsulage.</td>
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
        <td><varname>data_key</varname></td>
        <td>Matériel de clé de la clé de chiffrement de données que vous voulez gérer et protéger. La valeur <code>plaintext</code> doit être codée en base64. Pour générer une clé de chiffrement de données, omettez l'attribut <code>plaintext</code>. Le service génère un texte brut aléatoire (32 octets), l'encapsule, puis renvoie la valeur générée et la valeur encapsulée dans la réponse.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>Données d'authentification supplémentaires (AAD) utilisées pour sécuriser davantage la clé. Chaque chaîne peut inclure jusqu'à 255 caractères. Si vous indiquez des données d'authentification supplémentaires lorsque vous soumettez un appel d'encapsulage au service, vous devez indiquer les mêmes données d'authentification supplémentaires lors de l'appel de désencapsulage ultérieur.<br></br>Important : le service {{site.data.keyword.keymanagementserviceshort}} ne sauvegarde pas les données d'authentification supplémentaires. Si vous indiquez des données d'authentification supplémentaires, sauvegardez ces données dans un emplacement sécurisé afin de pouvoir y accéder et fournir les mêmes données lors des demandes de désencapsulage ultérieures.</td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des variables nécessaires pour encapsuler une clé spécifiée dans {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    La clé de chiffrement de données encapsulée, qui contient le matériel de clé codé en base64, est renvoyée dans la section entity-body de la réponse. L'objet JSON suivant présente un exemple de valeur renvoyée.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    Si vous omettez l'attribut `plaintext` dans votre demande d'encapsulage, le service renvoie à la fois la clé de chiffrement de données (DEK) générée et la clé de chiffrement de données encapsulée codées en base64.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    La valeur `plaintext` représente la DEK désencapsulée et la valeur `ciphertext` représente la DEK encapsulée.
    
    Si vous voulez que {{site.data.keyword.keymanagementserviceshort}} génère pour vous une nouvelle clé de chiffrement de données (DEK), vous pouvez également transmettre une section body vide dans votre demande d'encapsulage. La clé de chiffrement de données générée, qui contient le matériel de clé codé en base64, est renvoyée dans la section entity-body de la réponse, avec la clé de chiffrement de données encapsulée.
    {: tip}
    

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

# Désencapsulage de clés
{: #unwrap-keys}

Si vous êtes un utilisateur privilégié, vous pouvez désencapsuler une clé DEK pour accéder à son contenu via l'API {{site.data.keyword.keymanagementservicefull}}. La procédure de désencapsulage déchiffre le contenu d'une clé DEK et vérifie l'intégrité de son contenu en renvoyant le matériel de la clé d'origine au service de données {{site.data.keyword.cloud_notm}}.
{: shortdesc}

Pour découvrir comment l'encapsulage de clés peut vous aider à contrôler la sécurité des données au repos dans le cloud, reportez-vous à la rubrique [Chiffrement d'enveloppe](/docs/services/key-protect/concepts/envelope-encryption.html).

## Désencapsulage de clés à l'aide de l'API
{: #api}

[Après avoir soumis un appel d'encapsulage au service](/docs/services/key-protect/wrap-keys.html), vous pouvez désencapsuler une clé de chiffrement de données spécifiée pour accéder à son contenu en soumettant un appel `POST` au noeud final suivant.

```
https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_id>?action=unwrap
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service](/docs/services/key-protect/access-api.html).

2. Copiez l'ID de la clé racine que vous avez utilisée pour exécuter la demande initiale d'encapsulage.

    Vous pouvez extraire l'ID d'une clé en soumettant une demande `GET /v2/keys` ou en affichant vos clés dans l'interface graphique {{site.data.keyword.keymanagementserviceshort}}.

3. Copiez la valeur `ciphertext` qui a été renvoyée lors de la demande d'encapsulage initiale.

4. Exécutez la commande cURL suivante pour déchiffrer et authentifier le matériel relatif à la clé :

    ```cURL
    curl -X POST \
      'https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
      "ciphertext": "<encrypted_data_key>",
      "aad": ["<additional_data>", "<additional_data>"]
    }'
    ```
    {: codeblock}

    Pour utiliser les clés dans une organisation et un espace Cloud Foundry de votre compte, remplacez `Bluemix-Instance` par les en-têtes `Bluemix-org` et `Bluemix-space` appropriés. [Pour plus d'informations, voir la documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Remplacez les variables dans l'exemple de demande en fonction du tableau suivant :
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obligatoire.</strong> Abréviation de la région, comme <code>us-south</code> ou <code>eu-gb</code>, représentant la zone géographique dans laquelle votre instance de service {{site.data.keyword.keymanagementserviceshort}} réside. Pour plus d'informations, voir <a href="/docs/services/key-protect/regions.html#endpoints">Noeud final de service régional</a>.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obligatoire.</strong> ID unique de la clé racine que vous avez utilisée pour la demande d'encapsulage initiale.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Obligatoire.</strong> Votre jeton d'accès {{site.data.keyword.cloud_notm}}. Incluez l'ensemble du contenu du jeton <code>IAM</code>, y compris la valeur Bearer, dans la demande cURL. Pour plus d'informations, voir <a href="/docs/services/key-protect/access-api.html#retrieve-token">Extraction d'un jeton d'accès</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Obligatoire.</strong> Identificateur unique affecté à votre instance de service {{site.data.keyword.keymanagementserviceshort}}. Pour plus d'informations, voir <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Extraction d'un ID d'instance</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>Identificateur unique qui est utilisé pour suivre et corréler des transactions.</td>
      </tr>
      <tr>
        <td><varname>additional_data</varname></td>
        <td>Données d'authentification supplémentaires (AAD) utilisées pour sécuriser davantage la clé. Chaque chaîne peut inclure jusqu'à 255 caractères. Si vous indiquez des données d'authentification supplémentaires lorsque vous soumettez un appel d'encapsulage au service, vous devez indiquer les mêmes données d'authentification supplémentaires lors de l'appel de désencapsulage.</td>
      </tr>
      <tr>
        <td><varname>encrypted_data_key</varname></td>
        <td><strong>Obligatoire.</strong> Valeur <code>ciphertext</code> qui a été renvoyée lors d'une opération d'encapsulage.</td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des variables nécessaires pour désencapsuler des clés dans {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Le matériel relatif à la clé d'origine est renvoyé dans la section entity-body de la réponse. L'objet JSON suivant présente un exemple de valeur renvoyée.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}

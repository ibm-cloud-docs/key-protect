---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import standard encryption key, upload standard encryption key, import secret, persist secret, store secret, upload secret, store encryption key, standard key API examples

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

# Importation de clés standard
{: #import-standard-keys}

Vous pouvez ajouter des clés de chiffrement existantes à l'aide de l'interface graphique {{site.data.keyword.keymanagementserviceshort}} ou à l'aide d'un programme via l'API {{site.data.keyword.keymanagementserviceshort}}.

## Importation de clés standard à l'aide de l'interface graphique
{: #import-standard-key-gui}

[Après avoir créé une instance du service](/docs/services/key-protect?topic=key-protect-provision), procédez comme suit pour entrer votre clé standard existante à l'aide de l'interface graphique {{site.data.keyword.keymanagementserviceshort}}.

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/){: new_window}.
2. Accédez à **Menu** &gt; **Liste de ressources** pour afficher la liste de vos ressources.
3. Dans la liste de ressources {{site.data.keyword.cloud_notm}}, sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.
4. Pour importer une nouvelle clé, cliquez sur **Ajouter une clé**, puis sélectionnez la fenêtre **Importation de vos propres clés**.

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
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Type de clé</a> que vous souhaitez gérer dans {{site.data.keyword.keymanagementserviceshort}}. Dans la liste des types de clé, sélectionnez <b>Standard Key</b>.</td>
      </tr>
      <tr>
        <td>Key material</td>
        <td>
          <p>Matériel de clé codé en base64, comme une clé symétrique, que vous souhaitez gérer dans le service.</p>
          <p>Vérifiez que le matériel de clé remplit les conditions suivantes :</p>
          <p><ul>
              <li>La taille de la clé peut atteindre 10 000 octets au maximum.</li>
              <li>Les octets de données doivent être codés en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des paramètres de la fenêtre <b>Importation de vos propres clés</b></caption>
    </table>

5. Une fois les détails de la clé indiqués, cliquez sur **Importer une clé** pour confirmer l'opération. 

## Importation de clés standard à l'aide de l'API
{: #import-standard-key-api}

Importez une clé standard en soumettant un appel `POST` au noeud final suivant :

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Appelez l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window} avec la commande cURL suivante :

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'prefer: <return_preference>' \
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
       "payload": "<key_material>",
       "extractable": <key_type>
       }
     ]
    }'
    ```
    {: codeblock}

    Pour utiliser les clés dans une organisation et un espace Cloud Foundry de votre compte, remplacez `Bluemix-Instance` par les en-têtes `Bluemix-org` et `Bluemix-space` appropriés. [Pour plus d'informations, voir la documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Remplacez les variables de l'exemple de demande conformément au tableau suivant :
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Obligatoire.</strong> Abréviation de la région, comme <code>us-south</code> ou <code>eu-gb</code>, représentant la zone géographique dans laquelle votre instance de service {{site.data.keyword.keymanagementserviceshort}} réside. Pour plus d'informations, voir <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Noeud final de service régional</a>.</td>
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
        <td><varname>return_preference</varname></td>
        <td><p>En-tête qui modifie le comportement du serveur pour les opérations <code>POST</code> et <code>DELETE</code>.</p><p>Lorsque vous affectez la valeur <code>return=minimal</code> à la variable <em>return_preference</em>, le service renvoie uniquement les métadonnées de la clé, comme le nom et l'ID de la clé, dans la section entity-body de la réponse. Si cette variable a pour valeur <code>return=representation</code>, le service renvoie à la fois le matériel de clé et ses métadonnées.</p></td>
      </tr>
      <tr>
        <td><varname>key_alias</varname></td>
        <td><strong>Obligatoire.</strong> Nom lisible permettant d'identifier facilement votre clé. Pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Description étendue de votre clé. Pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Date et heure d'expiration de la clé dans le système, au format RFC 3339. Si l'attribut <code>expirationDate</code> est omis, la clé n'expire pas. </td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>Matériel de clé codé en base64, comme une clé symétrique, que vous souhaitez gérer dans le service.</p>
          <p>Vérifiez que le matériel de clé remplit les conditions suivantes :</p>
          <p><ul>
              <li>La taille de la clé peut atteindre 10 000 octets au maximum.</li>
              <li>Les octets de données doivent être codés en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Valeur booléenne qui détermine si le matériel de clé peut quitter le service.</p>
          <p>Lorsque vous affectez la valeur <code>true</code> à l'attribut <code>extractable</code>, le service désigne la clé en tant que clé standard que vous pouvez stocker dans vos applications ou services.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 2. Description des variables requises pour ajouter une clé standard à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Pour protéger la confidentialité de vos données personnelles, évitez d'entrer des informations identifiant la personne, comme votre nom ou votre emplacement, lorsque vous ajoutez des clés au service. Pour visualiser d'autres exemples d'informations identifiant la personne, voir la section 2.2 du document [NIST Special Publication 800-122 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Une réponse `POST api/v2/keys` qui aboutit renvoie la valeur de l'ID de la clé, ainsi que d'autres métadonnées. L'ID est un identificateur unique qui est affecté à la clé et qui est utilisé pour les appels adressés ultérieurement à {{site.data.keyword.keymanagementserviceshort}}.

3. Facultatif : Vérifiez que la clé a été ajoutée en exécutant l'appel suivant pour parcourir les clés de l'instance de service {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}


## Etapes suivantes
{: #import-standard-key-next-steps}

- Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, voir la [documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.

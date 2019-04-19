---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# Importation de clés racine
{: #import-root-keys}

Vous pouvez utiliser {{site.data.keyword.keymanagementservicefull}} pour sécuriser des clés racine existantes à l'aide de l'interface graphique {{site.data.keyword.keymanagementserviceshort}} ou à l'aide d'un programme via l'API {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Les clés racine sont des clés d'encapsulage de clés symétriques qui permettent d'assurer la sécurité des données chiffrées dans le cloud. Pour en savoir plus sur l'importation des clés racine dans {{site.data.keyword.keymanagementserviceshort}}, voir [Apport de vos clés de chiffrement au cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

## Importation de clés racine à l'aide de l'interface graphique
{: #import-root-key-gui}

[Après avoir créé une instance du service](/docs/services/key-protect?topic=key-protect-provision), procédez comme suit pour ajouter la clé racine existante à l'aide de l'interface graphique {{site.data.keyword.keymanagementserviceshort}} :

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/){: new_window}.
2. Accédez à **Menu** &gt; **Liste de ressources** pour afficher la liste de vos ressources.
3. Dans la liste de ressources {{site.data.keyword.cloud_notm}}, sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.
4. Pour importer une clé, cliquez sur **Ajouter une clé**, puis sélectionnez la fenêtre **Importation de vos propres clés**.

    Indiquez les détails relatifs à la clé :

    <table>
      <tr>
        <th>Paramètre</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Nom</td>
        <td>
          <p>Alias unique et lisible permettant d'identifier facilement votre clé.</p>
          <p>Pour protéger votre vie privée, assurez-vous que le nom de clé ne contient pas d'informations identifiant la personne, comme votre nom ou votre emplacement.</p>
        </td>
      </tr>
      <tr>
        <td>Type de clé</td>
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Type de clé</a> que vous souhaitez gérer dans {{site.data.keyword.keymanagementserviceshort}}. Dans la liste des types de clés, sélectionnez <b>Root key</b>.</td>
      </tr>
      <tr>
        <td>Matériel de clé</td>
        <td>
          <p>Matériel de clé codé en base64, comme une clé d'encapsulage de clé existante, que vous souhaitez stocker et gérer dans le service.</p>
          <p>Vérifiez que le matériel de clé remplit les conditions suivantes :</p>
          <p>
            <ul>
              <li>La clé doit être une clé de 128, 192 ou 256 bits.</li>
              <li>Les octets de données, par exemple 32 octets pour 256 bits doivent être codés en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des paramètres de la fenêtre <b>Importation de vos propres clés</b></caption>
    </table>

5. Une fois les détails de la clé indiqués, cliquez sur **Importer la clé** pour confirmer l'opération. 

## Importation de clés racine avec l'API
{: #import-root-key-api}

Vous pouvez importer vos clés racine dans le service à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}.

Planifiez l'importation de vos clés en [vérifiant les options de création et de chiffrement du matériel de clé](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Pour plus de sécurité vous pouvez activer l'importation sécurisée de votre matériel de clé en utilisant une [clé de transport](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) pour chiffrer votre matériel de clé avant de le transférer sur le cloud. Si vous préférez importer une clé racine sans utiliser de clé de transport, passez à l'[étape 4](#import-root-key).
{: note}

### Etape 1 : Création d'une clé de transport
{: #create-transport-key}

Les clés de transport sont actuellement une fonction bêta. Les fonctions bêta peuvent changer à tout moment, et les mises à jour futures peuvent introduire des changements qui sont incompatibles avec la dernière version.
{: important}

Créez une clé de transport pour votre instance de service en soumettant un appel `POST` au noeud final suivant.

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
      <td><varname>expiration_time</varname></td>
      <td>
        <p>Temps en secondes depuis la création d'une clé de transport qui détermine pendant combien de temps la clé reste valide.</p>
        <p>La valeur minimale est de 300 secondes (5 minutes) et la valeur maximale est de 86400 (24 heures). La valeur par défaut est 600 (10 minutes).</p>
      </td>
    </tr>
    <tr>
      <td><varname>use_count</varname></td>
      <td>Nombre de fois qu'une clé de transport peut être extraite à l'intérieur de sa durée d'expiration avant de cesser d'être accessible. La valeur par défaut est 1.</td>
    </tr>
      <caption style="caption-side:bottom;">Tableau 2. Description des variables requises pour créer une clé de transport avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
  </table>

  Une réponse `POST api/v2/lockers` qui aboutit renvoie la valeur de l'ID de votre clé de transport, ainsi que d'autres métadonnées. L'ID est un identificateur unique qui est associé à une clé de transport et est utilisé pour les appels adressés ultérieurement à l'API {{site.data.keyword.keymanagementserviceshort}}.

### Etape 2 : Extraction de la clé de transport et du jeton d'importation
{: #retrieve-transport-key}

Extrayez une clé de transport et un jeton d'importation en soumettant un appel `GET` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers/<key_id>
```
{: codeblock}

1. Appelez l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window} avec la commande cURL suivante :

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>locker_ID</varname></td>
        <td><strong>Obligatoire.</strong> Identificateur de la clé de transport que vous avez créée à l'<a href="#create-transport-key">étape 1</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 3. Description des variables requises pour extraire une clé de transport avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Une réponse `GET api/v2/lockers/{id}` qui aboutit renvoie une clé de chiffrement publique codée DER de 4096 bits au format PKIX que vous pouvez utiliser pour chiffrer votre matériel de clé racine, ainsi qu'un jeton d'importation qui est utilisé pour vérifier l'intégrité de la clé de transport.

### Etape 3 : Chiffrement de votre matériel de clé
{: #encrypt-root-key}

Après avoir extrait votre clé de transport, utilisez la clé pour chiffrer le matériel de clé que vous souhaitez importer dans {{site.data.keyword.keymanagementserviceshort}}.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

Pour générer le matériel de clé sur site, [consultez les options dont vous disposez pour créer des clés de chiffrement symétrique](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Par exemple, vous souhaiterez peut-être utiliser le système interne de gestion des clés de votre organisation qui repose sur un module de sécurité matériel pour créer et exporter le matériel de clé.
{: note}

Pour chiffrer votre matériel de clé :

1. Exportez le matériel de clé de 256 bits au format binaire depuis votre système de gestion des clés sur site.

    Pour savoir comment créer et exporter le matériel de clé, voir la documentation de votre module de sécurité matériel ou de votre système de gestion des clés sur site.

2. Utilisez la [clé de transport extraite](#retrieve-transport-key) à l'étape 2 pour chiffrer le matériel de clé.

   Lorsque vous chiffrez votre matériel de clé, utilisez le schéma de chiffrement `RSAES_OAEP_SHA_256`. Il s'agit du schéma par défaut que {{site.data.keyword.keymanagementserviceshort}} utilise pour créer la clé de transport. Pour éviter les problèmes de déchiffrement dans {{site.data.keyword.keymanagementserviceshort}}, n'incluez pas le paramètre facultatif `label` lorsque vous exécutez le chiffrement RSAES_OAEP sur le matériel de clé. Pour savoir comment exécuter le chiffrement RSA sur votre matériel de clé, consultez la documentation de votre module de sécurité matériel ou de votre système de gestion des clés sur site.

3. Vérifiez que le matériel de clé chiffré est codé en base64 avant de passer à l'étape suivante.

### Etape 4 : Importation du matériel de clé
{: #import-root-key}

[Après avoir chiffré et codé en base64 le matériel de clé](#encrypt-root-key), importez la clé racine dans {{site.data.keyword.keymanagementserviceshort}} en soumettant un appel `POST` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Appelez l'[API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window} avec la commande cURL suivante :

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
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
       "extractable": <key_type>,
       "encryptionAlgorithm": "<encryption_algorithm>",
       "importToken": "<import_token>"
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
        <td><varname>key_alias</varname></td>
        <td><strong>Obligatoire.</strong> Nom lisible permettant d'identifier facilement votre clé. Pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</td>
      </tr>
      <tr>
        <td><varname>key_description</varname></td>
        <td>Description étendue de votre clé. Pour protéger votre vie privée, ne stockez aucune donnée personnelle comme métadonnées pour votre clé.</td>
      </tr>
      <tr>
        <td><varname>YYYY-MM-DD</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Date et heure d'expiration de la clé dans le système, au format RFC 3339. Si l'attribut <code>expirationDate</code> est omis, la clé n'expire pas.</td>
      </tr>
      <tr>
        <td><varname>key_material</varname></td>
        <td>
          <p>Matériel de clé codé en base64, comme une clé d'encapsulage de clé existante, que vous souhaitez stocker et gérer dans le service.</p>
          <p>Vérifiez que le matériel de clé remplit les conditions suivantes :</p>
          <p>
            <ul>
              <li>La clé doit être une clé de 128, 192 ou 256 bits.</li>
              <li>Les octets de données, par exemple 32 octets pour 256 bits doivent être codés en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>key_type</varname></td>
        <td>
          <p>Valeur booléenne qui détermine si le matériel de clé peut quitter le service.</p>
          <p>Lorsque vous affectez la valeur <code>false</code> à l'attribut <code>extractable</code>, le service désigne la clé en tant que clé racine disponible pour les opérations <code>wrap</code> ou <code>unwrap</code>.</p>
        </td>
      </tr>
      <tr>
        <td><varname>encryption_algorithm</varname></td>
        <td>Schéma de chiffrement utilisé pour <a href="#encrypt-root-key">chiffrer le matériel de clé</a>. Actuellement, <code>RSAES_OAEP_SHA_256</code> est pris en charge. Pour importer le matériel de clé racine sans utiliser de clé de transport ni de jeton d'importation, omettez l'attribut <code>encryption_algorithm</code>.</td>
      </tr>
      <tr>
        <td><varname>import_token</varname></td>
        <td>Jeton d'importation utilisé pour vérifier la vivacité et l'intégrité d'une clé de transport. Si vous chiffrez votre matériel de clé à l'aide d'une clé de transport, vous devez fournir le jeton d'importation que vous avez extrait à l'<a href="#retrieve-transport-key">étape 2</a>. Pour importer le matériel de clé racine sans clé de transport ni jeton d'importation, omettez l'attribut <code>importToken</code>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 4. Description des variables requises pour ajouter une clé racine avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Pour protéger la confidentialité de vos données personnelles, évitez d'entrer des informations identifiant la personne, comme votre nom ou votre emplacement, lorsque vous ajoutez des clés au service. Pour visualiser d'autres exemples d'informations identifiant la personne, voir la section 2.2 du document [NIST Special Publication 800-122 ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Une réponse `POST api/v2/keys` qui aboutit renvoie la valeur de l'ID de la clé, ainsi que d'autres métadonnées. L'ID est un identificateur unique qui est affecté à la clé et qui est utilisé pour les appels adressés ultérieurement à {{site.data.keyword.keymanagementserviceshort}}.

2. Facultatif : Vérifiez que la clé a été ajoutée en exécutant l'appel suivant pour parcourir les clés de l'instance de service {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## Etapes suivantes
{: #import-root-key-next-steps}

- Pour plus d'informations sur la protection de clés à l'aide du chiffrement d'enveloppe, voir [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, voir la [documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.

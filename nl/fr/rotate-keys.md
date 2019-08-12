---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: rotate encryption key, encryption key rotation, rotate key API examples 

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

# Rotation des clés à la demande
{: #rotate-keys}

Vous pouvez effectuer une rotation de vos clés racine à la demande à l'aide d'{{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Lorsque vous effectuez une rotation de votre clé racine, vous raccourcissez sa durée de vie et limitez la quantité d'informations qu'elle protège.   

Pour savoir comment la rotation des clés vous aide à répondre aux normes de l'industrie et à respecter les meilleurs pratiques en matière de cryptographie, voir [Rotation de vos clés de chiffrement](/docs/services/key-protect?topic=key-protect-key-rotation).

La rotation est disponible uniquement pour les clés racine. Pour en savoir plus sur vos options de rotation des clés dans {{site.data.keyword.keymanagementserviceshort}}, consultez la section [Comparaison de vos options de rotation](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Rotation de clés racine à l'aide de l'interface graphique
{: #rotate-key-gui}

Si vous préférez effectuer une rotation de vos clés racine à l'aide d'une interface graphique, vous pouvez utiliser l'interface graphique utilisateur {{site.data.keyword.keymanagementserviceshort}}.

[Après avoir créé ou importé vos clés racine existantes dans le service](/docs/services/key-protect?topic=key-protect-create-root-keys), procédez comme suit pour effectuer une rotation de clé :

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.
2. Accédez à **Menu** &gt; **Liste de ressources** pour afficher la liste de vos ressources.
3. Dans la liste de ressources {{site.data.keyword.cloud_notm}}, sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.
4. Sur la page des détails de l'application, parcourez les clés de votre service dans le tableau **Keys**.
5. Cliquez sur l'icône ⋯ pour ouvrir une liste d'options pour la clé pour laquelle vous souhaitez effectuer une rotation.
6. Dans le menu d'options, cliquez sur **Rotation d'une clé** et confirmez la rotation dans l'écran suivant.

Si vous avez initialement importé la clé racine, vous devez fournir un nouveau matériel de clé codé en base64 pour effectuer une rotation de la clé. Pour plus d'informations, voir [Importation de clés racine à l'aide de l'interface graphique utilisateur](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-gui).
{: note}

## Rotation de clés racine à l'aide de l'API
{: #rotate-key-api}

[Après avoir désigné une clé racine dans le service](/docs/services/key-protect?topic=key-protect-create-root-keys), vous pouvez appliquer une rotation à la clé en lançant un appel `POST` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service afin d'utiliser les clés dans le service.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copiez l'ID de la clé racine à laquelle appliquer la rotation.

3. Exécutez la commande cURL suivante pour remplacer la clé par un nouveau matériel de clé.

    ```cURL
    curl -X POST \
      'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <key_material>'
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
        <td><strong>Obligatoire.</strong> Identificateur unique de la clé racine à laquelle appliquer la rotation.</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>Elément de clé codée en base64 que vous voulez stocker et gérer dans le service. Cette valeur est requise si vous avez initialement importé le matériel de clé lors de l'ajout de la clé au service.</p>
          <p>Pour procéder à la rotation d'une clé initialement générée dans {{site.data.keyword.keymanagementserviceshort}}, omettez l'attribut <code>payload</code> et transmettez une demande entity-body vide. Pour procéder à la rotation d'une clé importée, fournissez un matériel de clé qui répond aux exigences suivantes :</p>
          <p>
            <ul>
              <li>La clé doit être une clé de 128, 192 ou 256 bits.</li>
              <li>Les octets de données, par exemple 32 octets pour 256 bits doivent être codés en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des variables nécessaires pour la rotation d'une clé spécifiée dans {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Une demande de rotation réussie renvoie une réponse HTTP `204 No Content`, qui indique que votre clé racine a été remplacée par un nouveau matériel de clé.

4. Facultatif : vérifiez que la clé a fait l'objet d'une rotation en exécutant l'appel suivant pour parcourir les clés de votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}
  
    Examinez la valeur `lastRotateDate` de la réponse entity-body pour connaître la date et l'heure de la dernière rotation de votre clé.
    

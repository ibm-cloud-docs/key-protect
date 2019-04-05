---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Définition d'une politique de rotation
{: #set-rotation-policy}

Vous pouvez définir une politique de rotation automatique pour une clé racine avec {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Lorsque vous définissez une politique de rotation automatique pour une clé racine, vous écourtez la durée de vie de la clé à des intervalles réguliers, et vous limitez le volume d'information qui est protégé par cette clé.

Vous pouvez uniquement créer une politique de rotation pour les clés racine qui sont générées dans {{site.data.keyword.keymanagementserviceshort}}. Si vous avez initialement importé la clé racine, vous devez fournir un nouveau matériel de clé codé en base64 pour effectuer une rotation de la clé. Pour en savoir plus, voir [Rotation des clés racine à la demande](/docs/services/key-protect?topic=key-protect-rotate-root-keys).
{: note}

Vous souhaitez en savoir plus sur les options de rotation de clé dans {{site.data.keyword.keymanagementserviceshort}} ? Consultez la section [Comparaison de vos options de rotation de clés](/docs/services/key-protect?topic=key-protect-compare-key-rotation-options) pour obtenir des informations supplémentaires.
{: tip}

## Gestion des politiques de rotation dans l'interface utilisateur
{: #manage-policies-gui}

Si vous préférez gérer les politiques de vos clés racine à l'aide d'une interface graphique, vous pouvez utiliser l'interface graphique {{site.data.keyword.keymanagementserviceshort}}.

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/){: new_window}.
2. Accédez à **Menu** &gt; **Liste de ressources** pour afficher la liste de vos ressources.
3. Dans la liste de ressources {{site.data.keyword.cloud_notm}}, sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}} mise à disposition.
4. Sur la page des détails de l'application, parcourez les clés de votre service dans le tableau **Clés**.
5. Cliquez sur l'icône ⋯ pour ouvrir une liste d'options pour une clé spécifique.
6. Dans le menu d'options, cliquez sur **Gérer la politique** pour gérer la politique de rotation de la clé.
7. Dans la liste des options de rotation, sélectionnez une fréquence de rotation en mois.

    Si votre clé a une politique de rotation existante, l'interface affiche la période de rotation existante de la clé.

8. Cliquez sur **Créer une politique** pour définir la politique de la clé.

Lorsqu'il est temps de changer la clé en fonction de l'intervalle de rotation que vous spécifiez, {{site.data.keyword.keymanagementserviceshort}} remplace automatiquement la clé racine par le nouveau matériel de clé.

## Gestion des politiques de rotation avec l'API
{: #manage-rotation-policies-api}

### Affichage d'une politique de rotation
{: #view-rotation-policy-api}

Pour obtenir une vue d'ensemble, vous pouvez parcourir les politiques associées à une clé racine en effectuant un appel `GET` vers le point final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Extraction de vos données d'authentification et de service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Extrayez la politique de rotation d'une clé spécifiée à l'aide de la commande cURL suivante.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Remplacez les variables de l'exemple de demande conformément au tableau suivant :
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Obligatoire.</strong> Identificateur unique de la clé racine qui a une politique de rotation existante.</td>
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
        <caption style="caption-side:bottom;">Tableau 1. Variables nécessaires pour créer une politique de rotation avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Une réponse `GET api/v2/keys/{id}/policies` réussie renvoie les détails de la politique associée à votre clé. L'objet JSON suivant montre un exemple de réponse pour une clé racine ayant une politique de rotation existante. 

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    La valeur `interval_month` indique la fréquence de rotation de la clé en mois.

### Création d'une politique de rotation
{: #create-rotation-policy-api}

Créez une politique de rotation pour votre clé racine en soumettant un appel `PUT` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Créez une politique de rotation pour une clé spécifiée en exécutant la commande cURL suivante.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>Obligatoire.</strong> L'identificateur unique de la clé racine pour laquelle vous souhaitez créer une politique de rotation.</td>
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
        <td><varname>rotation_interval</varname></td>
        <td><strong>Obligatoire.</strong> Valeur entière qui détermine l'intervalle de rotation de la clé en mois. Le minimum est <code>1</code> et le maximum est <code>12</code>.        </td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 1. Variables nécessaires pour créer une politique de rotation avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Une réponse `PUT api/v2/keys/{id}/policies` réussie renvoie les détails de la politique associée à votre clé. L'objet JSON suivant montre un exemple de réponse pour une clé racine ayant une politique de rotation existante. 

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### Mise à jour d'une politique de rotation
{: #update-rotation-policy-api}

Mettez à jour la politique existante d'une clé racine en soumettant un appel `PUT` au noeud final suivant.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Extrayez vos données d'authentification et de service](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Remplacez la politique de ration d'une clé spécifiée en exécutant la commande cURL suivante.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
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
        <td><varname>key_ID</varname></td>
        <td><strong>Obligatoire.</strong> Identificateur unique de la clé racine dont vous souhaitez remplacer la politique de rotation.</td>
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
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>Obligatoire.</strong> Valeur entière qui détermine l'intervalle de rotation de la clé en mois. Le minimum est <code>1</code> et le maximum est <code>12</code>.        </td>
      </tr>
        <caption style="caption-side:bottom;">Tableau 1. Variables nécessaires pour créer une politique de rotation avec l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Une réponse `PUT api/v2/keys/{id}/policies` réussie renvoie les détails actualisés de la politique associée à votre clé. L'objet JSON suivant montre un exemple de réponse d'une clé racine avec une politique de rotation mise à jour.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    Les valeurs `interval_month` et `updatedat` sont mises à jour dans les détails de politique de la clé. Si un utilisateur différent met à jour une politique pour une clé que vous avez créée à l'origine, la valeur `updatedby` est également modifiée pour montrer l'identificateur de la personne qui a envoyé la demande.

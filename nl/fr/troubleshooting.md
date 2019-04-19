---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Traitement des incidents
{: #troubleshooting}

Parmi les problèmes généraux liés à l'utilisation de {{site.data.keyword.keymanagementservicefull}}, citons notamment l'indication d'en-têtes ou de données d'identification valides lorsque vous interagissez avec l'API. Dans de nombreux cas, ces problèmes peuvent être résolus en quelques opérations simples.
{: shortdesc}

## Impossible de supprimer mon instance de service Cloud Foundry
{: #unable-to-delete-service}

Lorsque vous tentez de supprimer votre instance de service {{site.data.keyword.keymanagementserviceshort}}, le service ne parvient pas à effectuer la suppression comme prévu.

Dans le tableau de bord {{site.data.keyword.cloud_notm}}, vous accédez à **Services Cloud Foundry**, puis sélectionnez votre instance {{site.data.keyword.keymanagementserviceshort}}. Vous cliquez sur l'icône ⋮ pour afficher la liste des options de l'offre de services, puis cliquez sur **Supprimer le service**.
{: tsSymptoms}

Le service n'est pas supprimé et l'erreur suivant est signalée : 
```
403 Forbidden: This action cannot be completed because you have existing secrets in your Key Protect service. You first need to delete the secrets before you can remove the service.
```
{: screen}

Le 15 décembre 2017, {{site.data.keyword.keymanagementserviceshort}} a cessé d'utiliser des organisations, espaces et rôles Cloud Foundry au profit de groupes de ressources IAM. Vous pouvez désormais mettre à disposition le service {{site.data.keyword.keymanagementserviceshort}} au sein d'un groupe de ressources, sans avoir à spécifier une organisation et un espace Cloud Foundry.
{: tsCauses}

Ces modifications ont affecté le mode de fonctionnement de l'annulation des mises à disposition pour les instances plus anciennes du service. Si vous avez créé votre instance {{site.data.keyword.keymanagementserviceshort}} avant le 28 septembre 2017, il est possible que l'annulation de service ne fonctionne pas comme prévu.

Pour supprimer une ancienne instance de service {{site.data.keyword.keymanagementserviceshort}}, vous devez commencer par supprimer vos clés actuelles à l'aide du noeud final `https://ibm-key-protect.edge.bluemix.net` existant afin d'interagir avec le service {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Pour supprimer vos clés et votre instance de service :

1. Connectez-vous à {{site.data.keyword.cloud_notm}} via l'interface de ligne de commande d'{{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **Remarque :** si la procédure de connexion échoue, exécutez la commande `bx login --sso` pour effectuer une nouvelle tentative. Le paramètre `--sso` est requis quand vous vous connectez avec un ID fédéré. Si cette option est utilisée, accédez au lien figurant dans la sortie de l'interface de ligne de commande pour générer un code d'accès à usage unique.

2. Sélectionnez la région, organisation et espace {{site.data.keyword.cloud_notm}} qui contient votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

    Notez les noms de votre organisation et de votre espace dans la sortie d'interface de ligne de commande. Vous pouvez également exécuter la commande `ibmcloud cf target` pour cibler votre organisation et espace Cloud Foundry.

    ```sh
    ibmcloud cf target -o <nom_organisation> -s <nom_espace>
    ```
    {: codeblock}

3. Extrayez les identificateurs globaux uniques de votre organisation et de votre espace {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud iam org <nom_organisation> --guid
    ibmcloud iam space <nom_espace> --guid
    ```
    {: codeblock}
    Remplacez `<organization_name>` et `<space_name>` par les alias uniques que vous avez affectés à vos organisation et espace. 

4. Extrayez votre jeton d'accès.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. Affichez la liste des clés stockées dans votre instance de service en exécutant la commande cURL suivante.

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Remplacez `<access_token>`, `<organization_GUID>` et `<space_GUID>` par les valeurs extraites aux étapes 3 et 4. 

6. Copiez la valeur d'ID de chaque clé stockée dans votre instance de service.

7. Exécutez la commande cURL suivante pour supprimer définitivement une clé et son contenu.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Remplacez `<access_token>`, `<organization_GUID>`, `<space_GUID>` et `<key_ID>` par les valeurs extraites aux étapes 3 à 5. Exécutez la commande pour chaque clé.    

8. Vérifiez que vos clés ont été supprimées en exécutant la commande cURL suivante.

    ```cURL
    curl -X GET \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Remplacez `<access_token>`, `<organization_GUID>` et `<space_GUID>` par les valeurs extraites aux étapes 3 et 4. 

9. Supprimez votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. Facultatif : vérifiez dans votre tableau de bord {{site.data.keyword.cloud_notm}} que votre instance de service {{site.data.keyword.keymanagementserviceshort}} a été supprimée.

    Vous pouvez également répertorier les services Cloud Foundry disponibles dans l'espace ciblé en exécutant la commande suivante.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## Impossible d'accéder à l'interface utilisateur
{: #unable-to-access-ui}

Lorsque vous accédez à l'interface utilisateur {{site.data.keyword.keymanagementserviceshort}}, celle-ci ne se charge pas comme prévu.

Dans le tableau de bord {{site.data.keyword.cloud_notm}}, sélectionnez votre instance du service {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

L'erreur suivante s'affiche : 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Le 15 décembre 2017, nous avons ajouté de nouvelles fonctions, telles que le [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption), au service {{site.data.keyword.keymanagementserviceshort}}. Vous pouvez désormais mettre à disposition le service {{site.data.keyword.keymanagementserviceshort}} au sein d'un groupe de ressources, sans avoir à spécifier une organisation et un espace Cloud Foundry.
{: tsCauses}

Ces modifications ont affecté l'interface utilisateur pour des instances plus anciennes du service. Si vous avez créé votre instance {{site.data.keyword.keymanagementserviceshort}} avant le 28 septembre 2017, il se peut que l'interface utilisateur ne fonctionne pas comme prévu.

Nous nous employons à résoudre ce problème de notre côté. A titre de solution provisoire, vous pouvez continuer à gérer vos clés à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Vous pouvez utiliser le noeud final `https://ibm-key-protect.edge.bluemix.net` existant pour interagir avec le service {{site.data.keyword.keymanagementserviceshort}}.

**Exemple de demande**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Impossible de créer ou de supprimer des clés
{: #unable-to-create-keys}

Lorsque vous accédez à l'interface utilisateur {{site.data.keyword.keymanagementserviceshort}}, les options d'ajout ou de suppression des clés ne s'affichent pas.

Dans le tableau de bord {{site.data.keyword.cloud_notm}}, sélectionnez votre instance du service {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Une liste de clés est visible, mais les options d'ajout ou de suppression des clés ne s'affichent pas. 

Vous ne disposez pas des droits appropriés pour exécuter des actions {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Vérifiez auprès de votre administrateur qu'il vous a attribué le rôle adéquat dans le groupe de ressources ou l'instance de service applicable. Pour plus d'informations sur les rôles, voir [Rôles et droits](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Aide et support
{: #getting-help}

Si vous rencontrez des problèmes ou si vous avez des questions lors de l'utilisation d'{{site.data.keyword.keymanagementserviceshort}}, vous pouvez consulter {{site.data.keyword.cloud_notm}} ou rechercher des informations ou poser des questions via un forum. Vous pouvez également ouvrir un ticket de demande de service.
{: shortdesc}

Vous pouvez vérifier si {{site.data.keyword.cloud_notm}} est disponible en accédant à la [page de statut ![icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/status?tags=platform,runtimes,services).

Vous pouvez consulter les forums pour voir si d'autres utilisateurs ont rencontré le
même problème. Quand vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.cloud_notm}}.

- Si vous avez des questions d'ordre technique sur {{site.data.keyword.keymanagementserviceshort}}, postez votre question sur le forum [Stack Overflow ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} en lui adjoignant les balises "ibm-cloud" et "key-protect".
- Pour des questions relatives au service et aux instructions de mise en route, utilisez le forum [IBM developerWorks dW Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/key-protect/){: new_window} forum. Incluez les balises "ibm-cloud" et "key-protect".

Pour plus d'informations sur l'utilisation des forums, voir la rubrique [Aide et support![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window}.

Pour plus d'informations sur l'ouverture d'un ticket de demande de service {{site.data.keyword.IBM_notm}}, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir la rubrique expliquant [comment contacter le support![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.

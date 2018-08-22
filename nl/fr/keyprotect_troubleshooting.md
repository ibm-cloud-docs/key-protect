---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

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

# Traitement des incidents
{: #troubleshooting}

Parmi les problèmes généraux liés à l'utilisation de {{site.data.keyword.keymanagementservicefull}}, citons notamment l'indication d'en-têtes ou de données d'identification valides lorsque vous interagissez avec l'API. Dans de nombreux cas, ces problèmes peuvent être résolus en quelques opérations simples.
{: shortdesc}

## Impossible d'accéder à l'interface utilisateur
{: #unabletoaccessUI}

Lorsque vous accédez à l'interface utilisateur {{site.data.keyword.keymanagementserviceshort}}, celle-ci ne se charge pas comme prévu. 

Dans le tableau de bord {{site.data.keyword.cloud_notm}}, sélectionnez votre instance du service {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

L'erreur suivante s'affiche :  
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Le 15 décembre 2017, nous avons ajouté de nouvelles fonctions, telles que le [chiffrement d'enveloppe](/docs/services/keymgmt/concepts/keyprotect_envelope.html), au service {{site.data.keyword.keymanagementserviceshort}}. Vous pouvez désormais mettre à disposition le service {{site.data.keyword.keymanagementserviceshort}} de manière globale, sans avoir à spécifier une organisation et un espace Cloud Foundry.
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
{: #unabletomanagekeys}

Lorsque vous accédez à l'interface utilisateur {{site.data.keyword.keymanagementserviceshort}}, les options d'ajout ou de suppression des clés ne s'affichent pas. 

Dans le tableau de bord {{site.data.keyword.cloud_notm}}, sélectionnez votre instance du service {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Une liste de clés est visible, mais les options d'ajout ou de suppression des clés ne s'affichent pas.  

Vous ne disposez pas des droits appropriés pour exécuter des actions {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Vérifiez auprès de votre administrateur qu'il vous a attribué le rôle adéquat dans le groupe de ressources ou l'instance de service applicable. Pour plus d'informations sur les rôles, voir [Rôles et droits](/docs/services/keymgmt/keyprotect_manage_access.html#roles).
{: tsResolve}

## Aide et support
{: #getting_help}

Si vous rencontrez des problèmes ou si vous avez des questions lors de l'utilisation d'{{site.data.keyword.keymanagementserviceshort}}, vous pouvez consulter {{site.data.keyword.cloud_notm}} ou rechercher des informations ou poser des questions via un forum. Vous pouvez également ouvrir un ticket de demande de service.
{: shortdesc}

Vous pouvez vérifier si {{site.data.keyword.cloud_notm}} est disponible en accédant à la [page de statut ![icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/status?tags=platform,runtimes,services).

Vous pouvez consulter les forums pour voir si d'autres utilisateurs ont rencontré le
même problème. Quand vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.cloud_notm}}.

- Si vous avez des questions d'ordre technique sur {{site.data.keyword.keymanagementserviceshort}}, postez votre question sur le forum [Stack Overflow ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} en lui adjoignant les balises "ibm-cloud" et "key-protect".
- Pour des questions relatives au service et aux instructions de mise en route, utilisez le forum [IBM developerWorks dW Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window} forum. Incluez les balises "ibm-cloud" et "key-protect".

Pour plus d'informations sur l'utilisation des forums, voir la rubrique expliquant [comment obtenir de l'aide ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}.

Pour plus d'informations sur l'ouverture d'un ticket de demande de service {{site.data.keyword.IBM_notm}}, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir la rubrique expliquant [comment contacter le support![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.

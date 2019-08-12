---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# Traitement des incidents
{: #troubleshooting}

Parmi les problèmes généraux liés à l'utilisation de {{site.data.keyword.keymanagementservicefull}}, citons notamment l'indication d'en-têtes ou de données d'identification valides lorsque vous interagissez avec l'API. Dans de nombreux cas, ces problèmes peuvent être résolus en quelques opérations simples.
{: shortdesc}

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

Vous pouvez vérifier si {{site.data.keyword.cloud_notm}} est disponible en accédant à la [page de statut](https://{DomainName}/status?tags=platform,runtimes,services){: external}.

Vous pouvez consulter les forums pour voir si d'autres utilisateurs ont rencontré le
même problème. Quand vous utilisez les forums pour poser une question, prenez soin d'étiqueter cette dernière de façon à ce qu'elle soit vue par les équipes de développement {{site.data.keyword.cloud_notm}}.

- Si vous avez des questions d'ordre technique sur {{site.data.keyword.keymanagementserviceshort}}, postez votre question sur le forum [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} en lui joignant les étiquettes "ibm-cloud" et "key-protect". 
- Pour les questions relatives aux instructions liées aux services et à la mise en route, utilisez le forum [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external}. Incluez les balises "ibm-cloud" et "key-protect".

Voir [Aide et support](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external} pour plus d'informations sur l'utilisation des forums.

Pour plus d'informations sur l'ouverture d'un ticket de demande de service {{site.data.keyword.IBM_notm}}, sur les niveaux de support disponibles ou les niveaux de gravité des tickets, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support){: external}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# Nouveautés
{: #releases}

Tenez-vous informé des nouvelles fonctions qui sont disponibles pour {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

## Juin 2019
{: #june-2019}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} ajoute la prise en charge pour {{site.data.keyword.at_full_notm}}
{: #added-at-logdna-support}
Nouveau à compter du 22/06/2019

Vous pouvez désormais surveiller des appels API vers le service {{site.data.keyword.keymanagementserviceshort}} en utilisant {{site.data.keyword.at_full_notm}}. 

Pour en savoir plus sur la surveillance de l'activité {{site.data.keyword.keymanagementserviceshort}}, voir [Evénements Activity Tracker](/docs/services/key-protect?topic=key-protect-at-events).

## Mai 2019
{: #may-2019}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} met à jour HSM vers FIPS 140-2 Niveau 3
{: #upgraded-hsms}
Nouveau à compter du 22/05/2019

{{site.data.keyword.keymanagementserviceshort}} utilise désormais {{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 pour le stockage et les opérations cryptographiques. Vos clés {{site.data.keyword.keymanagementserviceshort}} sont stockées sur du matériel anti-fraude conforme à FIPS 140-2 Niveau 3 pour toutes les régions.  

Pour en savoir plus sur les fonctions et avantages d'{{site.data.keyword.cloud_notm}} HSM 7.0, consultez la [page produit](https://www.ibm.com/cloud/hardware-security-module){: external}.

### Fin de support : Instances de service {{site.data.keyword.keymanagementserviceshort}} basé Cloud Foundry 
{: #legacy-service-eol}
Nouveau à compter du 15/05/2019

Le service {{site.data.keyword.keymanagementserviceshort}} existant, basé sur Cloud Foundry, a atteint sa fin de support le 15 mai 2019. Les instances de service {{site.data.keyword.keymanagementserviceshort}} gérées par Cloud Foundry ne sont plus prises en charge et les mises à jour apportées au service existant ne seront plus fournies. Les clients sont invités à utiliser des instances de service {{site.data.keyword.keymanagementserviceshort}} gérées par IAM pour bénéficier des fonctions les plus récentes pour le service.

Si vous avez créé votre instance de service {{site.data.keyword.keymanagementserviceshort}} après les 15 décembre 2017, elle est gérée par IAM et n'est pas affectée par ce changement. Si vous avez d'autres questions, contactez Terry Mosbaugh à l'adresse [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).

Besoin de retirer une instance de service {{site.data.keyword.keymanagementserviceshort}} de la section **Services Cloud Foundry** de votre liste de ressources {{site.data.keyword.cloud_notm}} ? Vous pouvez nous contacter depuis le [centre de support](https://{DomainName}/unifiedsupport/cases/add) en envoyant une demande de retrait de l'entrée de votre vue de console.
{: tip}

## Mars 2019
{: #mar-2019}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} ajoute une prise en charge des rotations de clés basées sur des politiques
{: #added-support-policy-key-rotation}
Nouveau depuis le 22-03-2019

Vous pouvez maintenant utiliser {{site.data.keyword.keymanagementserviceshort}} pour associer une politique de rotation pour vos clés racine.

Pour plus d'informations, voir [Définition d'une politique de rotation](/docs/services/key-protect?topic=key-protect-set-rotation-policy). Pour en savoir plus sur vos options de rotation de clés dans {{site.data.keyword.keymanagementserviceshort}}, consultez la section [Comparaison de vos options de rotation de clés](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Ajout : {{site.data.keyword.keymanagementserviceshort}} ajoute une prise en charge bêta des clés de transport
Nouveau depuis le 20-03-2019

Activez l'importation sécurisée des clés de chiffrement vers le cloud en créant des clés de chiffrement de transport pour votre service {{site.data.keyword.keymanagementserviceshort}}.

Pour plus d'informations, voir [Apport de vos clés de chiffrement au cloud](/docs/services/key-protect?topic=key-protect-importing-keys).

Les clés de transport sont actuellement une fonction bêta. Les fonctions bêta peuvent changer à tout moment et les mises à jour futures peuvent introduire des changements qui sont incompatibles avec la dernière version.
{: important}

## Février 2019
{: #feb-2019}

### Modification : Instances de service {{site.data.keyword.keymanagementserviceshort}} existantes
{: #changed-legacy-service-instances}

Nouveau depuis le 13-02-2019

Les instances de service {{site.data.keyword.keymanagementserviceshort}} mises à disposition avant le 15 décembre 2017 s'exécutent sur une infrastructure existante basée sur Cloud Foundry. Ce service {{site.data.keyword.keymanagementserviceshort}} existant sera retiré le 15 mai 2019.

**Conséquences pour l'utilisateur**

Si vous avez des clés de production actives dans une instance de service {{site.data.keyword.keymanagementserviceshort}} antérieure à cette date, veillez à faire migrer les clés vers une nouvelle instance de service avant le 15 mai 2019 afin d'éviter de perdre l'accès à vos données chiffrées. Vous pouvez vérifier si vous utilisez une instance existante en accédant à votre liste de ressources depuis la [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/). Si votre instance de service {{site.data.keyword.keymanagementserviceshort}} apparaît dans la section **Services Cloud Foundry** de la liste des ressources {{site.data.keyword.cloud_notm}} ou si vous utilisez le noeud final de l'API `https://ibm-key-protect.edge.bluemix.net` pour cibler des opérations pour le service, vous utilisez une instance existante de {{site.data.keyword.keymanagementserviceshort}}. Après le 15 mai 2019, le noeud final existant ne sera plus accessible et vous ne serez plus en mesure de cibler le service pour vos opérations.

Vous avez besoin d'aide pour la migration de vos clés de chiffrement dans une nouvelle instance de service ? Pour connaître la procédure à suivre, consultez la section [Client de migration dans GitHub](https://github.com/IBM-Cloud/kms-migration-client){: external}. Si vous avez des questions supplémentaires sur le statut de vos clés ou sur le processus de migration, veuillez contacter Terry Mosbaugh à l'adresse [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Décembre 2018
{: #dec-2018}

### Modification : Noeuds finaux de l'API {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-endpoints}

Nouveau depuis le 19-12-2018

Pour être en ligne avec la nouvelle expérience unifiée d'{{site.data.keyword.cloud_notm}}, {{site.data.keyword.keymanagementserviceshort}} a mis à jour les URL de base de ses API de service.

Vous pouvez désormais mettre à jour vos applications pour référencer les nouveaux noeuds finaux `cloud.ibm.com`.

- `keyprotect.us-south.bluemix.net` est maintenant `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` est maintenant `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` est maintenant `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` est maintenant `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` est maintenant `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` est maintenant `jp-tok.kms.cloud.ibm.com` 

Actuellement, les deux URL sont prises en charge pour chaque noeud final de service régional. 

## Octobre 2018
{: #oct-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Tokyo
{: #added-tokyo-region}

Nouveau depuis le 31-10-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Tokyo. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

### Ajout : plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #added-cli-plugin}

Nouveau depuis le 02-10-2018

Vous pouvez désormais utiliser le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}} pour gérer les clés de votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

Pour savoir comment installer le plug-in, voir [Configuration de l'interface de ligne de commande](/docs/services/key-protect?topic=key-protect-set-up-cli). Pour plus d'informations sur l'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}, voir la [documentation de référence de l'interface de ligne de commande](/docs/services/key-protect?topic=key-protect-cli-reference).

## Septembre 2018
{: #sept-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge la rotation des clés à la demande.
{: #added-key-rotation}

Nouveau depuis le 28-09-2018

Vous pouvez désormais utiliser {{site.data.keyword.keymanagementserviceshort}} pour effectuer une rotation de vos clés racine à la demande.

Pour plus d'informations, voir [Rotation des clés](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Ajout : tutoriel sur la sécurité de bout en bout pour votre application en cloud.
{: #added-security-tutorial}

Nouveau depuis le 14-09-2018

Vous recherchez des exemples pour vous aider à chiffrer du contenu de compartiments de stockage avec vos propres clés de chiffrement ?

Vous pouvez maintenant vous exercer à sécuriser de bout en bout votre application en cloud en suivant le [nouveau tutoriel](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

Pour plus d'informations, [voir l'exemple d'application dans GitHub](https://github.com/IBM-Cloud/secure-file-storage){: external}.

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Washington DC
{: #added-wdc-region}

Nouveau depuis le 10-09-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Washington DC. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Août 2018
{: #aug-2018}

### Modification : URL de la documentation de l'API {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-doc-url}

Nouveau depuis le 28-08-2018

La documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} a été déplacée. 

Vous pouvez désormais y accéder à l'adresse
https://{DomainName}/apidocs/key-protect. 

## Mars 2018
{: #mar-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Francfort.
{: #added-frankfurt-region}

Nouveau depuis le 21-03-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Francfort. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Janvier 2018
{: #jan-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Sydney.
{: #added-sydney-region}

Nouveau depuis le 31-01-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Sydney. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Décembre 2017
{: #dec-2017}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge la fonction Bring Your Own Key (BYOK).
{: #added-byok-support}

Nouveau depuis le 15-12-2017

Désormais, {{site.data.keyword.keymanagementserviceshort}} prend en charge la fonction Bring Your Own Key (BYOK) et le chiffrement géré par le client.

- Ajout de [clés racine](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), aussi appelées clés racine client (CRK), telles que des ressources primaires, au service. 
- Activation du [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) pour les compartiments {{site.data.keyword.cos_full_notm}}.

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Londres.
{: #added-london-region}

Nouveau depuis le 15-12-2017

Désormais, {{site.data.keyword.keymanagementserviceshort}} est disponible dans la région de Londres. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

### Modification : rôles {{site.data.keyword.iamshort}}
{: #changed-iam-roles}

Nouveau depuis le 15-12-2017

Les rôles {{site.data.keyword.iamshort}}, qui déterminent les actions que vous pouvez effectuer sur les ressources {{site.data.keyword.keymanagementserviceshort}}, ont changé.

- `Administrateur` devient `Responsable`
- `Editeur` devient `Auteur`
- `Afficheur` devient `Lecteur`

Pour plus d'informations, voir [Gestion de l'accès utilisateur](/docs/services/key-protect?topic=key-protect-manage-access).

## Septembre 2017
{: #sept-2017}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge Cloud IAM.
{: #added-iam-support}

Nouveau depuis le 19-09-2017

Désormais, vous pouvez utiliser {{site.data.keyword.iamshort}} afin de définir et de gérer les règles d'accès pour vos ressources {{site.data.keyword.keymanagementserviceshort}}.

Pour plus d'informations, voir [Gestion de l'accès utilisateur](/docs/services/key-protect?topic=key-protect-manage-access).

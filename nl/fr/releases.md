---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# Nouveautés
{: #releases}

Tenez-vous informé des nouvelles fonctions qui sont disponibles pour {{site.data.keyword.keymanagementservicefull}}. 

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
Nouveau depuis le 13-02-2019

Les instances de service {{site.data.keyword.keymanagementserviceshort}} mises à disposition avant le 15 décembre 2017 s'exécutent sur une infrastructure existante basée sur Cloud Foundry. Ce service {{site.data.keyword.keymanagementserviceshort}} existant sera retiré le 15 mai 2019.

**Conséquences pour l'utilisateur**

Si vous avez des clés de production actives dans une instance de service {{site.data.keyword.keymanagementserviceshort}} antérieure à cette date, veillez à faire migrer les clés vers une nouvelle instance de service avant le 15 mai 2019 afin d'éviter de perdre l'accès à vos données chiffrées. Vous pouvez vérifier si vous utilisez une instance existante en parcourant votre liste de ressources dans la console [{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/). Si votre instance de service {{site.data.keyword.keymanagementserviceshort}} apparaît dans la section **Services Cloud Foundry** de la liste des ressources {{site.data.keyword.cloud_notm}} ou si vous utilisez le noeud final de l'API `https://ibm-key-protect.edge.bluemix.net` pour cibler des opérations pour le service, vous utilisez une instance existante de {{site.data.keyword.keymanagementserviceshort}}. Après le 15 mai 2019, le noeud final existant ne sera plus accessible et vous ne serez plus en mesure de cibler le service pour vos opérations.

Vous avez besoin d'aide pour la migration de vos clés de chiffrement dans une nouvelle instance de service ? Pour connaître la procédure à suivre, consultez la section [Client de migration dans GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM-Cloud/kms-migration-client){: new_window}. Si vous avez des questions supplémentaires sur le statut de vos clés ou sur le processus de migration, veuillez contacter Terry Mosbaugh à l'adresse [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Décembre 2018
{: #dec-2018}

### Modification : Noeuds finaux de l'API {{site.data.keyword.keymanagementserviceshort}}
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
Nouveau depuis le 31-10-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Tokyo. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

### Ajout : plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
Nouveau depuis le 02-10-2018

Vous pouvez désormais utiliser le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}} pour gérer les clés de votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

Pour savoir comment installer le plug-in, voir [Configuration de l'interface de ligne de commande](/docs/services/key-protect?topic=key-protect-set-up-cli). Pour plus d'informations sur l'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}, voir la [documentation de référence de l'interface de ligne de commande](/docs/services/key-protect?topic=key-protect-cli-reference).

## Septembre 2018
{: #sept-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge la rotation des clés à la demande.
Nouveau depuis le 28-09-2018

Vous pouvez désormais utiliser {{site.data.keyword.keymanagementserviceshort}} pour effectuer une rotation de vos clés racine à la demande.

Pour plus d'informations, voir [Rotation des clés](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Ajout : tutoriel sur la sécurité de bout en bout pour votre application en cloud.
Nouveau depuis le 14-09-2018

Vous recherchez des exemples pour vous aider à chiffrer du contenu de compartiments de stockage avec vos propres clés de chiffrement ?

Vous pouvez maintenant vous exercer à sécuriser de bout en bout votre application en cloud en suivant le [nouveau tutoriel](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

Pour plus d'informations, [voir l'exemple d'application dans GitHub ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}.

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Washington DC 
Nouveau depuis le 10-09-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Washington DC. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Août 2018
{: #aug-2018}

### Modification : URL de la documentation de l'API {{site.data.keyword.keymanagementserviceshort}}
Nouveau depuis le 28-08-2018

La documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} a été déplacée. 

Vous pouvez désormais y accéder à l'adresse
https://{DomainName}/apidocs/key-protect. 

## Mars 2018
{: #mar-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Francfort.
Nouveau depuis le 21-03-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Francfort. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Janvier 2018
{: #jan-2018}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Sydney.
Nouveau depuis le 31-01-2018

Vous pouvez désormais créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans la région de Sydney. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

## Décembre 2017
{: #dec-2017}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge la fonction Bring Your Own Key (BYOK).
Nouveau depuis le 15-12-2017

Désormais, {{site.data.keyword.keymanagementserviceshort}} prend en charge la fonction Bring Your Own Key (BYOK) et le chiffrement géré par le client.

- Ajout de [clés racine](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), aussi appelées clés racine client (CRK), telles que des ressources primaires, au service. 
- Activation du [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) pour les compartiments {{site.data.keyword.cos_full_notm}}.

### Ajout : {{site.data.keyword.keymanagementserviceshort}} s'étend à la région de Londres.
Nouveau depuis le 15-12-2017

Désormais, {{site.data.keyword.keymanagementserviceshort}} est disponible dans la région de Londres. 

Pour plus d'informations, voir [Régions et emplacements](/docs/services/key-protect?topic=key-protect-regions).

### Modification : rôles {{site.data.keyword.iamshort}}
Nouveau depuis le 15-12-2017

Les rôles {{site.data.keyword.iamshort}}, qui déterminent les actions que vous pouvez effectuer sur les ressources {{site.data.keyword.keymanagementserviceshort}}, ont changé.

- `Administrateur` devient `Responsable`
- `Editeur` devient `Auteur`
- `Afficheur` devient `Lecteur`

Pour plus d'informations, voir [Gestion de l'accès utilisateur](/docs/services/key-protect?topic=key-protect-manage-access).

## Septembre 2017
{: #sept-2017}

### Ajout : {{site.data.keyword.keymanagementserviceshort}} prend en charge Cloud IAM.
Nouveau depuis le 19-09-2017

Désormais, vous pouvez utiliser {{site.data.keyword.iamshort}} afin de définir et de gérer les règles d'accès pour vos ressources {{site.data.keyword.keymanagementserviceshort}}.

Pour plus d'informations, voir [Gestion de l'accès utilisateur](/docs/services/key-protect?topic=key-protect-manage-access).

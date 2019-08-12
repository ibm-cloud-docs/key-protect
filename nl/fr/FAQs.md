---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# Foire aux questions
{: #faqs}

Vous pouvez utiliser la foire aux questions ci-dessous pour vous aider à utiliser {{site.data.keyword.keymanagementservicelong}}.

## Comment fonctionne la tarification de {{site.data.keyword.keymanagementserviceshort}} ?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} offre un [plan graduel](https://{DomainName}/catalog/services/key-protect) sans frais aux utilisateurs ayant besoin de 20 clés ou moins. Vous pouvez avoir 20 clés gratuites par compte {{site.data.keyword.cloud_notm}}. Si votre équipe a besoin de plusieurs instances de {{site.data.keyword.keymanagementserviceshort}}, {{site.data.keyword.cloud_notm}} ajoute vos clés actives dans toutes les instances du compte, puis applique la tarification. 

## Qu'est-ce qu'une clé de chiffrement active ?
{: #what-is-active-encryption-key}
{: faq}

Lorsque vous importez des clés de chiffrement dans {{site.data.keyword.keymanagementserviceshort}} ou lorsque vous utilisez {{site.data.keyword.keymanagementserviceshort}} pour générer des clés depuis ses modules de sécurité matériels, ces clés deviennent des clés _actives_. La tarification est basée sur toutes les clés actives d'un compte {{site.data.keyword.cloud_notm}}. 

## Comment dois-je regrouper et gérer mes clés ?
{: #how-to-group-keys}
{: faq}

Du point de vue de la tarification, la meilleure façon d'utiliser {{site.data.keyword.keymanagementserviceshort}} est de créer un nombre limité de clés racine et de les utiliser pour chiffrer les clés de chiffrement de données qui sont créées par un service de données de cloud ou une application externe. 

Pour en savoir plus sur l'utilisation des clés racine pour protéger les clés de chiffrement de données, consultez la section [Protection des données avec le chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Qu'est-ce qu'une clé racine ?
{: #what-is-root-key}
{: faq}

Les clés racine constituent les ressources principales de {{site.data.keyword.keymanagementserviceshort}}. Il s'agit de clés d'encapsulage de clés symétriques qui sont utilisées comme racines de confiance pour protéger d'autres clés qui sont stockées dans un service de données avec le [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption). Dans {{site.data.keyword.keymanagementserviceshort}}, vous pouvez créer, stocker et gérer le cycle de vie des clés racine pour obtenir un contrôle total des autres clés stockées dans le cloud. 

## Qu'est-ce que le chiffrement d'enveloppe ?
{: #what-is-envelope-encryption}
{: faq}

Le chiffrement d'enveloppe est une procédure qui consiste à chiffrer des données à l'aide d'une _clé de chiffrement de données_, puis à chiffrer la clé de chiffrement de données avec un _clé de chiffrement de clé_ hautement sécurisée.  Vos données sont protégées au repos grâce à plusieurs niveaux de chiffrement. Pour savoir comment activer le chiffrement d'enveloppe pour vos ressources {{site.data.keyword.cloud_notm}}, consultez la section [Intégration de services](/docs/services/key-protect?topic=key-protect-integrate-services).

## Quelle est la longueur maximale d'un nom de clé ?
{: #key-names}
{: faq}

Vous pouvez utiliser un nom de clé de 90 caractères au maximum.

## Puis-je stocker des informations personnelles en tant que métadonnées pour mes clés ?
{: #personal-data}
{: faq}

Pour protéger la confidentialité de vos données personnelles, ne stockez pas d'informations permettant de vous identifier personnellement comme métadonnées pour vos clés. Par exemple, votre nom, votre adresse, votre numéro de téléphone, votre adresse e-mail ou toute autre information permettant de vous identifier, de vous contacter ou de vous localiser, ainsi que vos clients et toute autre personne.

Vous êtes responsable d'assurer la sécurité de toutes les informations que vous stockez en tant que métadonnées pour les ressources {{site.data.keyword.keymanagementserviceshort}} et les clés de chiffrement. Pour obtenir d'autres exemples de données personnelles, veuillez consulter la section 2.2 du document [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

## Peut-on créer des clés dans une région et les utiliser dans une autre région ?
{: #keys-across-regions}
{: faq}

Vos clés de chiffrement sont confinées dans la région où vous les créez. {{site.data.keyword.keymanagementserviceshort}} ne les copie ou exporte pas vers d'autres régions.

## Comment puis-je contrôler qui a accès aux clés ?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} prend en charge un système de contrôle d'accès centralisé, régi par {{site.data.keyword.iamlong}}, afin de vous aider à gérer les utilisateurs et les accès pour vos clés de chiffrement. Si vous êtes l'administrateur de sécurité de votre service, vous pouvez affecter des [rôles Cloud IAM qui correspondent à des droits spécifiques de {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-manage-access#roles), que vous souhaitez accorder aux membres de votre équipe.

## Comment puis-je surveiller les appels d'API effectués vers {{site.data.keyword.keymanagementserviceshort}} ?
{: faq}

Vous pouvez utiliser le service {{site.data.keyword.cloudaccesstrailfull_notm}} pour savoir comment les utilisateurs et les applications interagissent avec votre instance de service {{site.data.keyword.keymanagementserviceshort}}. Par exemple, lorsque vous créez, importez, supprimez ou lisez une clé dans {{site.data.keyword.keymanagementserviceshort}}, un événement {{site.data.keyword.cloudaccesstrailshort}} est généré. Ces événements sont transmis automatiquement au service {{site.data.keyword.cloudaccesstrailshort}} dans la région dans laquelle le service {{site.data.keyword.keymanagementserviceshort}} a été mis à disposition.

Pour en savoir plus, veuillez consultez la section [Evénements Activity Tracker](/docs/services/key-protect?topic=key-protect-at-events).

## Que se passe-t-il si je supprime une clé ?
{: #key-destruction}
{: faq}

Lorsque vous supprimez une clé, le service marque la clé comme supprimée et la clé passe à l'état _Détruit_. Les clés à l'état détruit ne sont plus récupérables et les services de cloud qui utilisent la clé ne peuvent plus déchiffrer les données qui lui sont associées. Vos données restent dans ces services dans leur forme chiffrée. Les métadonnées associées à une clé, comme le nom et l'historique des transitions de la clé, sont conservées dans la base de données {{site.data.keyword.keymanagementserviceshort}}. 

Avant de supprimer une clé, assurez-vous que vous n'avez plus besoin d'accéder aux données qui lui sont associées. Cette action n'est pas réversible.

## Que se passe-t-il si j'ai besoin d'annuler l'accès à mon instance de service ?
{: #deprovision-service}
{: faq}

Si vous décidez de ne plus utiliser {{site.data.keyword.keymanagementserviceshort}}, vous devez supprimer toutes les clés restantes de votre instance de service avant de pouvoir annuler l'accès au service. Après avoir supprimé votre instance de service, {{site.data.keyword.keymanagementserviceshort}} utilise le [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption) pour chiffrer toutes les données associées à l'instance de service. 


---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sécurité et conformité
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} vous offre des stratégies de sécurité des données pour répondre à vos besoins de conformité et assurer la sécurité et la protection de vos données dans le cloud.
{: shortdesc}

## Sécurité et chiffrement des données
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} utilise les [{{site.data.keyword.cloud_notm}} modules de sécurité matériels (HSMs) d'![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/hardware-security-module) pour générer du matériel de clé géré par le fournisseur et effectuer des opérations de [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption). Les modules de sécurité matériels sont des dispositifs matériels inviolables qui stockent et utilisent des clés cryptographiques sans exposer les clés à l'extérieur d'une frontière cryptographique.

L'accès au service se fait via HTTPS, et la communication interne de {{site.data.data.keyword.keymanagementserviceshort}} utilise le protocole Transport Layer Security (TLS) 1.2 pour chiffrer les données en transit.

Pour en savoir plus sur la façon dont {{site.data.keyword.keymanagementserviceshort}} répond à vos exigences de conformité, consultez [Conformité de la plateforme et des services](/docs/overview?topic=overview-security#compliancetable).

## Suppression des données
{: #data-deletion}

Lorsque vous supprimez une clé, le service marque la clé comme supprimée et la clé passe à l'état _Détruit_. Les clés à l'état détruit ne sont plus récupérables et les services de cloud qui utilisent la clé ne peuvent plus déchiffrer les données associées à cette clé. Vos données restent dans ces services dans leur forme chiffrée. Les métadonnées associées à une clé, comme le nom et l'historique des transitions de la clé, sont conservées dans la base de données de {{site.data.keyword.keymanagementserviceshort}}. 

Supprimer une clé dans {{site.data.data.keyword.keymanagementserviceshort}} est une opération destructive. Gardez à l'esprit qu'après avoir supprimé une clé, l'action ne peut pas être annulée. Toutes les données associées à la clé seront immédiatement perdues au moment où la clé sera supprimée. Avant de supprimer une clé, vérifiez les données associées à la clé et assurez-vous que vous n'en avez plus besoin. Ne supprimez pas une clé qui protège activement des données dans vos environnements de production. 

Pour vous aider à déterminer quelles données sont protégées par une clé, vous pouvez voir comment votre instance de service {{site.data.keyword.keymanagementserviceshort}} est mappée à vos services de cloud existants en exécutant `ibmcloud iam authorization-policies`. Pour en savoir plus sur l'affichage des autorisations de service dans la console, voir [Octroi d'accès entre services](/docs/iam?topic=iam-serviceauth).
{: note}

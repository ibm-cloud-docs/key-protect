---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sécurité et conformité
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} vous offre des stratégies de sécurité des données pour répondre à vos besoins de conformité et assurer la sécurité et la protection de vos données dans le cloud.
{: shortdesc}

## Préparation en matière de sécurité
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} assure la sécurité en appliquant des règles de sécurité respectant les meilleurs pratiques d'{{site.data.keyword.IBM_notm}} en matière de systèmes, de réseau et d'ingénierie sécurisée.  

Pour plus d'informations sur les contrôles de sécurité dans {{site.data.keyword.cloud_notm}}, voir [Comment savoir si mes données sont protégées ?](/docs/overview?topic=overview-security#security).
{: tip}

### chiffrement de données
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} utilise des [modules de sécurité matériels (HSM) {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/hardware-security-module){: external} pour générer du matériel de clé géré par le fournisseur et effectuer des opérations de [chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption). Les modules de sécurité matériels sont des dispositifs matériels inviolables qui stockent et utilisent des clés cryptographiques sans exposer les clés à l'extérieur d'une frontière cryptographique.

L'accès au service se fait via HTTPS, et la communication interne de {{site.data.data.keyword.keymanagementserviceshort}} utilise le protocole Transport Layer Security (TLS) 1.2 pour chiffrer les données en transit.

### Suppression des données
{: #data-deletion}

Lorsque vous supprimez une clé, le service marque la clé comme supprimée et la clé passe à l'état _Détruit_. Les clés à l'état détruit ne sont plus récupérables et les services de cloud qui utilisent la clé ne peuvent plus déchiffrer les données associées à cette clé. Vos données restent dans ces services dans leur forme chiffrée. Les métadonnées associées à une clé, comme le nom et l'historique des transitions de la clé, sont conservées dans la base de données de {{site.data.keyword.keymanagementserviceshort}}. 

Supprimer une clé dans {{site.data.data.keyword.keymanagementserviceshort}} est une opération destructive. Gardez à l'esprit qu'après avoir supprimé une clé, l'action ne peut pas être annulée. Toutes les données associées à la clé seront immédiatement perdues au moment où la clé sera supprimée. Avant de supprimer une clé, vérifiez les données associées à la clé et assurez-vous que vous n'en avez plus besoin. Ne supprimez pas une clé qui protège activement des données dans vos environnements de production. 

Pour vous aider à déterminer quelles données sont protégées par une clé, vous pouvez voir comment votre instance de service {{site.data.keyword.keymanagementserviceshort}} est mappée à vos services de cloud existants en exécutant `ibmcloud iam authorization-policies`. Pour en savoir plus sur l'affichage des autorisations de service dans la console, voir [Octroi d'accès entre services](/docs/iam?topic=iam-serviceauth).
{: note}

## Préparation en matière de conformité
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} satisfaits les contrôles en matière de normes de conformité globale, régionale et de l'industrie, notamment RGPD, HIPAA, et ISO 27001/27017/27018, ainsi que d'autres. 

Pour la liste complète des certifications de conformité d'{{site.data.keyword.cloud_notm}}, voir [Conformité sur l'{{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### Support dans l'Union européenne
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} possède des contrôles supplémentaires mis en place afin de protéger vos ressources {{site.data.keyword.keymanagementserviceshort}} dans l'Union européenne (UE).  

Si vous utilisez des ressources {{site.data.keyword.keymanagementserviceshort}} à Francfort, région Allemagne, pour traiter des données à caractère personnel pour des citoyens européens, vous pouvez activer le paramètre Support dans l'Union européenne pour votre compte {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [Activation du paramètre Support dans l'Union européenne](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) et [Demande de support pour des ressources dans l'Union européenne](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### Règlement général sur la protection des données (RGPD)
{: #gdpr}

Le RGPD vise à créer un cadre légal de protection des données harmonisé à travers l'Union européenne et à restituer aux citoyens le contrôle de leurs données personnelles, tout en imposant des règles strictes sur les entités hébergeant et traitant ces données, où que ce soit dans le monde.

{{site.data.keyword.IBM_notm}} s'est engagée à fournir à ses clients et aux partenaires commerciaux {{site.data.keyword.IBM_notm}} des solutions de confidentialité des données, de sécurité et de gouvernance innovantes pour les aider à se conformer à la réglementation RGPD. 

Pour garantir la conformité au RGPD de vos ressources {{site.data.keyword.keymanagementserviceshort}}, [activez le paramètre Support dans l'Union européenne](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) pour votre compte {{site.data.keyword.cloud_notm}}. Pour plus d'informations sur la façon dont {{site.data.keyword.keymanagementserviceshort}} traite et protège les données à caractère personnel, consultez les addenda suivants. 

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### Prise en charge de la loi HIPAA 
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} satisfait les contrôles de la loi américaine Health Insurance Portability and Accountability Act (HIPAA) afin de garantir la protection des renseignements médicaux protégés (PHI). 

Si vous ou votre société êtes une entité couverte telle que définie par l'HIPAA, vous pouvez activer le paramètre Loi HIPAA prise en charge pour votre compte {{site.data.keyword.cloud_notm}}. Pour plus d'informations, voir [Activation du paramètre de prise en charge HIPAA](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} est certifié ISO 27001, 27017, 27018. Vous pouvez consultez les certifications de conformité depuis la page [Conformité sur l'{{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}. 

### SOC 2 Type 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} est certifié SOC 2 Type 1. Pour des informations sur la demande d'un rapport {{site.data.keyword.cloud_notm}} SOC 2, voir [Conformité sur l'{{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

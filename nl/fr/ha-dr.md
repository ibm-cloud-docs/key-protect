---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: Key Protect availability, Key Protect disaster recovery

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

# Haute disponibilité et reprise après incident
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} est un service régional hautement disponible doté de fonctions automatiques qui vous aident à maintenir vos applications sûres et opérationnelles.
{: shortdesc}

Utilisez cette page pour en savoir plus sur les stratégies de reprise après incident et de disponibilité de {{site.data.keyword.keymanagementserviceshort}}.

## Emplacements, location et disponibilité
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} est un service régional multi-locataire. 

Vous pouvez créer des ressources {{site.data.keyword.keymanagementserviceshort}} dans l'une des [régions {{site.data.keyword.cloud_notm}} prises en charge](/docs/services/key-protect/regions.html), c'est-à-dire les zones géographiques où vos demandes {{site.data.keyword.keymanagementserviceshort}} sont prises en charge et traitées. Chaque région {{site.data.keyword.cloud_notm}} contient [plusieurs zones de disponibilité ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/) afin de répondre aux besoins locaux de la région en matière d'accès, de temps d'attente et de sécurité.

Au moment de planifier votre stratégie de chiffrement des données au repos avec {{site.data.keyword.cloud_notm}}, souvenez-vous qu'en mettant à disposition {{site.data.keyword.keymanagementserviceshort}} dans une région aussi proche que possible de chez vous, vous aurez plus de chances d'obtenir des connexions rapides et fiables pour interagir avec les API {{site.data.keyword.keymanagementserviceshort}}. Choisissez une région spécifique si les utilisateurs, les applications ou les services dépendant d'une ressource {{site.data.keyword.keymanagementserviceshort}} sont tous concentrés autour d'un même secteur. N'oubliez pas que les utilisateurs et les services trop éloignés de la région choisie auront peut-être à subir des temps de latence pénalisants. 

Vos clés de chiffrement sont confinées dans la région où vous les créez. {{site.data.keyword.keymanagementserviceshort}} ne les copie ou exporte pas vers d'autres régions.
{: note}

## Reprise après incident
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} met en oeuvre un service régional de reprise après incident avec un objectif de temps de reprise (RTO) d'une heure. Le service obéit aux exigences d'{{site.data.keyword.cloud_notm}} en matière de planification et de reprise après incident. Pour plus d'informations, consultez [Reprise après incident](/docs/overview/zero_downtime.html#disaster-recovery).



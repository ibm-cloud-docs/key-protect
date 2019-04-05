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

# Hochverfügbarkeit und Disaster-Recovery
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} ist ein hoch verfügbarer regionaler Service mit automatischen Funktionen, die Ihnen helfen, Ihre Anwendungen sicher und betriebsbereit zu halten.
{: shortdesc}

Auf dieser Seite erfahren Sie mehr über die Verfügbarkeits- und Disaster-Recovery-Strategien von {{site.data.keyword.keymanagementserviceshort}}.

## Standorte, Tenancy und Verfügbarkeit
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} ist ein regionaler Multi-Tenant-Service. 

Sie können {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in einer der unterstützten [{{site.data.keyword.cloud_notm}}-Regionen](/docs/services/key-protect/regions.html) erstellen, die den geografischen Bereich darstellen, in dem Ihre {{site.data.keyword.keymanagementserviceshort}}-Anforderungen ausgeführt und verarbeitet werden. Jede {{site.data.keyword.cloud_notm}}-Region enthält [mehrere Verfügbarkeitszonen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/), mit denen der lokale Zugriff, niedrige Latenzzeit und Sicherheitsanforderungen für die Region erfüllt werden.

Bei Ihrer Planung Ihrer Strategie für die Verschlüsselung ruhender Daten mit {{site.data.keyword.cloud_notm}} beachten Sie, dass die Bereitstellung von {{site.data.keyword.keymanagementserviceshort}} in einer geografisch nahegelegenen Region mit höherer Wahrscheinlichkeit zu schnelleren, zuverlässigeren Verbindungen führt, wenn Sie mit den {{site.data.keyword.keymanagementserviceshort}}-APIs interagieren. Wählen Sie eine bestimmte Region aus, wenn die Benutzer, Anwendungen oder Services, die von einer {{site.data.keyword.keymanagementserviceshort}}-Ressource abhängig sind, geografisch konzentriert sind. Denken Sie daran, dass Benutzer und Services, die sich weit entfernt von der Region befinden, höhere Latenzzeiten haben könnten. 

Ihre Verschlüsselungsschlüssel sind auf die Region beschränkt, in der Sie sie erstellt haben. {{site.data.keyword.keymanagementserviceshort}} kopiert oder exportiert Verschlüsselungsschlüssel nicht in andere Regionen.{: note}

## Disaster-Recovery
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} verfügt über eine regionale Disaster-Recovery mit einer RTO (Recovery Time Objective-RTO) von einer Stunde. Der Service folgt den {{site.data.keyword.cloud_notm}}-Anforderungen für die Planung und Wiederherstellung nach einem Katastrophenfall. Weitere Informationen finden Sie in [Disaster-Recovery](/docs/overview/zero_downtime.html#disaster-recovery).



---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Evénements d'Activity Tracker
{: #at-events}

En tant que responsable de la sécurité, auditeur ou responsable, vous pouvez utiliser le service Activity Tracker pour suivre la façon dont utilisateurs et applications interagissent avec {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker enregistre les activités initiées par l'utilisateur qui changent l'état d'un service dans {{site.data.keyword.cloud_notm}}. Vous pouvez utiliser ce service pour étudier une activité anormale et des actions critiques, ainsi que pour respecter des exigences en matière de vérification de réglementation. En outre, vous avez la possibilité d'être alerté des actions lors de leur déroulement. Les événements collectés sont conformes à la norme CADF (Cloud Auditing Data Federation). 

Il existe actuellement deux services Activity Tracker disponibles dans le catalogue {{site.data.keyword.cloud_notm}}. {{site.data.keyword.keymanagementserviceshort}} envoie les événements ces deux services, et vous pouvez utiliser l'un ou l'autre pour surveiller votre activité {{site.data.keyword.keymanagementserviceshort}} dans {{site.data.keyword.cloud_notm}}. Toutefois, {{site.data.keyword.cloudaccesstrailfull}} est obsolète et aucune nouvelle instance ne peut être créée, et la prise en charge pour les instances de service existantes est disponible uniquement jusqu'au 30 septembre 2019.

Pour plus d'informations, voir :
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (obsolète)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## Liste des événements
{: #at-actions}

Le tableau suivant répertorie les actions {{site.data.keyword.keymanagementserviceshort}} qui génèrent un événement :

| Action                   | Description                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | Créer une clé                |
| `kms.secrets.read`       | Extraire une clé par ID        |
| `kms.secrets.delete`     | Supprimer une clé par ID          |
| `kms.secrets.list`       | Extraire une liste de clés     |
| `kms.secrets.head`       | Extraire le nombre de clés |
| `kms.secrets.wrap`       | Encapsuler une clé                  |
| `kms.secrets.unwrap`     | Désencapsuler une clé                |
| `kms.policies.read`      | Afficher la politique d'une clé     |
| `kms.policies.write`     | Définir une politique pour une clé      |
{: caption="Tableau 1. Actions {{site.data.keyword.keymanagementserviceshort}} générant des événements Activity Tracker" caption-side="top"}

## Affichage des événements
{: #at-ui}

Vous pouvez afficher les événements Activity Tracker associés à votre instance de service {{site.data.keyword.keymanagementserviceshort}} en utilisant [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) ou [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (obsolète).

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### Utilisation d'{{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

Les événements générés par une instance {{site.data.keyword.keymanagementserviceshort}} sont automatiquement transmises à l'instance de service {{site.data.keyword.at_full_notm}} disponible au même emplacement.  

{{site.data.keyword.at_full_notm}} ne peut posséder qu'une seule instance par emplacement. Pour afficher des événements, vous devez accéder à l'interface Web du service {{site.data.keyword.at_full_notm}} au même emplacement que celui où votre instance de service est disponible. Pour plus d'informations, voir [Lancement de l'interface utilisateur Web via l'interface utilisateur IBM Cloud](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### Utilisation de {{site.data.keyword.cloudaccesstrailfull_notm}} (obsolète)
{: #at-ui-legacy}

Les événements {{site.data.keyword.cloudaccesstrailshort}} sont disponibles dans le **domaine de compte** d'{{site.data.keyword.cloudaccesstrailshort}} qui est disponible dans la région {{site.data.keyword.cloud_notm}} dans laquelle les événements sont générés. Pour plus d'informations, voir [Affichage d'événements](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4).


## Analyse des événements
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

En raison de la sensibilité des informations pour une clé de chiffrement, lorsqu'un événement est généré suite à un appel API du service {{site.data.keyword.keymanagementserviceshort}}, il n'inclut pas d'informations détaillées sur la clé. Il inclut un ID de corrélation que vous pouvez utiliser pour identifier la clé en interne dans votre environnement cloud. L'ID de corrélation est une zone renvoyée dans la zone `responseBody.content`. Vous pouvez utiliser cette information pour corréler la clé {{site.data.keyword.keymanagementserviceshort}} avec les informations de l'action rapportées via l'événement {{site.data.keyword.cloudaccesstrailshort}}.

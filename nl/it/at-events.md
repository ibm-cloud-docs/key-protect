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

# Eventi di Activity Tracker
{: #at-events}

In qualità di responsabile della sicurezza, revisore o gestore, puoi utilizzare il servizio Activity Tracker per tenere traccia di come gli utenti e le applicazioni interagiscono con {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker registra le attività avviate dall'utente che modificano lo stato di un servizio in {{site.data.keyword.cloud_notm}}. Puoi utilizzare questo servizio per esaminare l'attività anomala e le azioni critiche e per rispettare i requisiti di controllo normativi. Inoltre, puoi essere avvisato sulle azioni nel momento in cui si verificano. Gli eventi raccolti sono conformi agli standard CADF (Cloud Auditing Data Federation). 

Attualmente esistono due servizi Activity Tracker disponibili nel catalogo {{site.data.keyword.cloud_notm}}. {{site.data.keyword.keymanagementserviceshort}} invia eventi a entrambi i servizi e puoi utilizzarli entrambi per monitorare la tua attività {{site.data.keyword.keymanagementserviceshort}} in {{site.data.keyword.cloud_notm}}. Tuttavia, {{site.data.keyword.cloudaccesstrailfull}} è obsoleto e non è possibile creare nuove istanze; inoltre il supporto per le istanze del servizio esistenti è disponibile solo fino al 30 settembre 2019.

Per ulteriori informazioni, vedi:
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (obsoleto)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## Elenco di eventi
{: #at-actions}

La seguente tabella elenca le azioni {{site.data.keyword.keymanagementserviceshort}} che generano un evento:

| Azione                   | Descrizione                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | Crea una chiave                |
| `kms.secrets.read`       | Richiama una chiave per ID        |
| `kms.secrets.delete`     | Elimina una chiave per ID          |
| `kms.secrets.list`       | Richiama un elenco di chiavi     |
| `kms.secrets.head`       | Richiama il numero delle chiavi |
| `kms.secrets.wrap`       | Impacchetta una chiave                  |
| `kms.secrets.unwrap`     | Spacchetta una chiave                |
| `kms.policies.read`      | Visualizza una politica per una chiave     |
| `kms.policies.write`     | Configura una politica per una chiave      |
{: caption="Tabella 1. Azioni {{site.data.keyword.keymanagementserviceshort}} che generano gli eventi di Activity Tracker" caption-side="top"}

## Visualizzazione degli eventi
{: #at-ui}

Puoi visualizzare gli eventi di Activity Tracker associati alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} utilizzando [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) o [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (obsoleto).

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### Utilizzo di {{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

Gli eventi generati da un'istanza di {{site.data.keyword.keymanagementserviceshort}} vengono inoltrati automaticamente a un'istanza del servizio {{site.data.keyword.at_full_notm}} che è disponibile nella stessa ubicazione. 

{{site.data.keyword.at_full_notm}} può avere una sola istanza per ubicazione. Per visualizzare gli eventi, devi accedere all'IU web del servizio {{site.data.keyword.at_full_notm}} nella stessa ubicazione in cui è disponibile la tua istanza del servizio. Per ulteriori informazioni, vedi [Avvio dell'IU web mediante l'IU IBM Cloud](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### Utilizzo di {{site.data.keyword.cloudaccesstrailfull_notm}} (obsoleto)
{: #at-ui-legacy}

Gli eventi {{site.data.keyword.cloudaccesstrailshort}} sono disponibili nel **dominio dell'account** {{site.data.keyword.cloudaccesstrailshort}}disponibile nella regione {{site.data.keyword.cloud_notm}} in cui vengono generati gli eventi. Per ulteriori informazioni, vedi [Visualizzazione degli eventi](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4).


## Analisi degli eventi
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

A causa della riservatezza delle informazioni di una chiave crittografata, quando viene generato un evento come risultato di una chiamata API al servizio {{site.data.keyword.keymanagementserviceshort}}, l'evento generato non include le informazioni dettagliate sulla chiave. L'evento include l'ID di correlazione che puoi utilizzare per identificare la chiave internamente nel tuo ambiente cloud. L'ID di correlazione è un campo che viene restituito come parte del campo `responseBody.content`. Puoi utilizzare queste informazioni per correlare la chiave {{site.data.keyword.keymanagementserviceshort}} con le informazioni dell'azione riportata tramite l'evento {{site.data.keyword.cloudaccesstrailshort}}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Eventi {{site.data.keyword.cloudaccesstrailshort}}
{: #activity-tracker-events}

Utilizza il servizio {{site.data.keyword.cloudaccesstrailfull}} per tenere traccia di come gli utenti e le applicazioni interagiscono con {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

Il servizio {{site.data.keyword.cloudaccesstrailfull_notm}} registra le attività avviate dall'utente che modificano lo stato di un servizio in {{site.data.keyword.cloud_notm}}. 

Per ulteriori informazioni, vedi la [Documentazione {{site.data.keyword.cloudaccesstrailshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla){: new_window}.

## Elenco di eventi
{: #list-activity-tracker-events}

La seguente tabella elenca le azioni che generano un evento:

| Azione               | Descrizione                 |
| -------------------- | --------------------------- |
| `kms.secrets.create` | Crea una chiave                |
| `kms.secrets.read`   | Richiama una chiave per ID        |
| `kms.secrets.delete` | Elimina una chiave per ID          |
| `kms.secrets.list`   | Richiama un elenco di chiavi     |
| `kms.secrets.head`   | Richiama il numero delle chiavi |
| `kms.secrets.wrap`   | Impacchetta una chiave                  |
| `kms.secrets.unwrap` | Spacchetta una chiave                |
| `kms.policies.read`  | Visualizza una politica per una chiave     |
| `kms.policies.write` | Configura una politica per una chiave     |
{: caption="Tabella 1. Azioni che generano gli eventi {{site.data.keyword.cloudaccesstrailfull_notm}}" caption-side="top"}

## Dove visualizzare gli eventi
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Gli eventi {{site.data.keyword.cloudaccesstrailshort}} sono disponibili nel **dominio dell'account** {{site.data.keyword.cloudaccesstrailshort}}disponibile nella regione {{site.data.keyword.cloud_notm}} in cui vengono generati gli eventi.

Ad esempio, quando crei, importi, elimini o leggi una chiave in {{site.data.keyword.keymanagementserviceshort}}, viene generato un evento {{site.data.keyword.cloudaccesstrailshort}}. Questi eventi vengono automaticamente inoltrati al servizio {{site.data.keyword.cloudaccesstrailshort}} nella stessa regione in cui è stato eseguito il provisioning del servizio {{site.data.keyword.keymanagementserviceshort}}.

Per monitorare l'attività API, devi eseguire il provisioning del servizio {{site.data.keyword.cloudaccesstrailshort}} in uno spazio nella stessa regione in cui è stato eseguito il provisioning del servizio {{site.data.keyword.keymanagementserviceshort}}. Successivamente, puoi visualizzare gli eventi tramite la vista account nell'IU {{site.data.keyword.cloudaccesstrailshort}} se hai un piano lite e tramite Kibana se hai un piano premium.

## Ulteriori informazioni
{: #activity-tracker-info}

A causa della riservatezza delle informazioni di una chiave crittografata, quando viene generato un evento come risultato di una chiamata API al servizio {{site.data.keyword.keymanagementserviceshort}}, l'evento generato non include le informazioni dettagliate sulla chiave. L'evento include l'ID di correlazione che puoi utilizzare per identificare la chiave internamente nel tuo ambiente cloud. L'ID di correlazione è un campo restituito come parte del campo `responseHeader.content`. Puoi utilizzare queste informazioni per correlare la chiave {{site.data.keyword.keymanagementserviceshort}} con le informazioni dell'azione riportata tramite l'evento {{site.data.keyword.cloudaccesstrailshort}}.

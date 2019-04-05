---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: encryption key states, encryption key lifecycle, manage key lifecycle

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

# Monitoraggio del ciclo di vita delle chiavi di crittografia
{: #key-states}

{{site.data.keyword.keymanagementservicefull}} segue le linee guida di sicurezza da
[NIST SP 800-57 for key states
![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}.
{: shortdesc}

## Transizioni e stati della chiave
{: #key_transitions}

Le chiavi di crittografia, nel loro ciclo di vita, passano attraverso vari stati che sono una funzione di quanto a lungo le chiavi rimangono attive e se i dati sono protetti. 

{{site.data.keyword.keymanagementserviceshort}} fornisce un'interfaccia utente grafica e un'API REST per il tracciamento delle chiavi quando si muovono tra gli stati nel loro ciclo di vita. Il seguente diagramma mostra come le chiavi passano attraverso gli stati tra la loro generazione e distruzione.

![Il diagramma mostra gli stessi componenti come descritto nella seguente tabella di definizioni.](../images/key-states_min.svg)

<table>
  <tr>
    <th>Stato</th>
    <th>Descrizione</th>
  </tr>
  <tr>
    <td>Preattiva</td>
    <td>Le chiavi sono state inizialmente create nello stato <i>pre-attivazione</i>. Una chiave preattivata non può essere utilizzata per proteggere i dati in modo crittografico.</td>
  </tr>
  <tr>
    <td>Attiva</td>
    <td>Le chiavi passano nello stato <i>attivata</i> nella data di attivazione. Questa transizione contrassegna l'inizio del periodo di crittografia di una chiave. Le chiavi senza data di attivazione diventano immediatamente attive e lo rimangono finché non scadono o vengono distrutte.</td>
  </tr>
  <tr>
    <td>Disattivata</td>
    <td>Una chiave passa allo stato <i>disattivata</i> alla sua data di scadenza, se ne è stata assegnata una. In questo stato, la chiave non può proteggere i dati in modo crittografico e può solo essere passata allo stato <i>distrutta</i>.</td>
  </tr>
  <tr>
    <td>Distrutta</td>
    <td>Le chiavi eliminate sono nello stato <i>distrutta</i>. Le chiavi in questo stato non sono ripristinabili. I metadati associati a una chiave, come il nome e la cronologia della chiave, vengono conservati nel database {{site.data.keyword.keymanagementserviceshort}}. </td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 1. Descrive le transizioni e gli stati della chiave.</caption>
</table>

Dopo aver aggiunto una chiave al servizio, utilizza il dashboard {{site.data.keyword.keymanagementserviceshort}} o le API REST {{site.data.keyword.keymanagementserviceshort}} per visualizzare la configurazione e la cronologia della transizione della tua chiave. Per scopi di controllo, puoi anche monitorare il percorso di attività di una chiave integrando {{site.data.keyword.keymanagementserviceshort}} con {{site.data.keyword.cloudaccesstrailfull}}. Dopo che è stato eseguito il provisioning di entrambi i servizi e che sono in esecuzione, vengono generati gli eventi di attività e raccolti automaticamente in un log {{site.data.keyword.cloudaccesstrailshort}} quando crei ed elimini le chiavi in {{site.data.keyword.keymanagementserviceshort}}. 

Per ulteriori informazioni, vedi [Monitoring {{site.data.keyword.keymanagementserviceshort}} activity ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: new_window}.

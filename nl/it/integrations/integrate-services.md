---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Key Protect integration, integrate service with Key Protect

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

# Integrazione dei servizi
{: #integrate-services}

{{site.data.keyword.keymanagementservicefull}} si integra con i dati e le soluzioni di archiviazione per aiutarti a portare e gestire la tua crittografia nel cloud.
{: shortdesc}

[Dopo aver creato un'istanza del servizio](/docs/services/key-protect?topic=key-protect-provision), puoi integrare {{site.data.keyword.keymanagementserviceshort}} con i seguenti servizi supportati:

<table>
    <tr>
        <th>Servizio</th>
        <th>Descrizione</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>Aggiungi la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption) ai tuoi bucket di archiviazione utilizzando {{site.data.keyword.keymanagementserviceshort}}. Utilizza le chiavi root che gestisci in {{site.data.keyword.keymanagementserviceshort}} per proteggere le chiavi di crittografia dei dati che codificano i tuoi dati inattivi. Per ulteriori informazioni, consulta [Integrazione con {{site.data.keyword.cos_full_notm}}](/docs/services/key-protect?topic=key-protect-integrate-cos).</p>
        </td>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.containerlong_notm}}</p>
        </td>
        <td>
          <p>Utilizza la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption) per proteggere i segreti nel tuo cluster {{site.data.keyword.containershort_notm}}. Per ulteriori informazioni, consulta [Crittografia dei segreti Kubernetes mediante {{site.data.keyword.keymanagementserviceshort}} ](/docs/containers?topic=containers-encryption#keyprotect).</p>
        </td>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.databases-for-postgresql_full_notm}}</p>
        </td>
        <td>
          <p>Proteggi i tuoi database associando delle chiavi root alla tua distribuzione {{site.data.keyword.databases-for-postgresql}}. Per ulteriori informazioni, consulta la [Documentazione di {{site.data.keyword.databases-for-postgresql}}](/docs/services/databases-for-postgresql?topic=databases-for-postgresql-key-protect).</p>
        </td>
    </tr>
      <tr>
        <td>
          <p>{{site.data.keyword.cloudant_short_notm}} per {{site.data.keyword.cloud_notm}} ({{site.data.keyword.cloud_notm}} Dedicated)</p>
        </td>
        <td>
          <p>Rafforza la tua strategia di crittografia inattiva associando delle chiavi root alla tua istanza {{site.data.keyword.cloudant_short_notm}} Dedicated Hardware. Per ulteriori informazioni, consulta la [Documentazione di {{site.data.keyword.cloudant_short_notm}}](/docs/services/Cloudant/offerings?topic=cloudant-security#secure-access-control).</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">Tabella 1. Descrive le integrazioni disponibili per {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

## Informazioni sulla tua integrazione 
{: #understand-integration}

Quando integri un servizio supportato con {{site.data.keyword.keymanagementserviceshort}}, abiliti la [crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption) per tale servizio. Questa integrazione ti consente di utilizzare una chiave root che memorizzi in {{site.data.keyword.keymanagementserviceshort}} per impacchettare le chiavi di crittografia dei dati che codificano i tuoi dati inattivi. 

Ad esempio, puoi creare una chiave root, gestire la chiave in {{site.data.keyword.keymanagementserviceshort}} e utilizzarla per proteggere i dati memorizzati tra diversi servizi cloud.

![Il diagramma mostra una vista contestuale della tua integrazione {{site.data.keyword.keymanagementserviceshort}}.](../images/kp-integrations_min.svg)

### Metodi API {{site.data.keyword.keymanagementserviceshort}}
{: #envelope-encryption-api-methods}

Dietro le quinte, l'API {{site.data.keyword.keymanagementserviceshort}} guida il processo di crittografia envelope.  

La seguente tabella elenca i metodi API che aggiungono o rimuovono la crittografia envelope su una risorsa.

<table>
  <tr>
    <th>Metodo</th>
    <th>Descrizione</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-wrap-keys">Impacchetta (codifica) una chiave di crittografia dei dati (o DEK, data encryption key)</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=unwrap</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-unwrap-keys">Spacchetta (decodifica) una chiave di crittografia dei dati (o DEK, data encryption key)</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 2. Descrive i metodi API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi in {{site.data.keyword.keymanagementserviceshort}}, consulta la [Documentazione di riferimento API di {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: tip}

## Integrazione di un servizio supportato
{: #grant-access}

Per aggiungere un'integrazione, crea un'autorizzazione tra i servizi utilizzando il dashboard {{site.data.keyword.iamlong}}. Le autorizzazioni abilitano le politiche di accesso da servizio a servizio, pertanto puoi associare una risorsa nel tuo servizio di dati cloud con una [chiave root](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types) che gestisci in {{site.data.keyword.keymanagementserviceshort}}.

Prima di creare un'autorizzazione, assicurati di fornire entrambi i servizi nella stessa regione. Per ulteriori informazioni sulle autorizzazioni dei servizi, vedi [Concessione dell'accesso tra i servizi ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-serviceauth){: new_window}.
{: note}

Quando sei pronto per integrare un servizio, usa la seguente procedura per creare un'autorizzazione:

1. Dalla barra dei menu, fai clic su **Gestisci** &gt; **Sicurezza** &gt; **Accesso (IAM)** e seleziona **Autorizzazioni**. 
2. Fai clic su **Crea**.
3. Seleziona un servizio di origine e di destinazione per l'autorizzazione.
 
  Per **Servizio di origine**, seleziona il servizio di dati cloud che vuoi integrare con {{site.data.keyword.keymanagementserviceshort}}. Per **Servizio di destinazione**, seleziona **{{site.data.keyword.keymanagementservicelong_notm}}**.

5. Abilita il ruolo **Lettore**.

    Con le autorizzazioni _Lettore_, il tuo servizio di origine può sfogliare le chiavi root fornite nell'istanza specificata di {{site.data.keyword.keymanagementserviceshort}}.

6. Fai clic su **Autorizza**.

## Operazioni successive
{: #integration-next-steps}

Aggiungi la crittografia avanzata alle tue risorse cloud creando una chiave root in {{site.data.keyword.keymanagementserviceshort}}. Aggiungi una nuova risorsa a un servizio di dati cloud supportato e quindi seleziona la chiave root che vuoi utilizzare per la crittografia avanzata.

- Per ulteriori informazioni sulla creazione di chiavi root con il servizio {{site.data.keyword.keymanagementserviceshort}}, vedi [Creazione delle chiavi root](/docs/services/key-protect?topic=key-protect-create-root-keys).
- Per ulteriori informazioni su come portare le tue proprie chiavi root al servizio {{site.data.keyword.keymanagementserviceshort}}, vedi [Importazione delle chiavi root](/docs/services/key-protect?topic=key-protect-import-root-keys).



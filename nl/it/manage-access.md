---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: user permissions, manage access, IAM roles

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

# Gestione dell'accesso utente
{: #manage-access}

{{site.data.keyword.keymanagementservicefull}} supporta un sistema di controllo dell'accesso centralizzato, controllato da
{{site.data.keyword.iamlong}}, per aiutarti a gestire gli utenti e l'accesso alle tue chiavi crittografate.
{: shortdesc}

Ti raccomandiamo di concedere le autorizzazioni di accesso quando inviti nuovi utenti nel tuo servizio o account. Ad esempio, considera le seguenti linee guida:

- **Abilita l'accesso utente alle risorse nel tuo account assegnando i ruoli Cloud IAM.**
    Anziché condividere le tue credenziali ammin, crea nuove politiche per gli utenti che devono accedere alle chiavi crittografate nel tuo account. Se sei l'amministratore del tuo account, ti viene automaticamente assegnata una politica _Gestore_ con l'accesso a tutte le risorse nell'account.
- **Concedi i ruoli e le autorizzazioni solo per gli scopi necessari.**
    Ad esempio, se un utente deve accedere solo a una vista di alto livello delle chiavi all'interno di uno spazio specificato, concedi all'utente il ruolo _Lettore_ per tale spazio.
- **Controlla regolarmente chi può gestire il controllo degli accessi ed eliminare le risorse della chiave.**
    Ricorda che concedendo un ruolo _Gestore_ a un utente lo rendi capace di modificare le politiche del servizio per altri utenti, in aggiunta a eliminare in modo permanente le risorse.

## Ruoli e autorizzazioni
{: #roles}

Con {{site.data.keyword.iamshort}} (IAM), puoi gestire e definire l'accesso per gli utenti e le risorse nel tuo account.

Per semplificare l'accesso, {{site.data.keyword.keymanagementserviceshort}} allinea i ruoli Cloud IAM in modo che ogni utente disponga di una vista differente del servizio, in base al ruolo assegnato. Se sei un amministratore della sicurezza del tuo servizio, puoi assegnare i ruoli Cloud IAM che corrispondono alle autorizzazioni {{site.data.keyword.keymanagementserviceshort}} specifiche che vuoi concedere ai membri del tuo team.

La seguente tabella mostra come i ruoli di identità e accesso vengono associati alle autorizzazioni {{site.data.keyword.keymanagementserviceshort}}:

<table>
  <col width="20%">
  <col width="40%">
  <col width="40%">
  <tr>
    <th>Ruolo di accesso al servizio</th>
    <th>Descrizione</th>
    <th>Azioni</th>
  </tr>
  <tr>
    <td><p>Lettore</p></td>
    <td><p>Un lettore può esplorare una vista di alto livello delle chiavi e eseguire le azioni di impacchettamento e spacchettamento. I lettori non possono accedere o modificare il materiale della chiave.</p></td>
    <td>
      <p>
        <ul>
          <li>Visualizza le chiavi</li>
          <li>Impacchetta le chiavi</li>
          <li>Spacchetta le chiavi</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Scrittore</p></td>
    <td><p>Uno scrittore può creare e modificare le chiavi, eseguirne la rotazione e accedere al materiale della chiave.</p></td>
    <td>
      <p>
        <ul>
          <li>Crea le chiavi</li>
          <li>Visualizza le chiavi</li>
          <li>Esegui la rotazione delle chiavi</li>
          <li>Impacchetta le chiavi</li>
          <li>Spacchetta le chiavi</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Gestore</p></td>
    <td><p>Un gestore può eseguire tutte le azioni di un lettore e di uno scrittore, inclusa la possibilità di configurare le politiche di rotazione per le chiavi, eliminare le chiavi, invitare nuovi utenti e assegnare le politiche di accesso ad altri utenti.</p></td>
    <td>
      <p>
        <ul>
          <li>Tutte le azioni che un lettore o uno scrittore possono eseguire</li>
          <li>Assegna le politiche di accesso utente</li>
          <li>Configura le politiche di rotazione della chiave</li>
          <li>Elimina le chiavi</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Tabella 1. Descrive come i ruoli di identità e accesso vengono associati alle autorizzazioni {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

I ruoli utente Cloud IAM forniscono l'accesso al livello dell'istanza del servizio o del servizio. I [ruoli Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-cfaccess){: new_window} sono separati e definiscono l'accesso a livello di organizzazione o di spazio. Per ulteriori informazioni su {{site.data.keyword.iamshort}}, consulta [Ruoli utente e autorizzazioni ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-userroles){: new_window}.
{: note}

## Operazioni successive
{: #manage-access-next-steps}

Gli amministratori e i proprietari dell'account possono invitare gli utenti e configurare le politiche del servizio che corrispondono alle azioni {{site.data.keyword.keymanagementserviceshort}} che possono eseguire gli utenti.

- Per ulteriori informazioni sull'assegnazione dei ruoli utente nella IU {{site.data.keyword.cloud_notm}}, vedi [Gestione dell'accesso IAM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-getstarted){: new_window}.


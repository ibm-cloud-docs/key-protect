---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: key management service, kms, manage encryption keys, data encryption, data-at-rest, protect data encryption keys

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

# Esercitazione introduttiva
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} ti aiuta ad eseguire il provisioning di chiavi crittografate per le applicazioni nei servizi {{site.data.keyword.cloud_notm}}. Questa esercitazione illustra come creare e aggiungere le chiavi di crittografia esistenti utilizzando il dashboard {{site.data.keyword.keymanagementserviceshort}}, quindi puoi gestire la codifica dei dati da un'ubicazione centrale.
{: shortdesc}

## Introduzione alle chiavi di crittografia
{: #get-started-keys}

Dal dashboard {{site.data.keyword.keymanagementserviceshort}}, puoi creare nuove chiavi per la crittografia o puoi importare le tue chiavi esistenti. 

Scegli tra i due tipi di chiave:

<dl>
  <dt>Chiavi root</dt>
    <dd>Le chiavi root sono chiavi di impacchettamento della chiave simmetriche che gestisci completamente in {{site.data.keyword.keymanagementserviceshort}}. Puoi utilizzare una chiave root per proteggere altre chiavi di crittografia con la codifica avanzata. Per ulteriori informazioni, consulta <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">Protezione dei dati con la crittografia envelope</a>.</dd>
  <dt>Chiavi standard</dt>
    <dd>Le chiavi standard sono le chiavi simmetriche utilizzate per la crittografia. Puoi utilizzare una chiave standard per codificare e decodificare direttamente i dati.</dd>
</dl>

## Creazione di nuove chiavi
{: #create-keys}

[Dopo aver creato un'istanza di {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps), sei pronto a designare le chiavi nel servizio. 

Completa la seguente procedura per creare la tua prima chiave crittografica. 

1. Nella pagina dei dettagli dell'applicazione, fai clic su **Manage** &gt; **Add key**.
2. Per creare una nuova chiave, seleziona la finestra **Create a key**.

    Specifica i dettagli della chiave:

    <table>
      <tr>
        <th>Impostazione</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Un alias univoco e leggibile dall'utente per una facile identificazione della tua chiave.</p>
          <p>Per proteggere la tua privacy, assicurati che il nome della chiave non contenga informazioni d'identificazione personale, come il tuo nome o la tua posizione.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo di chiave</td>
        <td>Il <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">tipo di chiave</a> che desideri gestire in {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 1. Descrizione delle impostazioni <b>Create a key</b></caption>
    </table>

3. Una volta che hai finito di compilare i dettagli della chiave, fai clic su **Create key** per confermare. 

Le chiavi create nel servizio sono chiavi simmetriche a 256 bit, supportate dall'algoritmo AES-GCM. Per aggiungere sicurezza, le chiavi sono generate dagli HSM (Hardware Security Module) certificati da FIPS 140-2 Level 2 ubicati in data center {{site.data.keyword.cloud_notm}} sicuri. 

## Importazione delle tue chiavi
{: #import-keys}

Puoi abilitare i vantaggi di BYOK (Bring Your Own Key) introducendo le tue chiavi esistenti al servizio. 

Completa la seguente procedura per aggiungere una chiave esistente.

1. Nella pagina dei dettagli dell'applicazione, fai clic su **Manage** &gt; **Add key**.
2. Per caricare una chiave esistente, seleziona la finestra **Import your own key**.

    Specifica i dettagli della chiave:

    <table>
      <tr>
        <th>Impostazione</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Un alias univoco e leggibile dall'utente per una facile identificazione della tua chiave.</p>
          <p>Per proteggere la tua privacy, assicurati che il nome della chiave non contenga informazioni d'identificazione personale, come il tuo nome o la tua posizione.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo di chiave</td>
        <td>Il <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">tipo di chiave</a> che desideri gestire in {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <tr>
        <td>Materiale della chiave</td>
        <td>Il materiale della chiave, come ad esempio la chiave simmetrica, che vuoi archiviare nel servizio {{site.data.keyword.keymanagementserviceshort}}. La chiave che fornisci deve essere codificata con base64.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabella 2. Descrizione delle impostazioni <b>Import your own key</b></caption>
    </table>

3. Una volta che hai finito di compilare i dettagli della chiave, fai clic su **Import key** per confermare. 

Dal dashboard {{site.data.keyword.keymanagementserviceshort}} , puoi controllare le caratteristiche generali delle tue nuove chiavi. 

## Operazioni successive
{: #get-started-next-steps}

Ora puoi utilizzare le chiavi per crittografare i tuoi servizi e le tue applicazioni. Se hai aggiunto una chiave root al servizio, potresti desiderare ulteriori informazioni sul suo utilizzo per proteggere le chiavi che codificano i tuoi dati inattivi. Per iniziare, consulta l'[Impacchettamento delle chiavi](/docs/services/key-protect?topic=key-protect-wrap-keys).

- Per trovare ulteriori informazioni sulla gestione e protezione delle tue chiavi crittografate con una chiave root, consulta [Protezione dei dati con la crittografia envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption).
- Per ulteriori informazioni sull'integrazione del servizio {{site.data.keyword.keymanagementserviceshort}} con altre soluzioni di dati cloud, [controlla la documentazione di Integrations](/docs/services/key-protect?topic=key-protect-integrate-services).
- Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.

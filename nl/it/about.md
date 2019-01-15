---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Informazioni su {{site.data.keyword.keymanagementserviceshort}}
{: #about}

{{site.data.keyword.keymanagementservicefull}} ti aiuta ad eseguire il provisioning di chiavi crittografate per le applicazioni nei servizi {{site.data.keyword.cloud_notm}}. Mentre gestisci il ciclo di vita delle tue chiavi, puoi ottenere dei benefici sapendo che le tue chiavi sono protette dagli HSM (Hardware Security Module) basati sul cloud certificati FIPS 140-2 Level 2 che proteggono dal furto di informazioni.
{: shortdesc}

## Motivi per usare il servizio
{: #kp-reasons}

Potresti aver bisogno di gestire le chiavi nei seguenti scenari:

<table>
  <tr>
    <th>Scenario</th>
    <th>Motivi</th>
  </tr>
  <tr>
    <td>Hai bisogno di codificare elevati volumi di dati sensibili, come le registrazioni mediche, per risorsa individuale.</td>
    <td>Puoi integrare il servizio {{site.data.keyword.keymanagementserviceshort}} con soluzioni di archiviazione, come [{{site.data.keyword.cos_full_notm}} ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](/docs/services/cloud-object-storage/about-cos.html) per crittografare i dati inattivi nel cloud. Ogni documento può essere protetto da una chiave differente, in questo modo disponi del controllo
granulare ai tuoi dati.</td>
  </tr>
  <tr>
    <td>Vuoi eseguire la crittografia envelope poiché sposti i dati nel cloud. Hai bisogno della tua chiave di crittografia master, in modo da ottenere il controllo completo di altre chiavi che codificano i tuoi dati inattivi.</td>
    <td>Con {{site.data.keyword.keymanagementserviceshort}}, puoi [impacchettare le tue chiavi di crittografia dei dati con una chiave root altamente sicura](/docs/services/key-protect/concepts/envelope-encryption.html). Puoi portare le tue proprie chiavi root o crearle nel servizio.</td>
  </tr>
  <tr>
    <td>Come amministratore IT per una grande azienda, hai bisogno di integrare, tracciare e ruotare
le chiavi per molte offerte di servizio differenti.</td>
    <td>L'interfaccia {{site.data.keyword.keymanagementserviceshort}}
semplifica la gestione di più servizi di codifica. Con il servizio, puoi gestire
e ordinare le chiavi in un'ubicazione centralizzata o puoi separarle per progetto e posizionarle
in spazi {{site.data.keyword.cloud_notm}} differenti.</td>
  </tr>
  <tr>
    <td>Sei un amministratore della sicurezza di una azienda, come un leader finanziario o un avvocato, che deve
rispettare la governance su come vengono protetti i dati. Devi concedere l'accesso controllato alle chiavi
senza compromettere la sicurezza dei dati.</td>
    <td>Con il servizio, puoi controllare l'accesso utente per gestire le chiavi
[assegnando ruoli di gestione dell'accesso e dell'identità differenti](/docs/services/key-protect/manage-access.html#roles). Ad esempio,
puoi concedere l'accesso in sola lettura agli utenti che necessitano di visualizzare le informazioni sulla creazione delle chiavi senza
visualizzare il materiale della chiave.</td>
  <tr>
    <td>Come sviluppatore, puoi integrare le applicazioni pre-esistenti, come l'archiviazione
con crittografia automatica, in {{site.data.keyword.keymanagementserviceshort}}. Puoi anche
sviluppare le tue proprie applicazioni da integrare con il servizio.</td>
    <td>La applicazioni in o al di fuori di {{site.data.keyword.cloud_notm}} possono
essere integrate con le API {{site.data.keyword.keymanagementserviceshort}}. Puoi utilizzare le tue chiavi esistenti per le tue applicazioni. </td>
  </tr>
  <tr>
    <td>Il tuo team di sviluppo ha politiche rigorose e hai bisogno di un modo per generare e ruotare
le chiavi ogni 14 giorni.</td>
    <td>Con {{site.data.keyword.cloud_notm}},
puoi generare le chiavi rapidamente da un HSM (Hardware Security Module) per soddisfare i tuoi bisogni di sicurezza
continuativa.</td>
  </tr>
</table>

## Come funziona {{site.data.keyword.keymanagementserviceshort}}
{: #kp-how}

{{site.data.keyword.keymanagementservicelong_notm}} ti aiuta nella gestione delle chiavi di crittografia nella tua organizzazione eseguendo l'allineamento con i ruoli di gestione dell'accesso e dell'identità
{{site.data.keyword.cloud_notm}}.

Un amministratore della sicurezza o IT
necessita di autorizzazioni avanzate rispetto a un revisore. Per semplificare l'accesso, {{site.data.keyword.keymanagementserviceshort}}
mappa i ruoli di gestione dell'accesso e dell'identità {{site.data.keyword.cloud_notm}} in modo che ogni ruolo abbia una visualizzazione diversa
del servizio. Per agevolare l'orientamento alla visualizzazione e al livello di accesso che meglio rispondono alle tue esigenze, vedi [Gestione di utenti e accesso](/docs/services/key-protect/manage-access.html#roles).

Il seguente diagramma mostra come i gestori, i lettori e gli scrittori possono interagire con le chiavi gestite nel servizio.

<dl>
  <dt>Integrazione dei servizi</dt>
    <dd>I gestori della tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}
gestiscono le chiavi per la codifica.</dd>
  <dt>Revisioni</dt>
    <dd>I lettori accedono a una vista di alto livello delle chiavi e identificano le attività sospette.</dd>
  <dt>Applicazioni</dt>
    <dd>Gli scrittori gestiscono le chiavi per la crittografia che codificano nelle applicazioni.</dd>
</dl>

![Il diagramma mostra gli stessi componenti descritti nell'elenco di definizione precedente.](images/keys-use-cases_min.svg)

## Architettura {{site.data.keyword.keymanagementserviceshort}}
{: #kp_architecture}

{{site.data.keyword.keymanagementservicelong_notm}} si articola
in tecnologie accettate dal settore.

<dl>
  <dt>Server {{site.data.keyword.cloud_notm}}</dt>
    <dd>Identità, progetti e i loro token dal server {{site.data.keyword.cloud_notm}} che abilitano il servizio
{{site.data.keyword.keymanagementserviceshort}} ad associare le risorse alle chiavi.</dd>
  <dt>API per {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>L'API REST {{site.data.keyword.keymanagementserviceshort}}
ti guida nella creazione e gestione della chiave. Il servizio fornisce una tenancy multipla crittografata..</dd>
  <dt>Interfaccia utente in {{site.data.keyword.cloud_notm}}</dt>
    <dd>Con l'interfaccia utente (IU) {{site.data.keyword.keymanagementserviceshort}},
puoi lavorare con le tue chiavi in modo sicuro.</dd>
  <dt>HSM (Hardware Security Module)</dt>
    <dd>Dietro le quinte, i data center {{site.data.keyword.cloud_notm}} forniscono l'hardware per proteggere le tue chiavi.</dd>
  <dt>Cluster database</dt>
    <dd>Puoi contare su un'archiviazione ridondante e sicura delle tue chiavi con un database in cluster.</dd>
</dl>

I seguenti diagrammi mostrano come {{site.data.keyword.keymanagementserviceshort}}
utilizza gli HSM (Hardware Security Module) per generare le chiavi in confronto a come il servizio archivia le chiavi.

![Il diagramma mostra come vengono generate le chiavi.](images/generated-key_min.svg)

![Il diagramma mostra come vengono archiviate le chiavi esistenti.](images/stored-key_min.svg)

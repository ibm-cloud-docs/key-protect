---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Risoluzione dei problemi
{: #troubleshooting}

I problemi generali con l'utilizzo di {{site.data.keyword.keymanagementservicefull}} potrebbero includere il fornire intestazioni o credenziali corrette quando interagisci con l'API. In molti casi, puoi risolvere questi problemi seguendo pochi semplici passi.
{: shortdesc}

## Impossibile accedere all'interfaccia utente
{: #unabletoaccessUI}

Quando accedi all'interfaccia utente {{site.data.keyword.keymanagementserviceshort}}, l'IU non viene caricata come previsto.

Dal dashboard {{site.data.keyword.cloud_notm}}, seleziona la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Viene visualizzato il seguente errore: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Il 15 dicembre 2017, abbiamo aggiunto nuove funzioni, come la [crittografia envelope](/docs/services/keymgmt/concepts/keyprotect_envelope.html), al servizio {{site.data.keyword.keymanagementserviceshort}}. Puoi ora fornire il servizio {{site.data.keyword.keymanagementserviceshort}} a livello globale, senza dover specificare un'organizzazione e uno spazio Cloud Foundry.
{: tsCauses}

Queste modifiche hanno interessato l'interfaccia utente per le istanze più vecchie del servizio. Se hai creato la tua istanza di {{site.data.keyword.keymanagementserviceshort}} prima del 28 settembre 2017, l'interfaccia utente potrebbe non funzionare come previsto.

Stiamo lavorando per risolvere questo problema. Come soluzione temporanea, puoi continuare a gestire le tue chiavi utilizzando l'API {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Puoi utilizzare l'endpoint `https://ibm-key-protect.edge.bluemix.net` legacy per interagire con il servizio {{site.data.keyword.keymanagementserviceshort}}.

**Richiesta di esempio**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Impossibile creare o eliminare chiavi
{: #unabletomanagekeys}

Quando accedi all'interfaccia utente {{site.data.keyword.keymanagementserviceshort}}, non vedi le opzioni per aggiungere o eliminare le chiavi.

Dal dashboard {{site.data.keyword.cloud_notm}}, seleziona la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Puoi vedere un elenco di chiavi, ma non vedi le opzioni per aggiungere o eliminare le chiavi. 

Non hai l'autorizzazione corretta per eseguire le azioni di {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifica con il tuo amministratore che ti sia stato assegnato il ruolo corretto nel gruppo di risorse o nell'istanza del servizio applicabile. Per ulteriori informazioni sui ruoli, vedi [Ruoli e autorizzazioni](/docs/services/keymgmt/keyprotect_manage_access.html#roles).
{: tsResolve}

## Come ottenere aiuto e supporto
{: #getting_help}

Se hai dei problemi o delle domande quando utilizzi {{site.data.keyword.keymanagementserviceshort}}, puoi controllare in {{site.data.keyword.cloud_notm}} oppure ottenere aiuto cercando informazioni o facendo domande attraverso un forum. Puoi inoltre aprire un ticket di supporto.
{: shortdesc}

Puoi controllare se {{site.data.keyword.cloud_notm}} è disponibile andando alla [pagina sugli stati ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/status?tags=platform,runtimes,services).

Puoi rivedere i forum per controllare se altri utenti hanno riscontrato lo stesso problema. Quando stai utilizzando i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo
{{site.data.keyword.cloud_notm}}.

- Se hai domande tecniche su {{site.data.keyword.keymanagementserviceshort}}, inserisci la tua domanda in [Stack Overflow ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} e contrassegnala con le tag "ibm-cloud" e "key-protect".
- Per domande sul servizio e sulle istruzioni introduttive, utilizza il forum [IBM developerWorks dW Answers ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window}. Includi le tag "ibm-cloud"
e "key-protect".

Per ulteriori dettagli sull'utilizzo dei forum, consulta il documento relativo a [come ottenere supporto ![Icona di link esterno](../../icons/launch-glyph.svg "Icona di link esterno")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window}.

Per ulteriori informazioni sull'apertura di un ticket di supporto {{site.data.keyword.IBM_notm}} o sui livelli di supporto e sulla gravità dei ticket, vedi [Come contattare il supporto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.

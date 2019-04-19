---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# Configurazione dell'API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} fornisce un'API REST che può essere utilizzata con qualsiasi linguaggio di programmazione per archiviare, richiamare e generare le chiavi di crittografia.
{: shortdesc}

## Richiamo delle tue credenziali {{site.data.keyword.cloud_notm}}
{: #retrieve-credentials}

Per utilizzare l'API, devi generare le tue credenziali del servizio e di autenticazione. 

Per raccogliere le tue credenziali:

1. [Genera un token di accesso {{site.data.keyword.cloud_notm}} IAM](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Richiama l'ID istanza che identifica in modo univoco la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## Creazione della tua richiesta API
{: #form-api-request}

Quando effettui una chiamata API al servizio, struttura la tua richiesta API in base a come hai inizialmente eseguito il provisioning della tua istanza di {{site.data.keyword.keymanagementserviceshort}}. 

Per creare la tua richiesta, accoppia un [endpoint del servizio regionale](/docs/services/key-protect?topic=key-protect-regions) con le credenziali di autenticazione appropriate. Ad esempio, se hai creato un'istanza del servizio per la regione `us-south`, utilizza il seguente endpoint e le intestazioni API per sfogliare le chiavi nel tuo servizio:

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

Sostituisci `<access_token>` e `<instance_ID>` con le tue credenziali del servizio e di autenticazione richiamate.

Vuoi tenere traccia delle tue richieste API nel caso che qualcosa non funzioni? Quando includi l'indicatore `-v` come parte della richiesta cURL, ottieni un valore `correlation-id` nelle intestazioni della risposta. Puoi utilizzare questo valore per correlare e tracciare la richiesta per scopi di debug.
{: tip} 

## Operazioni successive
{: #set-up-api-next-steps}

È tutto pronto per iniziare a gestire le tue chiavi di crittografia in Key Protect. Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.

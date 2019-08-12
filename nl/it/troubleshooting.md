---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# Risoluzione dei problemi
{: #troubleshooting}

I problemi generali con l'utilizzo di {{site.data.keyword.keymanagementservicefull}} potrebbero includere il fornire intestazioni o credenziali corrette quando interagisci con l'API. In molti casi, puoi risolvere questi problemi seguendo pochi semplici passi.
{: shortdesc}

## Impossibile creare o eliminare chiavi
{: #unable-to-create-keys}

Quando accedi all'interfaccia utente {{site.data.keyword.keymanagementserviceshort}}, non vedi le opzioni per aggiungere o eliminare le chiavi.

Dal dashboard {{site.data.keyword.cloud_notm}}, seleziona la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Puoi vedere un elenco di chiavi, ma non vedi le opzioni per aggiungere o eliminare le chiavi. 

Non hai l'autorizzazione corretta per eseguire le azioni di {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifica con il tuo amministratore che ti sia stato assegnato il ruolo corretto nel gruppo di risorse o nell'istanza del servizio applicabile. Per ulteriori informazioni sui ruoli, vedi [Ruoli e autorizzazioni](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Come ottenere aiuto e supporto
{: #getting-help}

Se hai dei problemi o delle domande quando utilizzi {{site.data.keyword.keymanagementserviceshort}}, puoi controllare in {{site.data.keyword.cloud_notm}} oppure ottenere aiuto cercando informazioni o facendo domande attraverso un forum. Puoi inoltre aprire un ticket di supporto.
{: shortdesc}

Puoi controllare se {{site.data.keyword.cloud_notm}} è disponibile andando alla [pagina sugli stati](https://{DomainName}/status?tags=platform,runtimes,services){: external}.

Puoi rivedere i forum per controllare se altri utenti hanno riscontrato lo stesso problema. Quando stai utilizzando i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo
{{site.data.keyword.cloud_notm}}.

- Se hai domande tecniche su {{site.data.keyword.keymanagementserviceshort}}, pubblica la tua domanda in [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} e contrassegnala con le tag "ibm-cloud" e "key-protect".
- Per domande sul servizio e sulle istruzioni per l'utilizzo iniziale, utilizza il forum [IBM developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external}. Includi le tag "ibm-cloud"
e "key-protect".

Per ulteriori dettagli sull'utilizzo dei forum, vedi [Come ottenere supporto](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external}.

Per ulteriori informazioni su come aprire un ticket di supporto {{site.data.keyword.IBM_notm}} o sui livelli di supporto e sulla gravità dei ticket, vedi [Come contattare il supporto](/docs/get-support?topic=get-support-getting-customer-support){: external}.

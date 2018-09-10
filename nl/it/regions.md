---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Regioni e ubicazioni
{: #regions}

Puoi collegarti alle tue applicazioni con il servizio {{site.data.keyword.keymanagementservicelong_notm}} specificando un endpoint del servizio regionale.
{: shortdesc}

## Regioni disponibili
{: #regions}

{{site.data.keyword.keymanagementserviceshort}} è disponibile nelle seguenti regioni e ubicazioni:
![L'immagine mostra le regioni in cui è disponibile il servizio Key Protect.](images/world-map_min.svg)

## Endpoint del servizio
{: #endpoints}

Se gestisci le tue risorse {{site.data.keyword.keymanagementserviceshort}} in modo programmatico, vedi la seguente tabella per determinare gli endpoint API da utilizzare quando ti colleghi all'[API {{site.data.keyword.keymanagementserviceshort}}](https://console.bluemix.net/apidocs/kms): 

<table>
    <tr>
        <th>Nome regione</th>
        <th>Ubicazione geografica</th>
        <th>Endpoint API del servizio</th>
    </tr>
    <tr>
        <td>Germania</td>
        <td>Francoforte, Germania</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Sydney</td>
        <td>Sydney, Australia</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Regno Unito</td>
        <td>Londra, Inghilterra</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Stati Uniti Sud</td>
        <td>Dallas, Stati Uniti</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Tabella 1. Mostra gli endpoint disponibili per l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Per le istanze del servizio {{site.data.keyword.keymanagementserviceshort}} presenti in un'organizzazione o uno spazio Cloud Foundry, utilizza l'endpoint `https://ibm-key-protect.edge.bluemix.net` legacy per interagire con l'API {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

Per ulteriori informazioni sull'autenticazione con {{site.data.keyword.keymanagementserviceshort}}, vedi [Accesso all'API](/docs/services/key-protect/access-api.html).

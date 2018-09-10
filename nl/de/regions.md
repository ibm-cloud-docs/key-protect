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

# Regionen und Standorte
{: #regions}

Sie können Ihre Anwendungen mit dem {{site.data.keyword.keymanagementservicelong_notm}}-Service verbinden, indem Sie einen regionalen Serviceendpunkt angeben.
{: shortdesc}

## Verfügbare Regionen
{: #regions}

{{site.data.keyword.keymanagementserviceshort}} ist in den Regionen und an den Standorten verfügbar, die im Folgenden aufgeführt sind:
![Regionen, in denen der Key Protect-Service verfügbar ist](images/world-map_min.svg)

## Serviceendpunkte
{: #endpoints}

Wenn Sie Ihre {{site.data.keyword.keymanagementserviceshort}}-Ressourcen programmgesteuert verwalten, können Sie mithilfe der folgenden Tabelle die API-Endpunkte bestimmen, die für die Verbindung zur [{{site.data.keyword.keymanagementserviceshort}}-API](https://console.bluemix.net/apidocs/kms) verwendet werden: 

<table>
    <tr>
        <th>Regionsname</th>
        <th>Geografischer Standort</th>
        <th>Service-API-Endpunkt</th>
    </tr>
    <tr>
        <td>Deutschland</td>
        <td>Frankfurt, Deutschland</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Sydney</td>
        <td>Sydney, Australien</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Vereinigtes Königreich</td>
        <td>London, England</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Vereinigte Staaten (Süden)</td>
        <td>Dallas, USA</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Tabelle 1. Verfügbare Endpunkte für die {{site.data.keyword.keymanagementserviceshort}}-API</caption>
</table>

Verwenden Sie für {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanzen, die in Cloud Foundry-Organisationen oder -Bereichen vorhanden sind, den alten Endpunkt `https://ibm-key-protect.edge.bluemix.net`, um mit der {{site.data.keyword.keymanagementserviceshort}}-API zu interagieren.
{: tip}

Weitere Informationen zur Authentifizierung mit {{site.data.keyword.keymanagementserviceshort}} finden Sie in [Auf die API zugreifen](/docs/services/key-protect/access-api.html).

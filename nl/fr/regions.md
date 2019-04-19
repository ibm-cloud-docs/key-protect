---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Key Protect API endpoints, available regions

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
{:deprecated: .deprecated}

# Régions et emplacements
{: #regions}

Vous pouvez connecter vos applications au service {{site.data.keyword.keymanagementservicelong}} en indiquant un noeud final de service régional.
{: shortdesc}

## Régions disponibles
{: #available-regions}

{{site.data.keyword.keymanagementserviceshort}} est disponible dans les régions et emplacements suivants :
![Image illustrant les régions dans lesquelles le service Key Protect est disponible.](images/world-map_min.svg)

## Noeuds finaux de service
{: #service-endpoints}

Si vous gérez vos ressources {{site.data.keyword.keymanagementserviceshort}} via un programme, reportez-vous au tableau suivant pour déterminer les noeuds finaux d'API à utiliser lorsque vous vous connectez à l'[API {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/apidocs/key-protect) : 

<table>
    <tr>
        <th>Emplacement</th>
        <th>Noeud final d'API de service</th>
    </tr>
    <tr>
        <td>Dallas</td>
        <td>
            <code>us-south.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Washington DC</td>
        <td>
            <code>us-east.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Londres</td>
        <td>
            <code>eu-gb.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Francfort</td>
        <td>
            <code>eu-de.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Sydney</td>
        <td>
            <code>au-syd.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <tr>
        <td>Tokyo</td>
        <td>
            <code>jp-tok.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Tableau 1. Noeuds finaux disponibles pour l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Vous pouvez continuer à utiliser `https://keyprotect.<region>.bluemix.net` pour cibler le service pour les opérations ou mettre à jour vos applications avec les nouveaux points finaux `cloud.ibm.com`.
{: tip}

Pour plus d'informations sur l'authentification auprès de {{site.data.keyword.keymanagementserviceshort}}, voir la rubrique [Accès à l'API](/docs/services/key-protect?topic=key-protect-set-up-api).

---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

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

# Regiões e terminais
{: #regions}

É possível conectar seus aplicativos com o serviço do {{site.data.keyword.keymanagementservicelong}}
especificando um endpoint de serviço regional.
{: shortdesc}

## Regiões disponíveis
{: #available-regions}

O {{site.data.keyword.keymanagementserviceshort}} está disponível nas regiões e nos locais a seguir:
![A imagem mostra as regiões nas quais o serviço do Key Protect está disponível.](images/world-map_min.svg)

## Terminais de serviço
{: #service-endpoints}

Se você estiver gerenciando seus recursos do {{site.data.keyword.keymanagementserviceshort}} programaticamente, consulte a tabela a seguir para determinar os terminais de API a serem usados quando você se conectar à API do [{{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/apidocs/key-protect): 

<table>
    <tr>
        <th>Localização</th>
        <th>Serviço de API</th>
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
        <td>Frankfurt</td>
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
        <td>Tóquio</td>
        <td>
            <code>jp-tok.kms.cloud.ibm.com</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Tabela 1. Mostra os terminais disponíveis para a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

É possível continuar a usar `https://keyprotect.<region>.bluemix.net` para destinar o serviço para as operações ou é possível atualizar os seus aplicativos com os novos terminais `cloud.ibm.com`.
{: tip}

Para obter mais informações sobre a autenticação com o {{site.data.keyword.keymanagementserviceshort}}, veja [Acessando a API](/docs/services/key-protect?topic=key-protect-set-up-api).

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

# Regiões e locais
{: #regions}

É possível conectar seus aplicativos com o serviço do {{site.data.keyword.keymanagementservicelong_notm}}
especificando um endpoint de serviço regional.
{: shortdesc}

## Regiões disponíveis
{: #available-regions}

O {{site.data.keyword.keymanagementserviceshort}} está disponível nas regiões e nos locais a seguir:
![A imagem mostra as regiões nas quais o serviço do Key Protect está disponível.](images/world-map_min.svg)

## Terminais de serviço
{: #service-endpoints}

Se você estiver gerenciando seus recursos do {{site.data.keyword.keymanagementserviceshort}}
programaticamente, consulte a tabela a seguir para determinar os terminais de API para usar ao se conectar com a API do [{{site.data.keyword.keymanagementserviceshort}}](https://console.bluemix.net/apidocs/key-protect): 

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

É possível continuar a usar `https://keyprotect.<region>.bluemix.net` para destinar o serviço para
as operações ou é possível atualizar seus aplicativos com os novos terminais `cloud.ibm.com`. Para as instâncias de serviço do {{site.data.keyword.keymanagementserviceshort}} que existem dentro de uma organização ou espaço do Cloud Foundry, use o terminal de legado `https://ibm-key-protect.edge.bluemix.net` para interagir com a API do {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

Para obter mais informações sobre a autenticação com o {{site.data.keyword.keymanagementserviceshort}}, veja [Acessando a API](/docs/services/key-protect/access-api.html).

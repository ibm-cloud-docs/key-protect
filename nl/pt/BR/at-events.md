---

copyright:
  years: 2016, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}

# Eventos do {{site.data.keyword.cloudaccesstrailshort}}
{: #at-events}

Use o serviço {{site.data.keyword.cloudaccesstrailfull}} para controlar como os usuários e os aplicativos interagem com o {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

O serviço {{site.data.keyword.cloudaccesstrailfull_notm}} registra atividades iniciadas pelo usuário que mudam o estado de um serviço no {{site.data.keyword.cloud_notm}}. 

Para obter mais informações, veja a [Documentação do {{site.data.keyword.cloudaccesstrailshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla){: new_window}.

## Lista de eventos
{: #events}

A tabela a seguir lista as ações que geram um evento:

<table>
    <tr>
        <th>Ações</th>
        <th>Descrição</th>
    </tr>
    <tr>
        <td>kms.secrets.create</td>
        <td>Criar uma chave</td>
    </tr>
    <tr>
        <td>kms.secrets.read</td>
        <td>Recuperar uma chave por ID</td>
    </tr>
   <tr>
        <td>kms.secrets.delete</td>
        <td>Excluir uma chave por ID</td>
    </tr>
    <tr>
        <td>kms.secrets.list</td>
        <td>Recuperar uma lista de chaves</td>
    </tr>
    <tr>
        <td>kms.secrets.head</td>
        <td>Recuperar o número de chaves</td>
    </tr>
     <tr>
        <td>kms.secrets.wrap</td>
        <td>Agrupar uma chave</td>
    </tr>
     <tr>
        <td>kms.secrets.unwrap</td>
        <td>Desagrupar uma chave</td>
    </tr>
    <caption style="caption-side:bottom;">Tabela 1. Ações que geram eventos do  {{site.data.keyword.cloudaccesstrailfull_notm}}</caption>
</table>

## Onde visualizar os eventos
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Os eventos do {{site.data.keyword.cloudaccesstrailshort}} estão disponíveis no
{{site.data.keyword.cloudaccesstrailshort}} **domínio de contas** disponível na região do
{{site.data.keyword.cloud_notm}} na qual eles são gerados.

Por exemplo, ao criar, importar, excluir ou ler uma chave no {{site.data.keyword.keymanagementserviceshort}}, um evento do {{site.data.keyword.cloudaccesstrailshort}} é gerado. Esses eventos são encaminhados automaticamente para o serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma
região em que o serviço {{site.data.keyword.keymanagementserviceshort}} é provisionado.

Para monitorar a atividade da API, deve-se provisionar o serviço {{site.data.keyword.cloudaccesstrailshort}} em um espaço que está disponível na
mesma região em que o serviço {{site.data.keyword.keymanagementserviceshort}} é
provisionado. Em seguida, se você tiver um plano Lite, será possível visualizar eventos por meio da visualização da
conta na IU do {{site.data.keyword.cloudaccesstrailshort}} e, se tiver um plano Premium, por meio do Kibana.

## Informações Adicionais
{: #info}

Devido à sensibilidade das informações de uma chave de criptografia, quando um
evento é gerado como resultado de uma chamada API para o serviço {{site.data.keyword.keymanagementserviceshort}}, o evento que é gerado não inclui
informações detalhadas sobre a chave. O evento inclui um ID de correlação que pode ser usado para identificar a chave internamente em seu ambiente de nuvem. O ID de correlação é um campo retornado como parte do campo `responseHeader.content`. É possível usar essas informações para correlacionar a chave do
{{site.data.keyword.keymanagementserviceshort}} com as informações da ação relatada
por meio do evento do {{site.data.keyword.cloudaccesstrailshort}}.

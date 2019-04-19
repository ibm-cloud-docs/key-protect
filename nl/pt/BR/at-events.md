---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Eventos do {{site.data.keyword.cloudaccesstrailshort}}
{: #activity-tracker-events}

Use o serviço {{site.data.keyword.cloudaccesstrailfull}} para controlar como os usuários e os aplicativos interagem com o {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

O serviço {{site.data.keyword.cloudaccesstrailfull_notm}} registra atividades iniciadas pelo usuário que mudam o estado de um serviço no {{site.data.keyword.cloud_notm}}. 

Para obter mais informações, consulte a [Documentação do {{site.data.keyword.cloudaccesstrailshort}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started#getting-started){: new_window}.

## Lista de eventos
{: #list-activity-tracker-events}

A tabela a seguir lista as ações que geram um evento:

| Ações               | Descrição                 |
| -------------------- | --------------------------- |
| `kms.secrets.create` | Criar uma chave                |
| `kms.secrets.read`   | Recuperar uma chave por ID        |
| `kms.secrets.delete` | Excluir uma chave por ID          |
| `kms.secrets.list`   | Recuperar uma lista de chaves     |
| `kms.secrets.head`   | Recuperar o número de chaves |
| `kms.secrets.wrap`   | Agrupar uma chave                  |
| `kms.secrets.unwrap` | Desagrupar uma chave                |
| `kms.policies.read`  | Visualizar uma política para uma chave     |
| `kms.policies.write` | Configurar uma política para uma chave      |
{: caption="Tabela 1. Ações que geram eventos do  {{site.data.keyword.cloudaccesstrailfull_notm}}" caption-side="top"}

## Onde visualizar os eventos
{: #view-activity-tracker-events}

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
{: #activity-tracker-info}

Devido à sensibilidade das informações de uma chave de criptografia, quando um
evento é gerado como resultado de uma chamada API para o serviço {{site.data.keyword.keymanagementserviceshort}}, o evento que é gerado não inclui
informações detalhadas sobre a chave. O evento inclui um ID de correlação que pode ser usado para identificar a chave internamente em seu ambiente de nuvem. O ID de correlação é um campo retornado como parte do campo `responseHeader.content`. É possível usar essas informações para correlacionar a chave do
{{site.data.keyword.keymanagementserviceshort}} com as informações da ação relatada
por meio do evento do {{site.data.keyword.cloudaccesstrailshort}}.

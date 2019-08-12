---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Eventos do Activity Tracker
{: #at-events}

Como um responsável pela segurança, auditor ou gerente, é possível usar o serviço do Activity Tracker para controlar como os usuários e aplicativos interagem com o {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

O Activity Tracker registra atividades iniciadas pelo usuário que mudam o estado de um serviço no {{site.data.keyword.cloud_notm}}. É possível usar esse serviço para investigar atividade anormal e ações críticas e para obedecer aos requisitos de auditoria regulamentares. Além disso, é possível ser alertado sobre ações conforme elas acontecem. Os eventos que são coletados obedecem ao padrão Cloud Auditing Data Federation (CADF). 

Atualmente, existem dois serviços do Activity Tracker que estão disponíveis no catálogo do {{site.data.keyword.cloud_notm}}. O {{site.data.keyword.keymanagementserviceshort}} envia eventos para ambos os serviços e é possível usar qualquer um dos serviços para monitorar a sua atividade do {{site.data.keyword.keymanagementserviceshort}} no {{site.data.keyword.cloud_notm}}. No entanto, o {{site.data.keyword.cloudaccesstrailfull}} foi descontinuado e nenhuma nova instância pode ser criada e o suporte para as instâncias de serviço existentes estará disponível apenas até 30 de setembro de 2019.

Para obter mais informações, consulte:
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (descontinuado)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## Lista de eventos
{: #at-actions}

A tabela a seguir lista as ações do {{site.data.keyword.keymanagementserviceshort}} que geram um evento:

| Ações                   | Descrição                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | Criar uma chave                |
| `kms.secrets.read`       | Recuperar uma chave por ID        |
| `kms.secrets.delete`     | Excluir uma chave por ID          |
| `kms.secrets.list`       | Recuperar uma lista de chaves     |
| `kms.secrets.head`       | Recuperar o número de chaves |
| `kms.secrets.wrap`       | Agrupar uma chave                  |
| `kms.secrets.unwrap`     | Desagrupar uma chave                |
| `kms.policies.read`      | Visualizar uma política para uma chave     |
| `kms.policies.write`     | Configurar uma política para uma chave      |
{: caption="Tabela 1. Ações do {{site.data.keyword.keymanagementserviceshort}} que geram eventos do Activity Tracker" caption-side="top"}

## Visualização de eventos
{: #at-ui}

É possível visualizar os eventos do Activity Tracker que estão associados à sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} usando o [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) ou o [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (descontinuado).

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### Usando o {{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

Os eventos que são gerados por uma instância do {{site.data.keyword.keymanagementserviceshort}} são encaminhados automaticamente para a instância de serviço do {{site.data.keyword.at_full_notm}} que está disponível no mesmo local. 

O {{site.data.keyword.at_full_notm}} pode ter apenas uma instância por local. Para visualizar eventos, deve-se acessar a IU da web do serviço do {{site.data.keyword.at_full_notm}} no mesmo local em que a sua instância de serviço estiver disponível. Para obter mais informações, consulte [Ativando a IU da web por meio da IU do IBM Cloud](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### Usando o {{site.data.keyword.cloudaccesstrailfull_notm}} (descontinuado)
{: #at-ui-legacy}

Os eventos do {{site.data.keyword.cloudaccesstrailshort}} estão disponíveis no
{{site.data.keyword.cloudaccesstrailshort}} **domínio de contas** disponível na região do
{{site.data.keyword.cloud_notm}} na qual eles são gerados. Para obter mais informações, consulte [Visualizando eventos](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4).


## Analisando eventos
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

Devido à sensibilidade das informações de uma chave de criptografia, quando um
evento é gerado como resultado de uma chamada API para o serviço {{site.data.keyword.keymanagementserviceshort}}, o evento que é gerado não inclui
informações detalhadas sobre a chave. O evento inclui um ID de correlação que pode ser usado para identificar a chave internamente em seu ambiente de nuvem. O ID de correlação é um campo que é retornado como parte do campo `responseBody.content`. É possível usar essas informações para correlacionar a chave do
{{site.data.keyword.keymanagementserviceshort}} com as informações da ação relatada
por meio do evento do {{site.data.keyword.cloudaccesstrailshort}}.

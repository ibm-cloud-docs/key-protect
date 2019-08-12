---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: encryption key states, encryption key lifecycle, manage key lifecycle

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

# Monitorando o ciclo de vida de chaves de criptografia
{: #key-states}

O {{site.data.keyword.keymanagementservicefull}} segue as diretrizes de segurança por [NIST SP 800-57 para estados de chave](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: external}.
{: shortdesc}

## Estados e transições de chave
{: #key-transitions}

As chaves criptográficas, durante seu tempo de vida, fazem a transição por vários estados que demonstram há quanto tempo as chaves existem e se os dados
estão protegidos. 

O {{site.data.keyword.keymanagementserviceshort}} fornece uma interface gráfica com o usuário e uma API de REST para rastrear as chaves à medida que elas passam por vários estados em seus ciclos de vida. O diagrama a seguir mostra como uma chave passa pelos estados entre sua geração e sua destruição.

![O diagrama mostra os mesmos componentes que os descritos na tabela de definição a seguir.](../images/key-states_min.svg)

| Estado | Descrição |
| --- | --- |
| Pré-ativação | As chaves são criadas inicialmente no estado _pré-ativação_. Uma chave pré-ativa não pode ser usada para proteger os dados criptograficamente.|
| Ativo | As chaves são movidas imediatamente para o estado _ativo_ na data de ativação. Essa transação marca o início do período de criptografia de uma chave. As chaves sem data de ativação se tornam ativas imediatamente e permanecem ativas até que expirem ou sejam destruídas. |
| Desativado | Uma chave será movida para o estado _desativado_ na data de expiração, se uma estiver designada. Nesse estado, a chave é incapaz de proteger os dados de forma criptográfica e pode ser movida somente para o estado _destruído_.|
| Destruído | As chaves excluídas estão no estado _destruído_. As chaves nesse estado não são recuperáveis. Metadados que estão associados a uma chave, como o nome e o histórico de transição da chave, são mantidos no banco de dados do {{site.data.keyword.keymanagementserviceshort}}. |
{: caption="Tabela 1. Descreve as transições e os estados da chave." caption-side="top"}

Depois de incluir uma chave para o serviço, use o painel {{site.data.keyword.keymanagementserviceshort}} ou as APIs de REST do {{site.data.keyword.keymanagementserviceshort}} para visualizar o histórico de transição e a configuração da sua chave. Para propósitos de auditoria, também é possível monitorar a trilha de atividade para uma chave integrando o {{site.data.keyword.keymanagementserviceshort}} com o {{site.data.keyword.cloudaccesstrailfull}}. Depois que ambos os serviços são provisionados e estão em execução, eventos de atividade são gerados e coletados automaticamente em um log do {{site.data.keyword.cloudaccesstrailshort}} quando você cria e exclui chaves no {{site.data.keyword.keymanagementserviceshort}}. 

Para obter mais informações, consulte [Monitorando a atividade do {{site.data.keyword.keymanagementserviceshort}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-kp){: external}.

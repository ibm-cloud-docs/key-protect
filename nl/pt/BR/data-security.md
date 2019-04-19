---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Segurança e conformidade
{: #security-and-compliance}

O {{site.data.keyword.keymanagementservicefull}} tem estratégias de segurança de dados em vigor para atender às suas necessidades de conformidade e assegurar que seus dados permaneçam seguros e protegidos na nuvem.
{: shortdesc}

## Segurança e criptografia de dados
{: #data-security}

O {{site.data.keyword.keymanagementserviceshort}} usa os [hardware security modules (HSMs) do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/hardware-security-module) para gerar material de chave gerenciado pelo fornecedor e executar as operações de [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). Os HSMs são dispositivos de hardware resistentes à violação que armazenam e usam material de chave criptográfica sem expor chaves fora de um limite criptográfico.

O acesso ao serviço ocorre por meio de HTTPS, e a comunicação interna do {{site.data.keyword.keymanagementserviceshort}} usa o protocolo de Segurança da Camada de Transporte (TLS) 1.2 para criptografar dados em trânsito.

Para saber mais sobre como o {{site.data.keyword.keymanagementserviceshort}} atende aos seus requisitos de conformidade, confira [Conformidade de plataforma e de serviço](/docs/overview?topic=overview-security#compliancetable).

## Exclusão de dados
{: #data-deletion}

Quando você exclui uma chave, o serviço marca a chave como excluída e as transições de chave para o estado _Destruído_. As chaves nesse estado não são mais recuperáveis, e os serviços de nuvem que usam a chave não podem mais decriptografar dados que estão associados à chave. Seus dados permanecem nesses serviços em seu formato criptografado. Metadados que estão associados a uma chave, como o nome e o histórico de transição da chave, são mantidos no banco de dados do {{site.data.keyword.keymanagementserviceshort}}. 

A exclusão de uma chave no {{site.data.keyword.keymanagementserviceshort}} é uma operação destrutiva. Lembre-se de que depois de excluir uma chave, a ação não pode ser revertida, e quaisquer dados associados à chave são perdidos imediatamente no momento em que a chave é excluída. Antes de excluir uma chave, revise os dados que estão associados à chave e assegure-se de que você não precise mais de acesso a ela. Não exclua uma chave que esteja ativamente protegendo dados em seus ambientes de produção. 

Para ajudá-lo a determinar quais dados estão protegidos por uma chave, é possível visualizar como a instância de serviço do {{site.data.keyword.keymanagementserviceshort}} mapeia para os serviços de nuvem existentes executando `ibmcloud iam authorization-policies`. Para saber mais sobre a visualização de autorizações de serviço por meio do console, consulte [Concedendo acesso entre serviços](/docs/iam?topic=iam-serviceauth).
{: note}

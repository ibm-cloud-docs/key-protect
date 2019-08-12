---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Segurança e conformidade
{: #security-and-compliance}

O {{site.data.keyword.keymanagementservicefull}} tem estratégias de segurança de dados em vigor para atender às suas necessidades de conformidade e assegurar que seus dados permaneçam seguros e protegidos na nuvem.
{: shortdesc}

## Disponibilidade de segurança
{: #security-ready}

O {{site.data.keyword.keymanagementserviceshort}} assegura a disponibilidade de segurança aderindo às melhores práticas do {{site.data.keyword.IBM_notm}} para sistemas, rede e engenharia segura. 

Para saber mais sobre controles de segurança no {{site.data.keyword.cloud_notm}}, consulte [Como eu sei se os meus dados estão seguros?](/docs/overview?topic=overview-security#security).
{: tip}

### Criptografia de Dados
{: #data-encryption}

O {{site.data.keyword.keymanagementserviceshort}} usa os [módulos de segurança de hardware (HSMs) do {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/hardware-security-module){: external} para gerar o material chave gerenciado pelo provedor e executar operações de [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). Os HSMs são dispositivos de hardware resistentes à violação que armazenam e usam material de chave criptográfica sem expor chaves fora de um limite criptográfico.

O acesso ao serviço ocorre por meio de HTTPS, e a comunicação interna do {{site.data.keyword.keymanagementserviceshort}} usa o protocolo de Segurança da Camada de Transporte (TLS) 1.2 para criptografar dados em trânsito.

### Exclusão de dados
{: #data-deletion}

Quando você exclui uma chave, o serviço marca a chave como excluída e as transições de chave para o estado _Destruído_. As chaves nesse estado não são mais recuperáveis, e os serviços de nuvem que usam a chave não podem mais decriptografar dados que estão associados à chave. Seus dados permanecem nesses serviços em seu formato criptografado. Metadados que estão associados a uma chave, como o nome e o histórico de transição da chave, são mantidos no banco de dados do {{site.data.keyword.keymanagementserviceshort}}. 

A exclusão de uma chave no {{site.data.keyword.keymanagementserviceshort}} é uma operação destrutiva. Lembre-se de que depois de excluir uma chave, a ação não pode ser revertida, e quaisquer dados associados à chave são perdidos imediatamente no momento em que a chave é excluída. Antes de excluir uma chave, revise os dados que estão associados à chave e assegure-se de que você não precise mais de acesso a ela. Não exclua uma chave que esteja ativamente protegendo dados em seus ambientes de produção. 

Para ajudá-lo a determinar quais dados estão protegidos por uma chave, é possível visualizar como a instância de serviço do {{site.data.keyword.keymanagementserviceshort}} mapeia para os serviços de nuvem existentes executando `ibmcloud iam authorization-policies`. Para saber mais sobre a visualização de autorizações de serviço por meio do console, consulte [Concedendo acesso entre serviços](/docs/iam?topic=iam-serviceauth).
{: note}

## Disponibilidade de conformidade
{: #compliance-ready}

O {{site.data.keyword.keymanagementserviceshort}} atende aos controles para padrões de conformidade globais, de segmento de mercado e regionais, incluindo GDPR, HIPAA e ISO 27001/27017/27018 e outros. 

Para obter uma listagem completa de certificações de conformidade do {{site.data.keyword.cloud_notm}}, consulte [Conformidade no {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### Suporte da UE
{: #eu-support}

O {{site.data.keyword.keymanagementserviceshort}} tem controles extras no local para proteger os seus recursos do {{site.data.keyword.keymanagementserviceshort}} na União Europeia (UE). 

Se você usar recursos do {{site.data.keyword.keymanagementserviceshort}} na região de Frankfurt, na Alemanha, para processar dados pessoais para cidadãos europeus, será possível ativar a configuração Suportada pela UE para a sua conta do {{site.data.keyword.cloud_notm}}. Para descobrir mais, consulte [Ativando a configuração Suportada pela UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) e [Solicitando suporte para recursos na União Europeia](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### Regulamento Geral sobre a Proteção de Dados (GDPR)
{: #gdpr}

O GDPR visa criar uma estrutura de lei de proteção de dados harmonizados em toda a UE e pretende devolver aos cidadãos o controle de seus dados pessoais, enquanto impõe regras rígidas sobre a hospedagem e o processamento desses dados, em qualquer lugar do mundo.

A {{site.data.keyword.IBM_notm}} é comprometida em fornecer a nossos clientes e Parceiros de Negócios {{site.data.keyword.IBM_notm}} soluções inovadoras de privacidade de dados, segurança e controle para auxiliá-los em sua jornada para a prontidão de GDPR.

Para assegurar a conformidade de GDPR para os seus recursos do {{site.data.keyword.keymanagementserviceshort}}, [ative a configuração suportada pela UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) para sua conta do {{site.data.keyword.cloud_notm}}. É possível aprender mais sobre como o {{site.data.keyword.keymanagementserviceshort}} processa e protege dados pessoais, revendo os complementos a seguir.

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### Suporte a HIPAA
{: #hipaa-ready}

O {{site.data.keyword.keymanagementserviceshort}} atende aos controles para o Health Insurance Portability and Accountability Act (HIPAA) dos EUA para assegurar a proteção de informação protegida de saúde (PHI). 

Se você ou a sua empresa for uma entidade coberta conforme definido pelo HIPAA, será possível ativar a configuração Suportada por HIPPA para a sua conta do {{site.data.keyword.cloud_notm}}. Para descobrir mais, consulte [Ativando a configuração suportada por HIPAA](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

O {{site.data.keyword.keymanagementserviceshort}} é certificado pelo ISO 27001, 27017, 27018. É possível visualizar certificações de conformidade visitando [Conformidade no {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}. 

### SOC 2 Tipo 1
{: #soc2-type1}

O {{site.data.keyword.keymanagementserviceshort}} é certificado pelo SOC 2 Tipo 1. Para obter informações sobre como solicitar um relatório do {{site.data.keyword.cloud_notm}} SOC 2, consulte [Conformidade no {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

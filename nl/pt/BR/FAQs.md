---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# Perguntas mais frequentes
{: #faqs}

É possível usar as seguintes Perguntas mais frequentes para ajudá-lo com o {{site.data.keyword.keymanagementservicelong}}.

## Como a precificação funciona para o {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

O {{site.data.keyword.keymanagementserviceshort}} oferece um [plano de camada graduada](https://{DomainName}/catalog/services/key-protect) sem encargos para usuários que requererem 20 ou menos chaves. É possível ter 20 chaves grátis por conta do {{site.data.keyword.cloud_notm}}. Se a sua equipe requerer múltiplas instâncias do {{site.data.keyword.keymanagementserviceshort}}, o {{site.data.keyword.cloud_notm}} incluirá suas chaves ativas em todas as instâncias dentro da conta e, em seguida, aplicará a precificação. 

## O que é uma chave de criptografia ativa?
{: #what-is-active-encryption-key}
{: faq}

Ao importar chaves de criptografia para o {{site.data.keyword.keymanagementserviceshort}} ou quando você usar o {{site.data.keyword.keymanagementserviceshort}} para gerar chaves de seus HSMs, elas se tornarão chaves _ativas_. A precificação é baseada em todas as chaves ativas dentro de uma conta do {{site.data.keyword.cloud_notm}}. 

## Como devo agrupar e gerenciar minhas chaves?
{: #how-to-group-keys}
{: faq}

Do ponto de vista de precificação, a melhor maneira de usar o {{site.data.keyword.keymanagementserviceshort}} é criar um número limitado de chaves raiz e, em seguida, usá-las para criptografar as chaves de criptografia de dados que são criadas por um app externo ou serviço de dados em nuvem. 

Para descobrir mais sobre o uso de chaves raiz para proteger chaves de criptografia de dados, confira [Protegendo dados com criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## O que é uma chave raiz?
{: #what-is-root-key}
{: faq}

Chaves raiz são recursos primários no {{site.data.keyword.keymanagementserviceshort}}. Elas são chaves simétricas de quebra de chaves usadas como raízes de confiança para proteger outras chaves que são armazenadas em um serviço de dados com [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). Com o {{site.data.keyword.keymanagementserviceshort}}, é possível criar,
armazenar e gerenciar o ciclo de vida das chaves raiz para obter controle total das outras chaves armazenadas na nuvem. 

## O que é criptografia de envelope?
{: #what-is-envelope-encryption}
{: faq}

A criptografia de envelope é a prática de criptografar dados com uma _chave de criptografia de dados_ e, em seguida, criptografar a chave de criptografia de dados com uma _chave de quebra de chave_ altamente segura.  Seus dados são protegidos em repouso ao aplicar várias camadas de criptografia. Para saber como ativar a criptografia de envelope para seus recursos do {{site.data.keyword.cloud_notm}}, confira [Integrando serviços](/docs/services/key-protect?topic=key-protect-integrate-services).

## Qual é o limite de comprimento de um nome da chave?
{: #key-names}
{: faq}

É possível usar um nome de chave que tenha até 90 caracteres de comprimento.

## Posso armazenar informações pessoais como metadados para minhas chaves?
{: #personal-data}
{: faq}

Para proteger a confidencialidade de seus dados pessoais, não armazene informações pessoalmente identificáveis (PII) como metadados para suas chaves. As informações pessoais incluem seu nome, endereço, número do telefone, endereço de e-mail ou outras informações que possam identificar, contatar ou localizar você, seus clientes ou qualquer outra pessoa.

Você é responsável por assegurar a segurança de quaisquer informações que você armazene como metadados para recursos do {{site.data.keyword.keymanagementserviceshort}} e chaves de criptografia. Para obter mais exemplos de dados pessoais, consulte a seção 2.2 da [NIST Special Publication 800-122](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: external}.
{: important}

## As chaves que são criadas em uma região podem ser usadas em outra região?
{: #keys-across-regions}
{: faq}

Suas chaves de criptografia estão limitadas à região em que foram criadas. O {{site.data.keyword.keymanagementserviceshort}} não copia ou exporta chaves de criptografia para outras regiões.

## Como controlar quem tem acesso às chaves?
{: #access-control}
{: faq}

O {{site.data.keyword.keymanagementserviceshort}} suporta um sistema de controle de acesso centralizado, governado pelo {{site.data.keyword.iamlong}}, para ajudar a gerenciar usuários e o acesso às suas chaves de criptografia. Se você for um administrador de segurança do seu serviço, será possível designar as [funções do Cloud IAM que correspondam às permissões específicas do {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-manage-access#roles) que deseja conceder aos membros de sua equipe.

## Como monitoro chamadas API para o {{site.data.keyword.keymanagementserviceshort}}?
{: faq}

É possível usar o serviço {{site.data.keyword.cloudaccesstrailfull_notm}} para controlar como os usuários e aplicativos interagem com sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Por exemplo, ao criar, importar, excluir ou ler uma chave no {{site.data.keyword.keymanagementserviceshort}}, um evento do {{site.data.keyword.cloudaccesstrailshort}} é gerado. Esses eventos são encaminhados automaticamente para o serviço {{site.data.keyword.cloudaccesstrailshort}} na mesma
região em que o serviço {{site.data.keyword.keymanagementserviceshort}} é provisionado.

Para descobrir mais, confira os [eventos do Activity Tracker](/docs/services/key-protect?topic=key-protect-at-events).

## O que acontece quando eu excluo uma chave?
{: #key-destruction}
{: faq}

Quando você exclui uma chave, o serviço marca a chave como excluída e as transições de chave para o estado _Destruído_. As chaves nesse estado não são mais recuperáveis, e os serviços de nuvem que usam a chave não podem mais decriptografar dados que estão associados à chave. Seus dados permanecem nesses serviços em seu formato criptografado. Metadados que estão associados a uma chave, como o nome e o histórico de transição da chave, são mantidos no banco de dados do {{site.data.keyword.keymanagementserviceshort}}. 

Antes de excluir uma chave, verifique se você não precisa mais de acesso a algum dado que esteja associado à chave. Essa ação não pode ser revertida.

## O que acontece quando eu preciso desprover minha instância de serviço?
{: #deprovision-service}
{: faq}

Se você decidir mudar do {{site.data.keyword.keymanagementserviceshort}}, deverá excluir qualquer chave restante de sua instância de serviço antes de poder desprover o serviço. Depois de excluir sua instância de serviço, o {{site.data.keyword.keymanagementserviceshort}} usará a [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption) para cripto-fragmentar quaisquer dados que estejam associados à instância de serviço. 


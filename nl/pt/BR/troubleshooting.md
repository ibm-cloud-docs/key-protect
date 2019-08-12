---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# Detectando Problemas
{: #troubleshooting}

Problemas gerais com o uso do {{site.data.keyword.keymanagementservicefull}} podem incluir o fornecimento de cabeçalhos ou credenciais corretos quando você interage com a API. Em muitos casos, é possível recuperar-se desses problemas seguindo algumas etapas simples.
{: shortdesc}

## Não é possível criar ou excluir chaves
{: #unable-to-create-keys}

Ao acessar a interface com o usuário do {{site.data.keyword.keymanagementserviceshort}}, você não vê as opções para incluir ou excluir chaves.

No painel do {{site.data.keyword.cloud_notm}}, selecione sua instância do serviço {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

É possível ver uma lista de chaves, mas opções para incluir ou excluir chaves não são exibidas. 

Você não tem a autorização correta para executar ações do {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifique com o seu administrador se você está designado com a função correta no grupo de recursos aplicável ou na instância de serviço. Para obter mais informações sobre funções, veja [Funções e permissões](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Obtendo ajuda e suporte
{: #getting-help}

Se você tiver problemas ou perguntas quando estiver usando o {{site.data.keyword.keymanagementserviceshort}}, poderá verificar o {{site.data.keyword.cloud_notm}} ou obter ajuda procurando informações ou fazendo perguntas por meio de um fórum. Também é possível abrir um chamado de suporte.
{: shortdesc}

É possível verificar se o {{site.data.keyword.cloud_notm}} está disponível acessando a [página de status](https://{DomainName}/status?tags=platform,runtimes,services){: external}.

Você pode revisar os fóruns para ver se outros usuários tiveram o mesmo problema. Quando estiver usando os fóruns para fazer uma pergunta, identifique sua pergunta para que ela seja vista pela equipe de desenvolvimento do {{site.data.keyword.cloud_notm}}.

- Se você tiver questões técnicas sobre o {{site.data.keyword.keymanagementserviceshort}}, poste a sua pergunta no [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} e identifique a sua pergunta com "ibm-cloud" e "key-protect".
- Para perguntas sobre o serviço e instruções de introdução, use o
fórum [IBM
developerWorks dW Answers](https://developer.ibm.com/answers/topics/key-protect/){: external}. Inclua "ibm-cloud" e "key-protect" tags.

Consulte [Obtendo suporte](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external} para obter mais detalhes sobre o uso dos fóruns.

Para obter mais informações sobre como abrir um chamado de suporte do {{site.data.keyword.IBM_notm}} ou sobre níveis de suporte e severidades de chamado, consulte [Contatando o suporte](/docs/get-support?topic=get-support-getting-customer-support){: external}.

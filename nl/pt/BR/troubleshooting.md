---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Detectando Problemas
{: #troubleshooting}

Problemas gerais com o uso do {{site.data.keyword.keymanagementservicefull}} podem incluir o fornecimento de cabeçalhos ou credenciais corretos quando você interage com a API. Em muitos casos, é possível recuperar-se desses problemas seguindo algumas etapas simples.
{: shortdesc}

## Não é possível acessar a interface com o usuário
{: #unable-to-access-ui}

Ao acessar a interface com o usuário do {{site.data.keyword.keymanagementserviceshort}}, a UI não é carregada conforme o esperado.

No painel do {{site.data.keyword.cloud_notm}}, selecione sua instância do serviço {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Você verá o seguinte erro: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

Em 15 de dezembro de 2017, incluímos novos recursos, como [criptografia de envelope](/docs/services/key-protect/concepts/envelope-encryption.html), no serviço do {{site.data.keyword.keymanagementserviceshort}}. Agora é possível provisionar o serviço do {{site.data.keyword.keymanagementserviceshort}} globalmente, sem precisar especificar uma organização e um espaço do Cloud Foundry.
{: tsCauses}

Essas mudanças afetaram a interface com o usuário para as instâncias mais velhas do serviço. Se você criou a sua instância do {{site.data.keyword.keymanagementserviceshort}} antes de 28 de setembro de 2017, a interface com o usuário poderá não funcionar conforme o esperado.

Estamos trabalhando para corrigir esse problema. Como uma solução temporária, é possível continuar a gerenciar as suas chaves usando a API do {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

É possível usar o terminal de legado `https://ibm-key-protect.edge.bluemix.net` para interagir com o serviço do {{site.data.keyword.keymanagementserviceshort}}.

**Exemplo de solicitação**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
```
{: codeblock}

## Não é possível criar ou excluir chaves
{: #unable-to-create-keys}

Ao acessar a interface com o usuário do {{site.data.keyword.keymanagementserviceshort}}, você não vê as opções para incluir ou excluir chaves.

No painel do {{site.data.keyword.cloud_notm}}, selecione sua instância do serviço {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

É possível ver uma lista de chaves, mas opções para incluir ou excluir chaves não são exibidas. 

Você não tem a autorização correta para executar ações do {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifique com o seu administrador se você está designado com a função correta no grupo de recursos aplicável ou na instância de serviço. Para obter mais informações sobre funções, veja [Funções e permissões](/docs/services/key-protect/manage-access.html#roles).
{: tsResolve}

## Obtendo ajuda e suporte
{: #getting-help}

Se você tiver problemas ou perguntas quando estiver usando o {{site.data.keyword.keymanagementserviceshort}}, poderá verificar o {{site.data.keyword.cloud_notm}} ou obter ajuda procurando informações ou fazendo perguntas por meio de um fórum. Também é possível abrir um chamado de suporte.
{: shortdesc}

É possível verificar se o {{site.data.keyword.cloud_notm}} está disponível acessando a página de status do [ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/status?tags=platform,runtimes,services).

Você pode revisar os fóruns para ver se outros usuários tiveram o mesmo problema. Quando estiver usando os fóruns para fazer uma pergunta, identifique sua pergunta para que ela seja vista pela equipe de desenvolvimento do {{site.data.keyword.cloud_notm}}.

- Se você tiver questões técnicas sobre o {{site.data.keyword.keymanagementserviceshort}}, poste a sua pergunta no [Estouro de pilha ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} e identifique a sua pergunta com "ibm-cloud" e "key-protect".
- Para perguntas sobre o serviço e instruções de introdução, use o fórum [IBM developerWorks dW Answers ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window}. Inclua "ibm-cloud" e "key-protect" tags.

Consulte [Obtendo ajuda![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window} para obter mais detalhes sobre o uso dos fóruns.

Para obter mais informações sobre como abrir um chamado de suporte do {{site.data.keyword.IBM_notm}} ou sobre níveis de suporte e severidades de chamado, veja [Entrando em contato com o suporte ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.

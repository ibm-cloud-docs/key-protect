---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

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
{:note: .note}
{:important: .important}

# Detectando Problemas
{: #troubleshooting}

Problemas gerais com o uso do {{site.data.keyword.keymanagementservicefull}} podem incluir o fornecimento de cabeçalhos ou credenciais corretos quando você interage com a API. Em muitos casos, é possível recuperar-se desses problemas seguindo algumas etapas simples.
{: shortdesc}

## Não é possível excluir minha instância de serviço do Cloud Foundry
{: #unable-to-delete-service}

Quando você tenta excluir a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}, o
serviço falha ao excluir, conforme esperado.

Por meio do painel do {{site.data.keyword.cloud_notm}}, navegue para os **serviços do Cloud
Foundry** e selecione a sua instância do {{site.data.keyword.keymanagementserviceshort}}. Clique no
ícone ? para abrir uma lista de opções para a oferta de serviços e clica em **Excluir serviço**.
{: tsSymptoms}

O serviço falha ao excluir e o erro a seguir é exibido: 
```
403 Proibido: essa ação não pode ser concluída porque você tem segredos existentes em seu serviço Key Protect. Primeiro,
é necessário excluir os segredos antes de poder remover o serviço.
```
{: screen}

Em 15 de dezembro de 2017, o {{site.data.keyword.keymanagementserviceshort}} passou a usar o IAM e os grupos
de recursos no lugar das organizações, espaços e funções do Cloud Foundry. Agora, é possível provisionar o serviço do {{site.data.keyword.keymanagementserviceshort}} dentro de um grupo de recursos, sem precisar especificar uma organização e um espaço do Cloud Foundry.
{: tsCauses}

Essas mudanças afetaram como o desprovimento funciona para instâncias de serviço mais antigas. Se você tiver criado
sua instância do {{site.data.keyword.keymanagementserviceshort}} antes de 28 de setembro de 2017, a exclusão
do serviço poderá não funcionar conforme esperado.

Para excluir uma instância de serviço mais antiga do {{site.data.keyword.keymanagementserviceshort}}, deve-se
primeiro excluir suas chaves existentes usando o terminal `https://ibm-key-protect.edge.bluemix.net` anterior para interagir com o serviço {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Para excluir suas chaves e sua instância de serviço:

1. Efetue login no {{site.data.keyword.cloud_notm}} com a CLI do {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **Nota:** se o login falhar, execute o comando `bx login --sso` para tentar novamente. O parâmetro `--sso` é necessário quando você efetua login com um ID federado. Se essa opção for usada, acesse o link listado na saída da CLI para gerar uma senha descartável.

2. Selecione a região, organização e espaço do {{site.data.keyword.cloud_notm}} que contém sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

    Observe os nomes de sua organização e o espaço na saída da CLI. Também é possível executar `ibmcloud cf
target` para destinar a organização e o espaço do Cloud Foundry.

    ```sh
    ibmcloud cf target -o <organization_name> -s <space_name>
    ```
    {: codeblock}

3. Recupere a sua organização do {{site.data.keyword.cloud_notm}} e
GUIDs de espaço.

    ```sh
    ibmcloud iam org <organization_name> --guid
    ibmcloud iam space <space_name> --guid
    ```
    {: codeblock}
    Substitua `<organization_name>` e `<space_name>` por aliases exclusivos que você designou para a sua
organização e espaço.

4. Recupere seu token de acesso.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. Liste as chaves que estão armazenadas em sua instância de serviço executando o comando cURL a seguir.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Substitua `<access_token>`, `<organization_GUID>` e `<space_GUID>` com os valores que você recuperou nas etapas 3 a 4. 

6. Copie o valor do ID para cada chave que está armazenada em sua instância de serviço.

7. Execute o comando cURL a seguir para excluir permanentemente uma chave e seus conteúdos.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<key_ID> \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-org: <organization_GUID>' \
    -H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Substitua `<access_token>`, `<organization_GUID>`, `<space_GUID>` e `<key_ID>` pelos valores que você recuperou nas etapas 3 a 5. Repita o
comando para cada chave.    

8. Verifique se suas chaves foram excluídas executando o comando cURL a seguir.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <access_token>' \
-H 'bluemix-org: <organization_GUID>' \
-H 'bluemix-space: <space_GUID>' \
    ```
    {: codeblock}

    Substitua `<access_token>`, `<organization_GUID>` e `<space_GUID>` com os valores que você recuperou nas etapas 3 a 4. 

9. Exclua sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud cf delete-service "<service_instance_name>"
    ```
    {: codeblock}

10. Opcional: verifique se a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} foi
excluída navegando para o painel do {{site.data.keyword.cloud_notm}}.

    Também é possível listar seus serviços disponíveis do Cloud Foundry no espaço de destino executando o comando a seguir.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


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

Em 15 de dezembro de 2017, incluímos novos recursos, como [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption), no serviço do {{site.data.keyword.keymanagementserviceshort}}. Agora, é possível provisionar o serviço do {{site.data.keyword.keymanagementserviceshort}} dentro de um grupo de recursos, sem precisar especificar uma organização e um espaço do Cloud Foundry.
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

Verifique com o seu administrador se você está designado com a função correta no grupo de recursos aplicável ou na instância de serviço. Para obter mais informações sobre funções, veja [Funções e permissões](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Obtendo ajuda e suporte
{: #getting-help}

Se você tiver problemas ou perguntas quando estiver usando o {{site.data.keyword.keymanagementserviceshort}}, poderá verificar o {{site.data.keyword.cloud_notm}} ou obter ajuda procurando informações ou fazendo perguntas por meio de um fórum. Também é possível abrir um chamado de suporte.
{: shortdesc}

É possível verificar se o {{site.data.keyword.cloud_notm}} está disponível acessando a página de status do [
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/status?tags=platform,runtimes,services).

Você pode revisar os fóruns para ver se outros usuários tiveram o mesmo problema. Quando estiver usando os fóruns para fazer uma pergunta, identifique sua pergunta para que ela seja vista pela equipe de desenvolvimento do {{site.data.keyword.cloud_notm}}.

- Se você tiver questões técnicas sobre o {{site.data.keyword.keymanagementserviceshort}}, poste a sua pergunta no [Estouro de pilha ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} e identifique a sua pergunta com "ibm-cloud" e "key-protect".
- Para perguntas sobre o serviço e instruções de introdução, use o fórum [IBM developerWorks dW Answers ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/key-protect/){: new_window}. Inclua "ibm-cloud" e "key-protect" tags.

Consulte [Obtendo suporte ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window} para obter mais detalhes sobre o uso dos fóruns.

Para obter mais informações sobre como abrir um chamado de suporte do {{site.data.keyword.IBM_notm}} ou sobre níveis de suporte e severidades de chamado, veja [Entrando em contato com o suporte ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Configurando uma política de rotação
{: #set-rotation-policy}

É possível configurar uma política de rotação automática para uma chave raiz usando o {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

Ao configurar uma política de rotação automática para uma chave raiz, você encurta o tempo de vida da chave em intervalos regulares e limita a quantidade de informações que são protegidas por essa chave.

É possível criar uma política de rotação apenas para chaves raiz que são geradas no {{site.data.keyword.keymanagementserviceshort}}. Se você importou a chave raiz inicialmente, deverá fornecer o novo material de chave codificado em Base64 para girar
a chave. Para obter mais informações, consulte [Girando chaves raiz on demand](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys).
{: note}

Deseja saber mais sobre as opções de rotação de chave no {{site.data.keyword.keymanagementserviceshort}}? Confira [Comparando suas opções de rotação de chave](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options) para obter mais informações.
{: tip}

## Gerenciando polícias de rotação na GUI
{: #manage-policies-gui}

Se você preferir gerenciar políticas para suas chaves raiz usando uma interface gráfica, será possível usar a GUI do {{site.data.keyword.keymanagementserviceshort}}.

1. [Efetue login no console do {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.
2. Acesse **Menu** &gt; **Lista de recursos** para visualizar uma lista de seus recursos.
3. Em sua lista de recursos do {{site.data.keyword.cloud_notm}}, selecione a sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.
4. Na página de detalhes do aplicativo, use a tabela de **Chaves** para procurar as chaves em seu serviço.
5. Clique no ícone ⋯ para abrir uma lista de opções para uma chave específica.
6. No menu de opções, clique em **Gerenciar política** para gerenciar a política de rotação para a chave.
7. Na lista de opções de rotação, selecione uma frequência de rotação em meses.

    Se a sua chave tiver uma política de rotação existente, a interface exibirá o período de rotação existente da chave.

8. Clique em **Criar política** para configurar a política para a chave.

Quando for a hora de girar a chave com base no intervalo de rotação que você especificar, o {{site.data.keyword.keymanagementserviceshort}} substituirá automaticamente a chave raiz por um novo material de chave.

## Gerenciando políticas de rotação com a API
{: #manage-rotation-policies-api}

### Visualizando uma política de rotação
{: #view-rotation-policy-api}

Para uma visualização de alto nível, é possível procurar as políticas que estão associadas a uma chave raiz ao fazer uma chamada `GET` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Recupere a política de rotação para uma chave especificada, executando o comando cURL a seguir.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz que tem uma política de rotação existente.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Terminais regionais em serviço</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Necessário.</strong> Seu token de acesso do {{site.data.keyword.cloud_notm}}. Inclua o conteúdo integral do token <code>IAM</code>, incluindo valor Bearer, na solicitação cURL. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperando um token de acesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo que é designado para sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperando um ID da instância</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>O identificador exclusivo que é usado para rastrear e correlacionar transações.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para criar uma política de rotação com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Uma resposta `GET api/v2/keys/{id}/policies` bem-sucedida retorna detalhes da política que estão associados à sua chave. O objeto JSON a seguir mostra uma resposta de exemplo para uma chave raiz que tem uma política de rotação existente.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    O valor `interval_month` indica a frequência de rotação de chave em meses.

### Criando uma política de rotação
{: #create-rotation-policy-api}

Crie uma política de rotação para sua chave raiz, fazendo uma chamada `PUT` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Crie uma política de rotação para uma chave especificada, executando o comando cURL a seguir.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <rotation_interval>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz para a qual você deseja criar uma política de rotação.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Terminais regionais em serviço</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Necessário.</strong> Seu token de acesso do {{site.data.keyword.cloud_notm}}. Inclua o conteúdo integral do token <code>IAM</code>, incluindo valor Bearer, na solicitação cURL. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperando um token de acesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo que é designado para sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperando um ID da instância</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>O identificador exclusivo que é usado para rastrear e correlacionar transações.</td>
      </tr>
      <tr>
        <td><varname>rotation_interval</varname></td>
        <td><strong>Necessário.</strong> Um valor de número inteiro que determina o tempo de intervalo de rotação de chave em meses. O mínimo é <code>1</code> e o máximo é <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para criar uma política de rotação com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Uma resposta `PUT api/v2/keys/{id}/policies` bem-sucedida retorna detalhes da política que estão associados à sua chave. O objeto JSON a seguir mostra uma resposta de exemplo para uma chave raiz que tem uma política de rotação existente.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### Atualizando uma política de rotação
{: #update-rotation-policy-api}

Atualize uma política existente para uma chave raiz, fazendo uma chamada `PUT` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Substitua a política de rotação por uma chave especificada, executando o comando cURL a seguir.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
     "resources": [
       {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <new_rotation_interval>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    Substitua as variáveis na solicitação de exemplo de acordo com a tabela a seguir.
    <table>
      <tr>
        <th>Variável</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz para a qual você deseja substituir uma política de rotação.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Terminais regionais em serviço</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Necessário.</strong> Seu token de acesso do {{site.data.keyword.cloud_notm}}. Inclua o conteúdo integral do token <code>IAM</code>, incluindo valor Bearer, na solicitação cURL. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperando um token de acesso</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo que é designado para sua instância de serviço {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, veja <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperando um ID da instância</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>O identificador exclusivo que é usado para rastrear e correlacionar transações.</td>
      </tr>
      <tr>
        <td><varname>new_rotation_interval</varname></td>
        <td><strong>Necessário.</strong> Um valor de número inteiro que determina o tempo de intervalo de rotação de chave em meses. O mínimo é <code>1</code> e o máximo é <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para criar uma política de rotação com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Uma resposta `PUT api/v2/keys/{id}/policies` bem-sucedida retorna detalhes da política atualizados que estão associados à sua chave. O objeto JSON a seguir mostra uma resposta de exemplo para uma chave raiz com uma política de rotação atualizada.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    Os valores `interval_month` e `updatedat` são atualizados nos detalhes da política para a chave. Se um usuário diferente atualiza uma política para uma chave que você criou inicialmente, o valor `updatedby` também muda para mostrar o identificador para a pessoa que enviou a solicitação.

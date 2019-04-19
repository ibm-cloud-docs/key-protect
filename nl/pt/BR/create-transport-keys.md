---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# Criando uma chave de transporte
{: #create-transport-keys}

É possível ativar a importação segura do material da chave raiz para a nuvem primeiro criando uma chave de criptografia de transporte para sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

As chaves de transporte são usadas para criptografar e importar de forma segura o material da chave raiz no {{site.data.keyword.keymanagementserviceshort}} com base nas políticas que você especificar. Para saber mais sobre como importar suas chaves de forma segura para a nuvem, consulte [Trazendo suas chaves de criptografia para a nuvem](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

As chaves de transporte são atualmente um recurso beta. Os recursos Beta podem mudar a qualquer momento, e as atualizações futuras podem introduzir mudanças que sejam incompatíveis com a versão mais recente.
{: important}

## Criando uma chave de transporte com a API
{: #create-transport-key-api}

Crie uma chave de transporte que esteja associada à sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} fazendo uma chamada `POST` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Recupere suas credenciais de serviço e autenticação para trabalhar com chaves no serviço](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Configure uma política para sua chave de transporte chamando a API do [{{site.data.keyword.keymanagementserviceshort}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <expiration_time>,  \
     "maxAllowedRetrievals": <use_count>  \
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
          <td><varname>region</varname></td>
          <td><strong>Necessário.</strong> A abreviação da região, como <code>us-south</code> ou <code>eu-gb</code>, que representa a área geográfica na qual reside sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}. Para obter mais informações, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Terminais regionais em serviço</a>.</td>
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
          <td><varname>expiration_time</varname></td>
          <td>
            <p>O tempo em segundos a partir da criação de uma chave de transporte que determina por quanto tempo a chave permanece válida.</p>
            <p>O valor mínimo é 300 segundos (5 minutos), e o máximo é 86.400 (24 horas). O valor padrão é 600 (10 minutos).</p>
          </td>
        </tr>
        <tr>
          <td><varname>use_count</varname></td>
          <td>O número de vezes que uma chave de transporte pode ser recuperada dentro de seu prazo de expiração antes de que ela não esteja mais acessível. O valor padrão é 1.</td>
        </tr>
          <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para incluir uma chave raiz com a API do {{site.data.keyword.keymanagementserviceshort}}</caption>
      </table>

    Uma solicitação `POST api/v2/lockers` bem-sucedida cria uma chave de transporte para sua instância de serviço e retorna seu valor de ID, juntamente com outros metadados. O ID é um identificador exclusivo que está associado à sua chave de transporte e é usado para chamadas subsequentes para a API do {{site.data.keyword.keymanagementserviceshort}}.

3. Opcional: verifique se a chave de transporte foi criada executando a chamada a seguir para recuperar metadados sobre a instância de serviço.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

## O que vem a seguir
{: #create-transport-key-next-steps}

- Para descobrir mais sobre o uso de uma chave de transporte para importar chaves raiz para o serviço, confira [Importando chaves raiz](/docs/services/key-protect?topic=key-protect-import-root-keys).
- Para descobrir mais sobre como gerenciar programaticamente as suas chaves, [consulte o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

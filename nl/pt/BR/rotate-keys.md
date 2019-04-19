---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: rotate encryption key, encryption key rotation, rotate key API examples 

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

# Girando chaves on demand
{: #rotate-keys}

É possível girar suas chaves raiz on demand usando o {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Ao girar sua chave raiz, você encurta o tempo de vida da chave e limita a quantia de informações que são protegidas por
essa chave.   

Para saber como a rotação de chave ajuda a atender aos padrões de mercado e às melhores práticas criptográficas, consulte [Girando suas chaves de criptografia](/docs/services/key-protect?topic=key-protect-key-rotation).

A rotação está disponível apenas para chaves raízes. Para saber mais sobre suas opções de rotação de chave no {{site.data.keyword.keymanagementserviceshort}}, confira [Comparando suas opções de rotação de chave](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).
{: note}

## Girando chaves raiz com a GUI
{: #rotate-key-gui}

Se você preferir girar suas chaves raiz usando uma interface gráfica, será possível usar a GUI do {{site.data.keyword.keymanagementserviceshort}}.

[Depois de criar ou importar suas chaves raiz existentes
para o serviço](/docs/services/key-protect?topic=key-protect-create-root-keys), conclua as etapas a seguir para girar uma chave:

1. [Efetue login no console do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/){: new_window}.
2. Acesse **Menu** &gt; **Lista de recursos** para visualizar uma lista de seus recursos.
3. Em sua lista de recursos do {{site.data.keyword.cloud_notm}}, selecione a sua instância provisionada do {{site.data.keyword.keymanagementserviceshort}}.
4. Na página de detalhes do aplicativo, use a tabela de **Chaves** para procurar as chaves em seu serviço.
5. Clique no ícone de ponto de acesso para abrir uma lista de opções para a chave que você deseja girar.
6. No menu de opções, clique em **Girar chave** e confirme a rotação na próxima tela.

Se você importou a chave raiz inicialmente, deverá fornecer o novo material de chave codificado em Base64 para girar
a chave. Para obter mais informações, consulte [Importando chaves raiz com a GUI](/docs/services/key-protect?topic=key-protect-import-root-keys#gui).
{: note}

## Girando chaves raiz usando a API
{: #rotate-key-api}

[Depois de designar uma chave raiz no serviço](/docs/services/key-protect?topic=key-protect-create-root-keys), é
possível girar sua chave fazendo uma chamada de `POST` para o terminal a seguir.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate
```
{: codeblock}

1. [Recuperar suas credenciais de autenticação e serviço para que funcionem com chaves no serviço.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copie o ID da chave raiz que deseja girar.

3. Execute o comando cURL a seguir para substituir a chave pelo novo material de chave.

    ```cURL
    curl -X POST \ 'https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <key_material>'
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
        <td><varname>key_ID</varname></td>
        <td><strong>Necessário.</strong> O identificador exclusivo para a chave raiz que você deseja girar.</td>
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
        <td><varname>key_material</varname></td>
        <td>
          <p>O material de chave codificado base64 que você deseja armazenar e gerenciar no serviço. Esse valor será necessário se você importar inicialmente o material de chave quando incluir a chave no serviço.</p>
          <p>Para girar uma chave que foi gerada inicialmente pelo {{site.data.keyword.keymanagementserviceshort}}, omita o atributo <code>payload</code> e transmita um corpo da entidade de solicitação vazio. Para girar uma chave importada, forneça um material de chave que atenda aos requisitos a seguir:</p>
          <p>
            <ul>
              <li>A chave deve ser 128, 192 ou 256 bits.</li>
              <li>Os bytes de dados, por exemplo, 32 bytes para 256 bits, devem ser codificados usando codificação base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descreve as variáveis que são necessárias para girar uma chave especificada no {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Uma solicitação de rotação bem-sucedida retorna uma resposta HTTP `204 No Content`, que indica que sua chave raiz foi substituída por um novo material de chave.

4. Opcional: verifique se a chave foi girada ao executar a chamada a seguir para procurar as chaves na instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
    https://<region>.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}
  
    Revise o valor `lastRotateDate` no corpo da entidade de resposta para inspecionar a data e hora em que sua chave foi girada pela última vez.
    

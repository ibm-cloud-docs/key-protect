---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Introdução ao {{site.data.keyword.keymanagementserviceshort}}

O {{site.data.keyword.keymanagementservicefull}} ajuda a provisionar chaves criptografadas para apps em serviços {{site.data.keyword.cloud_notm}}. Este tutorial mostra como criar e incluir chaves criptográficas existentes usando o painel do
{{site.data.keyword.keymanagementserviceshort}}, assim, é possível gerenciar a criptografia de dados de um local
central.
{: shortdesc}

## Introdução às chaves de criptografia
{: #get_started_keys}

No painel do {{site.data.keyword.keymanagementserviceshort}}, é possível criar novas chaves para criptografia
ou importar as chaves existentes. 

Escolha entre dois tipos de chave:

<dl>
  <dt>Chaves raiz</dt>
    <dd>As chaves raiz são chaves simétricas de agrupamento de chaves completamente gerenciadas em {{site.data.keyword.keymanagementserviceshort}}. É possível usar uma chave raiz para proteger outras chaves criptográficas com criptografia avançada.</dd>
  <dt>Chaves padrão</dt>
    <dd>Chaves padrão são chaves simétricas que são usadas para criptografia. É possível usar uma chave padrão para criptografar e decriptografar dados diretamente.</dd>
</dl>

## Criando novas chaves
{: #creating_keys}

[Depois de criar uma
instância do {{site.data.keyword.keymanagementserviceshort}}](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps), você está pronto para designar chaves no serviço. 

Conclua as etapas a seguir para criar sua primeira chave criptográfica. 

1. No painel {{site.data.keyword.keymanagementserviceshort}}, clique em **Gerenciar** &gt; **Incluir chave**.
2. Para criar uma nova chave, selecione a janela **Gerar uma nova chave**.

    Especifique os detalhes da chave:

    <table>
      <tr>
        <th>Configuração</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Um alias exclusivo, legível para fácil identificação de sua chave.</p>
          <p>Para proteger sua privacidade, assegure-se de que o nome da chave não contenha informações pessoalmente identificáveis (PII), como seu nome ou local.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo de chave</td>
        <td>O [tipo de chave](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que você gostaria de gerenciar no {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 1. Descrição das configurações de Gerar nova chave</caption>
    </table>

3. Quando você tiver concluído o preenchimento dos detalhes da chave, clique em **Gerar chave** para confirmar. 

Chaves que são criadas no serviço são chaves simétricas de 256 bits, suportadas pelo algoritmo AES-GCM. Para segurança adicional, as chaves são geradas por módulos de segurança de hardware (HSMs) certificados FIPS 140-2 Nível 2 que estão localizados em data centers seguros do {{site.data.keyword.cloud_notm}}. 

## Incluindo chaves existentes
{: #adding_keys}

É possível ativar os benefícios de segurança de Bring Your Own Key (BYOK) apresentando as suas chaves existentes para o serviço. 

Conclua as etapas a seguir para incluir uma chave existente.

1. No painel {{site.data.keyword.keymanagementserviceshort}}, clique em **Gerenciar** &gt; **Incluir chave**.
2. Para fazer upload de uma chave existente, selecione a janela **Inserir chave existente**.

    Especifique os detalhes da chave:

    <table>
      <tr>
        <th>Configuração</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td>Nome</td>
        <td>
          <p>Um alias exclusivo, legível para fácil identificação de sua chave.</p>
          <p>Para proteger sua privacidade, assegure-se de que o nome da chave não contenha informações pessoalmente identificáveis (PII), como seu nome ou local.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo de chave</td>
        <td>O [tipo de chave](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que você gostaria de gerenciar no {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <tr>
        <td>Material de chave</td>
        <td>Somente necessário se você estiver incluindo uma chave existente. O material da chave pode ser qualquer tipo de dados, como uma chave simétrica, que você deseja armazenar no serviço do {{site.data.keyword.keymanagementserviceshort}}. A chave fornecida deve ser codificada em Base64.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabela 2. Descrição das configurações de Inserir chave existente</caption>
    </table>

3. Quando você tiver concluído o preenchimento dos detalhes da chave, clique em **Incluir nova chave** para confirmar. 

No painel {{site.data.keyword.keymanagementserviceshort}}, é possível inspecionar as características gerais de suas novas chaves. 

## O que vem a seguir

Agora, é possível usar as chaves para codificar seus apps e serviços.

- Para saber mais sobre como gerenciar as suas chaves programaticamente, [verifique o documento de referência da API do {{site.data.keyword.keymanagementserviceshort}} para amostras de código ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/apidocs/639){: new_window}.
- Para ver um exemplo de como chaves armazenadas no {{site.data.keyword.keymanagementserviceshort}} podem funcionar para criptografar e decriptografar dados, [consulte o aplicativo de amostra no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
- Para saber mais sobre como integrar o serviço do {{site.data.keyword.keymanagementserviceshort}} com outras soluções de dados da nuvem, o [ consulte o doc Integrações](/docs/services/keymgmt/keyprotect_integration.html).

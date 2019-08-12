---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# Trazendo suas próprias chaves de criptografia para a nuvem
{: #importing-keys}

As chaves de criptografia contêm subconjuntos de informações, como os metadados que ajudam a identificar a chave e o _material da chave_ que é usado para criptografar e decriptografar dados.
{: shortdesc}

Quando você usa o {{site.data.keyword.keymanagementserviceshort}} para criar chaves, o serviço gera o material da chave criptográfica em seu nome que está enraizado em hardware security modules (HSMs) baseados em nuvem. Mas, dependendo de seus requisitos de negócios, pode ser necessário gerar o material da chave de sua solução interna e, em seguida, estender a infraestrutura de gerenciamento de chaves no local para a nuvem importando chaves para o {{site.data.keyword.keymanagementserviceshort}}.

<table>
  <th>Benefício</th>
  <th>Descrição</th>
  <tr>
    <td>Bring your own keys (BYOK) </td>
    <td>Você deseja controlar totalmente e fortalecer suas práticas de gerenciamento de chaves gerando chaves fortes por meio do seu hardware security module (HSM) no local. Se você optar por exportar chaves simétricas de sua infraestrutura de gerenciamento de chaves interna, será possível usar o {{site.data.keyword.keymanagementserviceshort}} para trazê-las com segurança para a nuvem.</td>
  </tr>
  <tr>
    <td>Importação segura de material de chave raiz</td>
    <td>Ao exportar suas chaves para a nuvem, você deseja assegurar que o material da chave seja protegido enquanto ele estiver em andamento. Mitigue ataques man-in-the-middle <a href="#transport-keys">usando uma chave de transporte</a> para importar de forma segura o material da chave raiz em sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabela 1. Descreve os benefícios de importar o material da chave</caption>
</table>


## Planejando antecipadamente a importação do material de chave
{: #plan-ahead}

Mantenha em mente as considerações a seguir quando estiver pronto para importar o material de chave raiz para o serviço.

<dl>
  <dt>Revise suas opções para criar o material da chave</dt>
    <dd>Explore suas opções para criar chaves de criptografia simétricas de 256 bits com base em suas necessidades de segurança. Por exemplo, é possível usar seu sistema de gerenciamento de chaves interno, suportado por um hardware security module (HSM) no local, validado por FIPS, para gerar material de chave antes de trazer chaves para a nuvem. Se você estiver construindo uma prova de conceito, também será possível usar um kit de ferramentas de criptografia, como <a href="https://www.openssl.org/" target="_blank">OpenSSL</a> para gerar o material de chave que você pode importar para o {{site.data.keyword.keymanagementserviceshort}} para suas necessidades de teste.</dd>
  <dt>Escolha uma opção para importar o material da chave para o {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>Escolha entre duas opções para importar chaves raiz com base no nível de segurança que é necessário para seu ambiente ou carga de trabalho. Por padrão, o {{site.data.keyword.keymanagementserviceshort}} criptografa seu material de chave enquanto ele está em trânsito usando o protocolo Segurança da Camada de Transporte (TLS) 1.2. Se você estiver construindo uma prova de conceito ou experimentando o serviço pela primeira vez, será possível importar o material da chave raiz para o {{site.data.keyword.keymanagementserviceshort}} usando essa opção padrão. Se a sua carga de trabalho requerer um mecanismo de segurança além do TLS, também será possível <a href="#transport-keys">usar uma chave de transporte</a> para criptografar e importar o material da chave raiz para o serviço.</dd>
  <dt>Planeje antecipadamente a criptografia de seu material da chave</dt>
    <dd>Se você optar por criptografar seu material da chave usando uma chave de transporte, determine um método para executar a criptografia RSA no material da chave. Deve-se usar o esquema de criptografia <code>RSAES_OAEP_SHA_256</code> conforme especificado pelo <a href="https://tools.ietf.org/html/rfc3447" target="_blank">Padrão PKCS #1 v2.1 para criptografia RSA</a>. Revise os recursos de seu sistema de gerenciamento de chaves interno ou do HSM no local para determinar suas opções.</dd>
  <dt>Gerencie o ciclo de vida do material de chave importado</dt>
    <dd>Após importar o material de chave no serviço, lembre-se de que você é responsável por gerenciar o ciclo de vida completo de sua chave. Usando a API do {{site.data.keyword.keymanagementserviceshort}}, é possível configurar uma data de expiração para a chave quando você decide fazer upload dela para o serviço. No entanto, se você deseja <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">girar uma chave raiz importada </a>, deve-se gerar e fornecer novo material de chave para obsoletar e substituir a chave existente. </dd>
</dl>

## Usando chaves de transporte
{: #transport-keys}

As chaves de transporte são atualmente um recurso beta. Os recursos Beta podem mudar a qualquer momento, e as atualizações futuras podem introduzir mudanças que sejam incompatíveis com a versão mais recente.
{: important}

Se você desejar criptografar seu material de chave antes de importá-lo para o {{site.data.keyword.keymanagementserviceshort}}, será possível criar uma chave de criptografia de transporte para sua instância de serviço usando a API do {{site.data.keyword.keymanagementserviceshort}}. 

As chaves de transporte são um tipo de recurso no {{site.data.keyword.keymanagementserviceshort}} que ativam a importação segura do material da chave raiz para a instância de serviço. Ao usar uma chave de transporte para criptografar seu material de chave no local, você protege as chaves raiz enquanto elas estão em andamento para o {{site.data.keyword.keymanagementserviceshort}} com base nas políticas que você especificar. Por exemplo, é possível configurar uma política na chave de transporte que limita o uso da chave com base no tempo e na contagem de uso.

### Como ele funciona
{: #how-transport-keys-work}

Quando você [cria uma chave de transporte](/docs/services/key-protect?topic=key-protect-create-transport-keys) para sua instância de serviço, o {{site.data.keyword.keymanagementserviceshort}} gera uma chave RSA de 4.096 bits. O serviço criptografa a chave privada e, em seguida, retorna a chave pública e um token de importação que é possível usar para criptografar e importar seu material da chave raiz. 

Quando estiver pronto para [importar uma chave raiz](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-api) no {{site.data.keyword.keymanagementserviceshort}}, forneça o material da chave raiz criptografada e o token de importação que é usado para verificar a integridade da chave pública. Para concluir a solicitação, o {{site.data.keyword.keymanagementserviceshort}} usa a chave privada que está associada à sua instância de serviço para decriptografar o material da chave raiz criptografada. Esse processo assegura que apenas a chave de transporte que você gerou no {{site.data.keyword.keymanagementserviceshort}} possa decriptografar o material da chave que você importa e armazena no serviço.

É possível criar apenas uma chave de transporte por instância de serviço. Para saber mais sobre limites de recuperação para chaves de transporte, [consulte o {{site.data.keyword.keymanagementserviceshort}}doc de referência da API ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### Métodos de API
{: #secure-import-api-methods}

Nos bastidores, a API do {{site.data.keyword.keymanagementserviceshort}} impulsiona o processo de criação da chave de transporte.  

A tabela a seguir lista os métodos de API que configuraram um bloqueador e criam chaves de transporte para sua instância de serviço.

<table>
  <tr>
    <th>Método</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Criar uma chave de transporte</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Recuperar metadados da chave de transporte</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Recuperar uma chave de transporte e um token de importação</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabela 2. Descreve os métodos de API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Para descobrir mais sobre como gerenciar programaticamente suas chaves no {{site.data.keyword.keymanagementserviceshort}}, consulte o doc de referência da API do [{{site.data.keyword.keymanagementserviceshort}}![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

## O que vem a seguir

- Para saber como criar uma chave de transporte para sua instância de serviço, consulte [Criando uma chave de transporte](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- Para descobrir mais sobre como importar chaves para o serviço, consulte [Importando chaves raiz](/docs/services/key-protect?topic=key-protect-import-root-keys). 

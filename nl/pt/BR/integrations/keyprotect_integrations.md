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

# Integrando serviços
{: #integrations}

O {{site.data.keyword.keymanagementservicefull}} integra-se com dados e soluções de armazenamento para ajudá-lo a trazer e gerenciar a sua própria criptografia na nuvem.
{: shortdesc}

[Depois de criar uma instância do serviço](/docs/services/keymgmt/keyprotect_provision.html), é possível integrar o {{site.data.keyword.keymanagementserviceshort}} com os serviços suportados a seguir:

<table>
    <tr>
        <th>Serviço</th>
        <th>Descrição</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>Inclua a [criptografia de envelope](/docs/services/keymgmt/concepts/keyprotect_envelope.html) em seus buckets de armazenamento usando o {{site.data.keyword.keymanagementserviceshort}}. Use chaves raiz que você gerencia no {{site.data.keyword.keymanagementserviceshort}} para proteger as chaves de criptografia de dados que criptografam os seus dados em repouso.</p>
          <p>Para obter mais informações, verifique [Integrando com o {{site.data.keyword.cos_full_notm}}](/docs/services/keymgmt/integrations/keyprotect_cloud-object-storage.html).</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">Tabela 1. Descreve as integrações que estão disponíveis para o {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

## Entendendo sua integração 
{: #understand_integration}

Ao integrar um serviço suportado com o {{site.data.keyword.keymanagementserviceshort}}, você ativa a [criptografia de envelope](/docs/services/keymgmt/concepts/keyprotect_envelope.html) para esse serviço. Essa integração permite o uso de uma chave raiz que você armazena no {{site.data.keyword.keymanagementserviceshort}} para agrupar as chaves de criptografia de dados que criptografam os seus dados em repouso. 

Por exemplo, é possível criar uma chave raiz, gerenciá-la no {{site.data.keyword.keymanagementserviceshort}} e usá-la para proteger os dados que são armazenados em serviços de nuvem diferentes.

![O diagrama mostra uma visualização contextual de sua integração do {{site.data.keyword.keymanagementserviceshort}}.](../images/kp-integrations_min.svg)

### {{site.data.keyword.keymanagementserviceshort}} métodos de API
{: #api_methods}

Nos bastidores, a API do {{site.data.keyword.keymanagementserviceshort}} conduz o processo de criptografia de envelope.  

A tabela a seguir lista os métodos de API que incluem ou removem a criptografia de envelope em um recurso.

<table>
  <tr>
    <th>Método</th>
    <th>Descrição</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_wrap_keys.html">Agrupar (criptografar) uma chave de criptografia de dados</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_unwrap_keys.html">Desagrupar (decriptografar) uma chave de criptografia de dados</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabela 2. Descreve os métodos de API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Para saber mais sobre como gerenciar programaticamente as suas chaves no {{site.data.keyword.keymanagementserviceshort}}, verifique o documento de referência de API do [{{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/apidocs/639){: new_window} para obter amostras de código.
{: tip}

## Integrando um serviço suportado
{: #grant_access}

Para incluir uma integração, crie uma autorização entre serviços usando o painel do {{site.data.keyword.iamlong}}. As autorizações permitem políticas de acesso de serviço para serviço, para que você possa associar um recurso em seu serviço de dados em nuvem com uma [chave raiz](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que você gerencia no {{site.data.keyword.keymanagementserviceshort}}.

Certifique-se de fornecer ambos os serviços na mesma região antes de criar uma autorização. Para saber mais sobre autorizações de serviço, veja [Concedendo acesso entre os serviços ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](/docs/iam/authorizations.html){: new_window}.
{: tip}

Quando você estiver pronto para integrar um serviço, use as etapas a seguir para criar uma autorização:

1. [Efetue login no console do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/){: new_window}.
2. Na barra de menus, clique em **Gerenciar** &gt; **Segurança** &gt; **Identidade e acesso**e, em seguida, selecione **Autorizações**. 
3. Clique em **Criar**.
4. Selecione uma origem e destino para a autorização.
 
  - Para **Serviço de origem**, selecione o serviço de dados em nuvem que você deseja integrar com o {{site.data.keyword.keymanagementserviceshort}}. Por exemplo, **Cloud Object Storage**.
  - Para **Serviço de destino**, selecione **{{site.data.keyword.keymanagementservicelong_notm}}**. 
4. Para conceder acesso somente leitura entre os serviços, selecione a caixa de seleção **Leitor**.

    Com as permissões de _Leitor_, o seu serviço de origem pode procurar as chaves raiz que são fornecidas na instância especificada do {{site.data.keyword.keymanagementserviceshort}}.
5. Clique em **Autorizar**.

### O que vem a seguir

Inclua a criptografia avançada em seus recursos em nuvem criando uma chave raiz no {{site.data.keyword.keymanagementserviceshort}}. Inclua um novo recurso em um serviço de dados de nuvem suportado e, em seguida, selecione a chave raiz que deseja usar para a criptografia avançada.

- Para saber mais sobre como criar chaves raiz com o serviço do {{site.data.keyword.keymanagementserviceshort}}, veja [Criando chaves raiz](/docs/services/keymgmt/keyprotect_create_root.html).
- Para saber mais sobre como trazer as suas próprias chaves raiz para o serviço do {{site.data.keyword.keymanagementserviceshort}}, veja [Importando chaves raiz](/docs/services/keymgmt/keyprotect_import_root.html).



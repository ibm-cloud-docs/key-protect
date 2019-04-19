---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: rotate encryption keys, rotate keys automatically, key rotation

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

# Girando chaves de criptografia
{: #key-rotation}

A rotação de teclas ocorre quando você obsoleta o material da chave original de uma chave raiz e a rechaveia gerando material novo de chave criptográfica.

Fazer a rotação das chaves regularmente ajuda a atender os padrões de mercado e as melhores práticas criptográficas. A tabela a seguir descreve os principais benefícios da rotação de chave:

<table>
  <th>Benefício</th>
  <th>Descrição</th>
  <tr>
    <td>Gerenciamento do período criptográfico para as chaves</td>
    <td>A rotação de chave limita por quanto tempo suas informações são protegidas por uma única chave. Ao fazer a rotação das chaves raiz em intervalos regulares, você também encurte o período criptográfico das chaves. Quanto mais longo o tempo
de vida de uma chave de criptografia, maior a probabilidade de uma violação de segurança.</td>
  </tr>
  <tr>
    <td>Mitigação de incidente</td>
    <td>Se a sua organização detectar um problema de segurança, será possível fazer a rotação da chave imediatamente para mitigar ou reduzir custos que estão associados com o comprometimento da chave.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabela 1. Descreve os benefícios da rotação de chave</caption>
</table>

A rotação de chave é tratada na Publicação Especial do NIST 800-57, Recomendação para o gerenciamento de chave. Para saber mais, consulte [NIST SP
800-57 Pt. 1 Rev. 4. ![Ícone de link externo](../../../icons/launch-glyph.svg "Ícone de link externo")](https://www.nist.gov/publications/recommendation-key-management-part-1-general-0){: new_window}
{: tip}

## Comparando suas opções de rotação de chave no {{site.data.keyword.keymanagementserviceshort}}
{: #compare-key-rotation-options}

No {{site.data.keyword.keymanagementserviceshort}}, é possível [configurar uma política de rotação para uma chave](/docs/services/key-protect?topic=key-protect-set-rotation-policy) ou [girar a chave on demand](/docs/services/key-protect?topic=key-protect-rotate-keys), sem a necessidade de manter o controle de seu material de chave raiz obsoleto. 

As opções de rotação estão disponíveis somente para chaves raiz.
{: note}

<dl>
  <dt>Configurando uma política de rotação para uma chave</dt>
    <dd>O {{site.data.keyword.keymanagementserviceshort}} ajuda a simplificar a rotação para chaves de criptografia ativando políticas de rotação para chaves que você gera no serviço. Depois de criar uma chave raiz, é possível gerenciar uma política de rotação para a chave na GUI do {{site.data.keyword.keymanagementserviceshort}} ou com a API. <a href="/docs/services/key-protect?topic=key-protect-key-rotation#rotation-frequency">Escolha um intervalo de rotação automática entre 1 e 12 meses para sua chave</a> com base em suas necessidades de segurança contínua. Quando for a hora de girar a chave com base no intervalo de rotação que você especificar, o {{site.data.keyword.keymanagementserviceshort}} substituirá automaticamente a chave por um novo material de chave.</dd>
  <dt>Girando chaves on demand</dt>
    <dd>Como um administrador de segurança, talvez você queira ter mais controle sobre a frequência de rotação de suas chaves. Se você não desejar configurar uma política de rotação automática para uma chave, será possível criar manualmente uma nova chave para substituir uma chave existente e, em seguida, atualizar seus aplicativos para que eles referenciem a nova chave. Para simplificar esse processo, é possível usar o {{site.data.keyword.keymanagementserviceshort}} para girar a chave on demand. Nesse caso, o {{site.data.keyword.keymanagementserviceshort}} cria e substitui a chave em seu nome por cada solicitação de rotação. A chave retém os mesmos metadados e o ID da chave.</dd>
</dl>

## Como a rotação de chave funciona 
{: #how-key-rotation-works}

A rotação de chave funciona por meio da transição de forma segura do material da chave de um estado de chave _Ativo_ para um _Desativado_. Para substituir o material de chave desativado ou obsoleto, o novo material da chave se move para o estado _Ativo_ e se torna disponível para operações criptográficas.

### Usando o {{site.data.keyword.keymanagementserviceshort}} para girar chaves
{: #use-key-protect-rotate-keys}

Lembre-se das considerações a seguir conforme você se prepara para usar o {{site.data.keyword.keymanagementserviceshort}} para girar chaves raiz.

<dl>
  <dt>Girando chaves raiz que são geradas no {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>É possível usar o {{site.data.keyword.keymanagementserviceshort}} para girar uma chave raiz que foi gerada no {{site.data.keyword.keymanagementserviceshort}} definindo uma política de rotação para a chave ou girando a chave on demand. Os metadados para a chave raiz, como seu ID da chave, não mudam ao girar a chave.</dd>
  <dt>Girando chaves raiz que você traz para o serviço</dt>
    <dd>Para girar uma chave raiz que você importou inicialmente para o serviço, deve-se gerar e fornecer novo material de chave para a chave. É possível usar o {{site.data.keyword.keymanagementserviceshort}} para girar as chaves raiz importadas on demand, fornecendo o novo material de chave como parte da solicitação de rotação. Os metadados para a chave raiz, como seu ID da chave, não mudam ao girar a chave. Como um novo material de chave deve ser fornecido para girar uma chave importada, as políticas de rotação automática não estão disponíveis para chaves raiz que possuam material de chave importado.</dd>
  <dt>Gerenciando material de chave obsoleto</dt>
    <dd>O {{site.data.keyword.keymanagementserviceshort}} cria um novo material de chave depois de girar uma chave raiz. O serviço obsoleta o material de chave antigo e retém as versões obsoletas até que a chave raiz seja excluída. Quando você usa a chave raiz para criptografia de envelope, o {{site.data.keyword.keymanagementserviceshort}} usa apenas o material de chave mais recente que está associado à chave. O material de chave obsoleto não pode mais ser usado para proteger chaves, mas permanece disponível para operações de desagrupamento. Se o {{site.data.keyword.keymanagementserviceshort}} detectar que você está usando o material de chave obsoleto para desagrupar DEKs, o serviço fornecerá um DEK recém-agrupado que se baseia no material da chave raiz mais recente. É possível usar a DEK recém-agrupada para reagrupar chaves com o material da chave mais recente.</dd>
 <dt>Ativando a rotação de chave para serviços de dados do {{site.data.keyword.cloud_notm}}</dt>
    <dd>Para ativar essas opções de rotação de chave para seu serviço de dados no {{site.data.keyword.cloud_notm}}, o serviço de dados deve ser integrado ao {{site.data.keyword.keymanagementserviceshort}}. Consulte a documentação para seu serviço de dados do {{site.data.keyword.cloud_notm}} ou <a href="/docs/services/key-protect?topic=key-protect-integrate-services">confira a nossa lista de serviços integrados para saber mais</a>.</dd>
</dl>

Ao girar uma chave no {{site.data.keyword.keymanagementserviceshort}}, taxas adicionais não são cobradas. É possível continuar desagrupando as chaves de criptografia de dados agrupadas (WDEKs) com o material de chave obsoleto sem custo extra. Para obter mais informações sobre as opções de precificação, consulte a página do catálogo do [{{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect).
{: tip}

### Entendendo o processo de rotação de chave
{: #understand-key-rotation-process}

Por trás dos bastidores, a API do {{site.data.keyword.keymanagementserviceshort}} guia o processo de rotação de chave.  

O diagrama a seguir mostra uma visualização contextual da funcionalidade da rotação de chave. ![O diagrama mostra uma visualização contextual da rotação de chave.](../images/key-rotation_min.svg)

Com cada solicitação de rotação, o {{site.data.keyword.keymanagementserviceshort}} associa o novo material de chave com sua chave raiz. 

![O diagrama mostra uma microvisualização da pilha de chaves raiz.](../images/root-key-stack_min.svg)

Depois que uma rotação é concluída, o novo material de chave raiz se torna disponível para proteger futuras chaves de criptografia de dados (DEKs) com a [criptografia de envelope](/docs/services/key-protect?topic=key-protect-envelope-encryption). O material de chave obsoleto é movido para o estado _Desativado_, no qual ele pode ser usado apenas para desagrupar e acessar as DEKs mais antigas que ainda não estão protegidas pelo material de chave raiz mais recente. Se o {{site.data.keyword.keymanagementserviceshort}} detectar que você está usando o material de chave raiz obsoleto para desagrupar uma DEK mais antiga, o serviço automaticamente criptografará novamente a DEK e retornará uma chave de criptografia de dados agrupada (WDEK) que se baseia no material de chave raiz mais recente. Armazene e use a nova WDEK para operações futuras de desagrupamento e, assim, proteja as suas DEKs com o material de chave raiz mais recente.

Para saber como usar a API do {{site.data.keyword.keymanagementserviceshort}} para girar as chaves raiz, consulte [Girando chaves](/docs/services/key-protect?topic=key-protect-rotate-keys).

## Frequência de rotação de chave
{: #rotation-frequency}

Depois de gerar uma chave raiz no {{site.data.keyword.keymanagementserviceshort}}, decida a frequência de sua rotação. Você pode desejar girar suas chaves devido à rotatividade de pessoal, mau funcionamento de processo ou de acordo com a política de expiração de chave interna de sua organização. 

Gire suas chaves regularmente, por exemplo, a cada 30 dias, para atender às melhores práticas criptográficas. 

| Tipo de rotação | Frequência | Descrição
| --- | --- | --- |
| [Rotação de chave baseada em política](/docs/services/key-protect?topic=key-protect-set-rotation-policy) | A cada 1 a 12 meses | Escolha um intervalo de rotação entre 1 a 12 meses para sua chave com base em suas necessidades de segurança contínua. Depois de configurar uma política de rotação para uma chave, o relógio será iniciado imediatamente com base na data de criação inicial para a chave. Por exemplo, se você configurar uma política de rotação mensal para uma chave que você criou em `2019/02/01`, o {{site.data.keyword.keymanagementserviceshort}} girará automaticamente a chave em `2019/03/01`.|
| [Rotação de chave on demand](/docs/services/key-protect?topic=key-protect-rotate-keys) | Até uma rotação por hora | Se você estiver girando uma chave on demand, o {{site.data.keyword.keymanagementserviceshort}} permitirá uma rotação por hora para cada chave raiz. |
{: caption="Tabela 2. Opções de frequência de rotação para girar chaves no {{site.data.keyword.keymanagementserviceshort}}" caption-side="top"}

## O que vem a seguir
{: #rotation-next-steps}

- Para aprender a usar o {{site.data.keyword.keymanagementserviceshort}} para configurar uma política de rotação automática para uma chave individual, consulte [Configurando uma política de rotação](/docs/services/key-protect?topic=key-protect-set-rotation-policy).
- Para saber mais sobre como girar manualmente as chaves raiz, consulte [Girando chaves on demand](/docs/services/key-protect?topic=key-protect-rotate-keys).

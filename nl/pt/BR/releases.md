---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# O que há de novo
{: #releases}

Fique atualizado com os novos recursos que estão disponíveis para o {{site.data.keyword.keymanagementservicefull}}. 

## Março de 2019
{: #mar-2019}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} inclui suporte para rotação de chave baseada em política
{: #added-support-policy-key-rotation}
Novo a partir de: 2019-03-22

Agora é possível usar o {{site.data.keyword.keymanagementserviceshort}} para associar uma política de rotação para suas chaves raiz.

Para obter mais informações, consulte [Configurando uma política de rotação](/docs/services/key-protect?topic=key-protect-set-rotation-policy). Para saber mais sobre suas opções de rotação de chave no {{site.data.keyword.keymanagementserviceshort}}, confira [Comparando suas opções de rotação de chave](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} inclui suporte beta para chaves de transporte
Novo a partir de: 20/03/2019

Ative a importação segura de chaves de criptografia para a nuvem criando chaves de criptografia de transporte para seu serviço do {{site.data.keyword.keymanagementserviceshort}}.

Para obter mais informações, consulte [Trazendo suas chaves de criptografia para a nuvem](/docs/services/key-protect?topic=key-protect-importing-keys).

As chaves de transporte são atualmente um recurso beta. Os recursos Beta podem mudar a qualquer momento, e as atualizações futuras podem introduzir mudanças que sejam incompatíveis com a versão mais recente.
{: important}

## Fevereiro de 2019
{: #feb-2019}

### Mudado: instâncias de serviço do {{site.data.keyword.keymanagementserviceshort}} anterior
{: #changed-legacy-service-instances}

Novo a partir de: 13/02/2019

As instâncias de serviço do {{site.data.keyword.keymanagementserviceshort}} que foram provisionadas antes de 15 de dezembro de 2017 estão em execução em uma infraestrutura anterior que é baseada no Cloud Foundry. Esse serviço anterior do {{site.data.keyword.keymanagementserviceshort}} será desatribuído em 15 de maio de 2019.

**O que isso significa para você**

Se você tiver chaves de produção ativas em uma instância de serviço mais antiga do {{site.data.keyword.keymanagementserviceshort}}, assegure-se de migrar as chaves para uma nova instância de serviço até 15 de maio de 2019 para evitar perder acesso aos seus dados criptografados. É possível verificar se você está usando uma instância anterior, navegando para a lista de recursos por meio do console [{{site.data.keyword.cloud_notm}}![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/). Se a sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}} estiver listada na seção **Serviços do Cloud Foundry** da lista de recursos do {{site.data.keyword.cloud_notm}} ou se você estiver usando o terminal de API `https://ibm-key-protect.edge.bluemix.net` para destinar as operações para o serviço, você estará usando uma instância anterior do {{site.data.keyword.keymanagementserviceshort}}. Depois de 15 de maio de 2019, o terminal anterior não estará mais acessível e você não será capaz de destinar o serviço para as operações.

Precisa de ajuda para migrar suas chaves de criptografia para uma nova instância de serviço? Para obter etapas detalhadas, verifique o [cliente de migração no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Cloud/kms-migration-client){: new_window}. Se você tiver perguntas adicionais sobre o status de suas chaves ou o processo de migração, fale com Terry Mosbaugh em [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Dezembro de 2018
{: #dec-2018}

### Mudado: terminais de API do {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-endpoints}

Novo a partir de: 2018-12-19

Para se alinhar com a nova experiência unificada do {{site.data.keyword.cloud_notm}}, o {{site.data.keyword.keymanagementserviceshort}} atualizou as URLs de base para suas APIs de serviço.

Agora é possível atualizar seus aplicativos para referenciar os novos terminais `cloud.ibm.com`.

- `keyprotect.us-south.bluemix.net` agora é `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` agora é `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` agora é `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` agora é `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` agora é `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` agora é `jp-tok.kms.cloud.ibm.com` 

Ambas as URLs para cada terminal de serviço regional são suportadas neste momento. 

## Outubro de 2018
{: #oct-2018}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} se expande para a região de Tóquio
{: #added-tokyo-region}

Novo a partir de: 31/10/2018

Agora é possível criar recursos do {{site.data.keyword.keymanagementserviceshort}} na região de Tóquio. 

Para obter mais informações, veja [Regiões e locais](/docs/services/key-protect?topic=key-protect-regions).

### Incluído: plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}}
{: #added-cli-plugin}

Novo a partir de: 02/10/2018

Agora é possível usar o plug-in da CLI do {{site.data.keyword.keymanagementserviceshort}} para gerenciar
chaves na instância de serviço do {{site.data.keyword.keymanagementserviceshort}}.

Para saber como instalar o plug-in, consulte [Configurando a CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). Para descobrir mais sobre a CLI do {{site.data.keyword.keymanagementserviceshort}},
[consulte o doc de referência da CLI](/docs/services/key-protect?topic=key-protect-cli-reference).

## Setembro de 2018
{: #sept-2018}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} inclui suporte para rotação de chave on demand
{: #added-key-rotation}

Novo a partir de: 28/09/2018

Agora é possível usar o {{site.data.keyword.keymanagementserviceshort}} para girar suas chaves raiz on demand.

Para obter mais informações, consulte [Girando chaves](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Incluído: tutorial de segurança de ponta a ponta para seu aplicativo em nuvem
{: #added-security-tutorial}

Novo a partir de: 14/09/2018

Procurando amostras de código para ajudar a criptografar o conteúdo do depósito de armazenamento com suas próprias
chaves de criptografia?

Agora é possível praticar a inclusão da segurança de ponta a ponta para seu aplicativo em nuvem seguindo [o novo tutorial](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

Para obter mais informações, [consulte o aplicativo de amostra no GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}.

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} se expande para a região de Washington DC
{: #added-wdc-region}

Novo a partir de: 10/09/2018

Agora é possível criar recursos do {{site.data.keyword.keymanagementserviceshort}} na região de Washington DC. 

Para obter mais informações, veja [Regiões e locais](/docs/services/key-protect?topic=key-protect-regions).

## Agosto de 2018
{: #aug-2018}

### Mudado: URL da documentação da API do {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-doc-url}

Novo a partir de: 28/08/2018

A referência da API do {{site.data.keyword.keymanagementserviceshort}} foi movida. 

Agora é possível acessar a documentação da API em
https://{DomainName}/apidocs/key-protect. 

## Março de 2018
{: #mar-2018}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} foi expandido para a região de Frankfurt
{: #added-frankfurt-region}

Novo a partir de: 2018-03-21

Agora é possível criar recursos do {{site.data.keyword.keymanagementserviceshort}} na região de Frankfurt. 

Para obter mais informações, veja [Regiões e locais](/docs/services/key-protect?topic=key-protect-regions).

## Janeiro de 2018
{: #jan-2018}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} se expande para a região de Sydney
{: #added-sydney-region}

Novo a partir de: 2018-01-31

Agora é possível criar recursos do {{site.data.keyword.keymanagementserviceshort}} na região de Sydney. 

Para obter mais informações, veja [Regiões e locais](/docs/services/key-protect?topic=key-protect-regions).

## Dezembro de 2017
{: #dec-2017}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} inclui suporte para Bring Your Own Key (BYOK)
{: #added-byok-support}

Novo a partir de: 2017-12-15

O {{site.data.keyword.keymanagementserviceshort}} agora suporta o Bring Your Own Key (BYOK) e a criptografia gerenciada pelo cliente.

- Foram introduzidas [chaves raiz](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), também denominadas Customer Root Keys (CRKs), como recursos primários no serviço. 
- Ativado  [ criptografia de envelope ](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how)  para  {{site.data.keyword.cos_full_notm}}  buckets.

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} foi expandido para a região de Londres
{: #added-london-region}

Novo a partir de: 2017-12-15

O {{site.data.keyword.keymanagementserviceshort}} agora está disponível na região de Londres. 

Para obter mais informações, veja [Regiões e locais](/docs/services/key-protect?topic=key-protect-regions).

### Alterado:  {{site.data.keyword.iamshort}}  roles
{: #changed-iam-roles}

Novo a partir de: 2017-12-15

As funções do {{site.data.keyword.iamshort}}, que determinam as ações que podem ser executadas nos recursos do {{site.data.keyword.keymanagementserviceshort}}, foram mudadas.

- ` Administrador `  agora é  ` Manager `
- ` Editor `  agora é  ` Gravador `
- ` Visualizador `  agora é  ` Leitor `

Para obter mais informações, veja [Gerenciando o acesso de usuário](/docs/services/key-protect?topic=key-protect-manage-access).

## Setembro 2017
{: #sept-2017}

### Incluído: o {{site.data.keyword.keymanagementserviceshort}} inclui suporte para o Cloud IAM
{: #added-iam-support}

Novo a partir de: 2017-09-19

Agora, é possível usar o {{site.data.keyword.iamshort}} para configurar e gerenciar políticas de acesso para seus recursos do {{site.data.keyword.keymanagementserviceshort}}.

Para obter mais informações, veja [Gerenciando o acesso de usuário](/docs/services/key-protect?topic=key-protect-manage-access).

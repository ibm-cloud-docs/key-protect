---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: user permissions, manage access, IAM roles

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

# Gerenciando acesso de usuário
{: #manage-access}

O {{site.data.keyword.keymanagementservicefull}} suporta um sistema de controle de acesso centralizado, governado pelo {{site.data.keyword.iamlong}}, para ajudar a gerenciar usuários e o acesso às suas chaves de criptografia.
{: shortdesc}

Uma boa prática é conceder permissões de acesso à medida que você convida novos usuários à sua conta ou serviço. Por exemplo, considere as diretrizes a seguir:

- **Ative o acesso de usuário aos recursos em sua conta designando funções do Cloud IAM.**
    Em vez de compartilhar suas credenciais de administrador, crie novas políticas para os usuários que precisam de acesso às chaves de criptografia em sua conta. Se você for o administrador para sua conta, você será automaticamente designado a uma política de _Gerenciador_
com acesso a todos os recursos sob a conta.
- **Conceda funções e permissões no menor escopo necessário.**
    Por exemplo, se um usuário precisar acessar apenas uma visualização de alto nível de chaves dentro de um espaço especificado, conceda a função _Leitor_ ao usuário para esse espaço.
- **Audite regularmente quem pode gerenciar o controle de acesso e exclua recursos de chave.**
    Lembre-se de que ao conceder uma função de _Gerenciador_ a um usuário ele poderá modificar políticas de serviço para outros usuários, além de destruir recursos.

## Funções e permissões
{: #roles}

Com o {{site.data.keyword.iamshort}} (IAM), é possível gerenciar e definir o acesso para usuários e recursos em sua conta.

Para simplificar o acesso, o {{site.data.keyword.keymanagementserviceshort}} se alinha com as funções do Cloud IAM para
que cada usuário tenha uma visualização diferente do serviço, de acordo com a função designada ao usuário. Se você for um
administrador de segurança do seu serviço, será possível designar as funções do Cloud IAM que correspondem às permissões específicas do
{{site.data.keyword.keymanagementserviceshort}} que desejar conceder a membros da sua equipe.

A tabela a seguir mostra como as funções de identidade e acesso são mapeadas para as permissões do {{site.data.keyword.keymanagementserviceshort}}:

<table>
  <col width="20%">
  <col width="40%">
  <col width="40%">
  <tr>
    <th>Função de acesso de serviço</th>
    <th>Descrição</th>
    <th>Ações</th>
  </tr>
  <tr>
    <td><p>Leitor</p></td>
    <td><p>Um leitor pode procurar uma visualização de alto nível de chaves e executar ações de agrupamento e de desagrupamento. Leitores não podem acessar ou modificar o material de chave.</p></td>
    <td>
      <p>
        <ul>
          <li>Visualizar chaves</li>
          <li>Quebrar chaves</li>
          <li>Desagrupar chaves</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Gravador</p></td>
    <td><p>Um gravador pode criar, modificar e girar chaves e acessar o material de chave.</p></td>
    <td>
      <p>
        <ul>
          <li>Criar chaves</li>
          <li>Visualizar chaves</li>
          <li>Girar chaves</li>
          <li>Quebrar chaves</li>
          <li>Desagrupar chaves</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Gerente</p></td>
    <td><p>Um gerenciador pode executar todas as ações que um leitor e um gravador podem executar, incluindo a capacidade de configurar políticas de rotação para chaves, excluir chaves, convidar novos usuários e designar políticas de acesso a outros usuários.</p></td>
    <td>
      <p>
        <ul>
          <li>Todas as ações que um leitor ou um gravador pode executar</li>
          <li>Designar políticas de acesso de usuário</li>
          <li>Configurar políticas de rotação de chave</li>
          <li>Excluir chaves</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Tabela 1. Descreve como as funções de identidade e acesso são mapeadas para permissões do {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

As funções de usuário do Cloud IAM fornecem acesso no nível de serviço ou instância de serviço. As [funções do Cloud Foundry](/docs/iam?topic=iam-cfaccess){: external} são separadas e definem o acesso no nível da organização ou do espaço. Para saber mais sobre o {{site.data.keyword.iamshort}}, confira [Funções e permissões do usuário](/docs/iam?topic=iam-userroles){: external}.
{: note}

## O que vem a seguir
{: #manage-access-next-steps}

Os proprietários e administradores de conta podem convidar usuários e configurar políticas de serviço que correspondem às ações do {{site.data.keyword.keymanagementserviceshort}} que os usuários podem executar.

- Para obter mais informações sobre como designar funções de usuário na IU do {{site.data.keyword.cloud_notm}}, consulte [Gerenciando o acesso ao IAM](/docs/iam?topic=iam-getstarted){: external}.


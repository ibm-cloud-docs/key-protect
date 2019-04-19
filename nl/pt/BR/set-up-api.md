---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# Configurando a API
{: #set-up-api}

O {{site.data.keyword.keymanagementservicefull}} fornece uma API de REST que pode ser usada com qualquer linguagem de programação para armazenar, recuperar e gerar chaves de criptografia.
{: shortdesc}

## Recuperando suas credenciais do {{site.data.keyword.cloud_notm}}
{: #retrieve-credentials}

Para trabalhar com a API, é necessário gerar suas credenciais de serviço e autenticação. 

Para reunir suas credenciais:

1. [Gere um token de acesso do {{site.data.keyword.cloud_notm}} IAM](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Recupere o ID da instância que identifica exclusivamente sua instância de serviço do {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## Formando sua solicitação de API
{: #form-api-request}

Ao fazer uma chamada API para o serviço, estruture sua solicitação de API de acordo com a maneira como você inicialmente
provisionou sua instância do {{site.data.keyword.keymanagementserviceshort}}. 

Para construir sua solicitação, emparelhe um [terminal em serviço
regional](/docs/services/key-protect?topic=key-protect-regions) com as credenciais de autenticação apropriadas. Por exemplo, se você criou uma instância de serviço para a região us-south, use o terminal a seguir e os cabeçalhos de API para procurar chaves em seu serviço:

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

Substitua `<access_token>` e `<instance_ID>` pelas suas credenciais de serviço e autenticação recuperadas.

Deseja rastrear suas solicitações de API no caso de algo sair errado? Quando você inclui o sinalizador `-v` como parte da solicitação cURL, você obtém um valor `correlation-id` nos cabeçalhos de resposta. É possível usar esse valor para correlacionar e rastrear a solicitação para fins de depuração.
{: tip} 

## O que vem a seguir
{: #set-up-api-next-steps}

Você está pronto para começar a gerenciar suas chaves de criptografia no Key Protect. Para descobrir mais sobre como gerenciar programaticamente as suas chaves, [consulte o doc de referência da API do {{site.data.keyword.keymanagementserviceshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

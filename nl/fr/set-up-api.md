---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# Configuration de l'API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} fournit une API REST qui peut être utilisée avec n'importe quel langage de programmation pour stocker, extraire et générer des clés de chiffrement.
{: shortdesc}

## Extraction de vos données d'identification {{site.data.keyword.cloud_notm}}
{: #retrieve-credentials}

Pour utiliser l'API, vous devez générer vos données d'authentification et d'identification de service. 

Pour rassembler vos données d'identification :

1. [Générez un jeton d'accès IAM {{site.data.keyword.cloud_notm}}](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Extrayez l'ID d'instance qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}} de manière univoque](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## Mise en forme de la demande d'API
{: #form-api-request}

Lorsque vous soumettez un appel d'API au service, structurez votre demande d'API en fonction de la manière dont votre instance {{site.data.keyword.keymanagementserviceshort}} a été initialement mise à disposition. 

Pour générer une demande, associez un [noeud final de service régional](/docs/services/key-protect?topic=key-protect-regions) aux données d'authentification appropriées. Par exemple, si vous avez créé une instance de service pour la région `us-south`, utilisez le noeud final et les en-têtes API ci-dessous pour parcourir les clés du service :

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <access_token>' \
    -H 'bluemix-instance: <instance_ID>'
```
{: codeblock} 

Remplacez `<access_token>` et `<instance_ID>` par les données d'authentification et d'identification de service extraites.

Vous souhaitez effectuer le suivi de vos demandes d'API en cas de problème ? Lorsque vous incluez le drapeau `-v` dans le cadre d'une demande cURL, vous obtenez une valeur `correlation-id` dans les en-têtes de réponse. Vous pouvez utiliser cette valeur pour établir une corrélation et suivre la demande à des fins de débogage.
{: tip} 

## Etapes suivantes
{: #set-up-api-next-steps}

Vous êtes maintenant prêt pour commencer à gérer vos clés de chiffrement dans Key Protect. Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, [voir la documentation de référence {{site.data.keyword.keymanagementserviceshort}} API](https://{DomainName}/apidocs/key-protect){: external}.

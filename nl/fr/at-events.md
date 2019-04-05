---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: Activity tracker events, KMS API calls, monitor KMS events

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Evénements {{site.data.keyword.cloudaccesstrailshort}}
{: #activity-tracker-events}

Utilisez le service {{site.data.keyword.cloudaccesstrailfull}} pour savoir comment les utilisateurs et les applications interagissent avec {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

Le service {{site.data.keyword.cloudaccesstrailfull_notm}} enregistre les activités initiées par l'utilisateur qui changent l'état d'un service dans {{site.data.keyword.cloud_notm}}. 

Pour plus d'informations, veuillez consulter la [documentation d'{{site.data.keyword.cloudaccesstrailshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started-with-cla){: new_window}.

## Liste des événements
{: #list-activity-tracker-events}

Le tableau suivant répertorie les actions qui génèrent un événement :

| Action               | Description                 |
| -------------------- | --------------------------- |
| `kms.secrets.create` | Créer une clé                |
| `kms.secrets.read`   | Extraire une clé par ID        |
| `kms.secrets.delete` | Supprimer une clé par ID          |
| `kms.secrets.list`   | Extraire une liste de clés     |
| `kms.secrets.head`   | Extraire le nombre de clés |
| `kms.secrets.wrap`   | Encapsuler une clé                  |
| `kms.secrets.unwrap` | Désencapsuler une clé                |
| `kms.policies.read`  | Afficher la politique d'une clé      |
| `kms.policies.write` | Définir une politique pour une clé       |
{: caption="Tableau 1. Actions qui génèrent des événements {{site.data.keyword.cloudaccesstrailfull_notm}}" caption-side="top"}

## Où consulter les événements
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Les événements {{site.data.keyword.cloudaccesstrailshort}} sont disponibles dans le **domaine de compte** d'{{site.data.keyword.cloudaccesstrailshort}} qui est disponible dans la région {{site.data.keyword.cloud_notm}} dans laquelle les événements sont générés.

Par exemple, lorsque vous créez, importez, supprimez ou lisez une clé dans {{site.data.keyword.keymanagementserviceshort}}, un événement {{site.data.keyword.cloudaccesstrailshort}} est généré. Ces événements sont transmis automatiquement au service {{site.data.keyword.cloudaccesstrailshort}} dans la région dans laquelle le service {{site.data.keyword.keymanagementserviceshort}} a été mis à disposition.

Pour surveiller l'activité des API, vous devez mettre à disposition le service {{site.data.keyword.cloudaccesstrailshort}} dans un espace qui est disponible dans la région dans laquelle le service {{site.data.keyword.keymanagementserviceshort}} a été mis à disposition. Ensuite, vous pouvez afficher les événements via la vue de compte dans l'interface utilisateur d'{{site.data.keyword.cloudaccesstrailshort}} si vous utilisez un plan Lite, et via Kibana si vous utilisez un plan Premium.

## Informations supplémentaires
{: #activity-tracker-info}

En raison de la sensibilité des informations pour une clé de chiffrement, lorsqu'un événement est généré suite à un appel API du service {{site.data.keyword.keymanagementserviceshort}}, il n'inclut pas d'informations détaillées sur la clé. Il inclut un ID de corrélation que vous pouvez utiliser pour identifier la clé en interne dans votre environnement cloud. L'ID de corrélation est une zone renvoyée dans la zone `responseHeader.content`. Vous pouvez utiliser cette information pour corréler la clé {{site.data.keyword.keymanagementserviceshort}} avec les informations de l'action rapportées via l'événement {{site.data.keyword.cloudaccesstrailshort}}.

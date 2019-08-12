---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: key management service, kms, manage encryption keys, data encryption, data-at-rest, protect data encryption keys

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

# Tutoriel d'initiation
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} permet de mettre à disposition des clés chiffrées pour des applications au sein des services {{site.data.keyword.cloud_notm}}. Ce tutoriel explique comment créer et ajouter des clés cryptographiques à l'aide du tableau de bord {{site.data.keyword.keymanagementserviceshort}} pour pouvoir gérer le chiffrement de données à partir d'un même emplacement central.
{: shortdesc}

## Initiation aux clés de chiffrement
{: #get-started-keys}

Dans le tableau de bord {{site.data.keyword.keymanagementserviceshort}}, vous pouvez créer des clés pour la cryptographie ou importer des clés existantes. 

Sélectionnez l'un des deux types de clé :

<dl>
  <dt>Root keys (Clés racine)</dt>
    <dd>Les clés racine sont des clés d'encapsulage de clés symétriques que vous pouvez gérer dans leur intégralité dans {{site.data.keyword.keymanagementserviceshort}}. Vous pouvez utiliser une clé racine pour protéger d'autres clés cryptographiques avec un mécanisme de chiffrement avancé. Pour en savoir plus, voir <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">Protection des données avec le chiffrement d'enveloppe</a>.</dd>
  <dt>Standard keys (Clés standard)</dt>
    <dd>Les clés standard sont des clés symétriques utilisées pour la cryptographie. Vous pouvez utiliser une clé standard pour chiffrer et déchiffrer directement des données.</dd>
</dl>

## Création de clés
{: #create-keys}

[Une fois que vous avez créé une instance {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps), vous êtes prêt à désigner des clés dans le service. 

Pour créer votre première clé cryptographique, procédez comme suit : 

1. Sur la page des détails de l'application, cliquez sur **Gérer** &gt; **Ajouter une clé**.
2. Pour créer une clé, sélectionnez la fenêtre **Créer une clé**.

    Indiquez les détails relatifs à la clé :

    <table>
      <tr>
        <th>Paramètre</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Alias unique et lisible permettant d'identifier facilement votre clé.</p>
          <p>Pour protéger votre vie privée, assurez-vous que le nom de clé ne contient pas d'informations identifiant la personne, comme votre nom ou votre emplacement.</p>
        </td>
      </tr>
      <tr>
        <td>Key type</td>
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Type de clé</a> que vous souhaitez gérer dans {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 1. Description des paramètres de la fenêtre <b>Créer une clé</b></caption>
    </table>

3. Une fois les détails de la clé indiqués, cliquez sur **Create key** pour confirmer l'opération. 

Les clés qui sont créées dans le service sont des clés symétriques de 256 bits prises en charge par l'algorithme AES-CBC-PAD. Pour renforcer la sécurité, elles sont générées par des modules de sécurité matériels (ou modules HSM) certifiés FIPS 140-2 niveau 3 résidant dans des centres de données {{site.data.keyword.cloud_notm}} sécurisés. 

## Importation de vos propres clés
{: #import-keys}

Vous pouvez bénéficier des avantages du mécanisme BYOK (Bring Your Own Key) en intégrant vos propres clés au service. 

Pour ajouter une clé existante, procédez comme suit :

1. Sur la page des détails de l'application, cliquez sur **Gérer** &gt; **Ajouter une clé**.
2. Pour télécharger une clé existante, sélectionnez la fenêtre **Importation de vos propres clés**.

    Indiquez les détails relatifs à la clé :

    <table>
      <tr>
        <th>Paramètre</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Alias unique et lisible permettant d'identifier facilement votre clé.</p>
          <p>Pour protéger votre vie privée, assurez-vous que le nom de clé ne contient pas d'informations identifiant la personne, comme votre nom ou votre emplacement.</p>
        </td>
      </tr>
      <tr>
        <td>Key type</td>
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Type de clé</a> que vous souhaitez gérer dans {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <tr>
        <td>Key material</td>
        <td>Matériel de clé, comme une clé symétrique, que vous voulez stocker dans le service {{site.data.keyword.keymanagementserviceshort}}. La clé que vous indiquez doit être codée en base64.</td>
      </tr>
      <caption style="caption-side:bottom;">Tableau 2. Description des paramètres de la fenêtre <b>Importation de vos propres clés</b></caption>
    </table>

3. Une fois les détails de la clé indiqués, cliquez sur **Importer une clé** pour confirmer l'opération. 

Dans le tableau de bord {{site.data.keyword.keymanagementserviceshort}}, vous pouvez examiner les caractéristiques générales des nouvelles clés. 

## Etapes suivantes
{: #get-started-next-steps}

Vous pouvez désormais utiliser vos clés pour coder vos applications et services. Si vous avez ajouté une clé racine au service, vous voudrez peut-être en savoir plus sur l'utilisation de la clé racine pour protéger les clés qui chiffrent vos données au repos. Reportez-vous à [Encapsulage de clés](/docs/services/key-protect?topic=key-protect-wrap-keys) pour commencer.

- Pour plus d'informations sur la gestion et la protection de vos clés de chiffrement avec une clé racine, voir [Protection des données avec le chiffrement d'enveloppe](/docs/services/key-protect?topic=key-protect-envelope-encryption).
- Pour plus d'informations sur l'intégration du service {{site.data.keyword.keymanagementserviceshort}} à d'autres solutions de données de cloud, [voir la documentation relative aux intégrations](/docs/services/key-protect?topic=key-protect-integrate-services).
- Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme, [voir la documentation de référence {{site.data.keyword.keymanagementserviceshort}} API](https://cloud.ibm.com/apidocs/key-protect){: external}.

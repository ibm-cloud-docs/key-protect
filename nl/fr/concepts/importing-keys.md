---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# Apport de vos clés de chiffrement au cloud
{: #importing-keys}

Les clés de chiffrement contiennent des sous-ensembles d'information, comme des métadonnées, qui vous aident à identifier la clé et le _matériel de clé_ utilisé pour chiffrer et déchiffrer les données.
{: shortdesc}

Lorsque vous utilisez {{site.data.keyword.keymanagementserviceshort}} pour créer des clés, le service génère pour vous un matériel de clé cryptographique qui repose sur des modules de sécurité matériels basés sur le cloud. Cependant, selon les besoins de votre entreprise, vous devrez peut-être générer le matériel de clé à partir de votre solution interne, puis étendre votre infrastructure de gestion des clés sur site au cloud, en important les clés dans {{site.data.keyword.keymanagementserviceshort}}.

<table>
  <th>Avantage</th>
  <th>Description</th>
  <tr>
    <td>Apport de vos propres clés (BYOK) </td>
    <td>Vous souhaitez contrôler et renforcer vos pratiques de gestion des clés en générant des clés fortes à partir de votre module HSM sur site. Si vous choisissez d'exporter des clés symétriques depuis votre infrastructure interne de gestion des clés, vous pouvez utiliser {{site.data.keyword.keymanagementserviceshort}} pour les apporter dans le cloud en toute sécurité.</td>
  </tr>
  <tr>
    <td>Importation sécurisée du matériel de clé racine</td>
    <td>Lorsque vous exportez vos clés vers le cloud, vous voulez être sûr que le matériel de clé est protégé durant son transit. Atténuez les attaques MiTM en <a href="#transport-keys">utilisant une clé de transport</a> pour importer le matériel de clé racine en toute sécurité dans votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</td>
  </tr>
  <caption style="caption-side:bottom;">Tableau 1. Description des avantages de l'importation du matériel de clé</caption>
</table>


## Planification de l'importation du matériel de clé
{: #plan-ahead}

Tenez compte des considérations suivantes lorsque vous êtes prêt à importer le matériel de clé racine dans le service.

<dl>
  <dt>Examinez vos options de création de matériel de clé</dt>
    <dd>Explorez les options disponibles pour créer des clés de chiffrement symétrique de 256 bits en fonction de vos besoins de sécurité. Par exemple, vous pouvez utiliser votre système interne de gestion des clés basé sur un module de sécurité matériel sur site validé par le FIPS pour générer le matériel de clé avant d'apporter vos clés dans le cloud. Si vous construisez une preuve de concept, vous pouvez également utiliser un kit d'outils de cryptographie tel que <a href="https://www.openssl.org/" target="_blank">OpenSSL</a> pour générer le matériel de clé, que vous pouvez importer dans {{site.data.keyword.keymanagementserviceshort}} pour vos besoins de test.</dd>
  <dt>Choisissez une option pour importer le matériel de clé dans {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>Choisissez parmi deux options d'importation de clés racine en fonction du niveau de sécurité requis par votre environnement ou votre charge de travail. Par défaut, {{site.data.keyword.keymanagementserviceshort}} chiffre votre matériel de clé pendant son transit à l'aide du protocole Transport Layer Security (TLS) 1.2. Si vous construisez une preuve de concept ou testez le service pour la première fois, vous pouvez importer le matériel de clé racine dans {{site.data.keyword.keymanagementserviceshort}} en utilisant cette option par défaut. Si votre charge de travail nécessite un mécanisme de sécurité supérieur à TLS, vous pouvez également <a href="#transport-keys">utiliser une clé de transport</a> pour chiffrer et importer le matériel de clé racine dans le service.</dd>
  <dt>Planifiez le chiffrement de votre matériel de clé</dt>
    <dd>Si vous choisissez de chiffrer votre matériel de clé à l'aide d'une clé de transport, définissez une méthode pour exécuter le chiffrement RSA sur le matériel de clé. Vous devez utiliser le schéma de chiffrement <code>RSAES_OAEP_SHA_256</code> comme spécifié par la norme <a href="https://tools.ietf.org/html/rfc3447" target="_blank">PKCS #1 v2.1 relative au chiffrement RSA</a>. Examinez les capacités de votre système interne de gestion des clés ou de votre module HSM sur site pour définir vos options.</dd>
  <dt>Gérez le cycle de vie du matériel de clé importé</dt>
    <dd>Après avoir importé le matériel de clé dans le service, gardez à l'esprit que vous êtes responsable de la gestion du cycle de vie complet de votre clé. Avec l'API {{site.data.keyword.keymanagementserviceshort}}, vous pouvez définir une date d'expiration pour la clé lorsque vous décidez de la télécharger dans le service. Cependant, si vous souhaitez <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">effectuer une rotation d'une clé racine importée</a>, vous devez générer et fournir un nouveau matériel de clé pour retirer et remplacer la clé existante. </dd>
</dl>

## Utilisation des clés de transport
{: #transport-keys}

Les clés de transport sont actuellement une fonction bêta. Les fonctions bêta peuvent changer à tout moment, et les mises à jour futures peuvent introduire des changements qui sont incompatibles avec la dernière version.
{: important}

Si vous souhaitez chiffrer votre matériel de clé avant de l'importer dans {{site.data.keyword.keymanagementserviceshort}}, vous pouvez créer une clé de chiffrement de transport pour votre instance de service à l'aide de l'API {{site.data.keyword.keymanagementserviceshort}}. 

Les clés de transport sont un type de ressources dans {{site.data.keyword.keymanagementserviceshort}} qui permettent l'importation sécurisée d'un matériel de clé racine vers votre instance de service. En utilisant une clé de transport pour chiffrer votre matériel de clé local, vous protégez les clés racine durant leur transit vers {{site.data.keyword.keymanagementserviceshort}} conformément aux politiques que vous spécifiez. Par exemple, vous pouvez définir une politique sur la clé de transport qui limite l'utilisation de la clé en fonction du temps et du nombre d'utilisations.

### Fonctionnement
{: #how-transport-keys-work}

Lorsque vous [créez une clé de transport](/docs/services/key-protect?topic=key-protect-create-transport-keys) pour votre instance de service, {{site.data.keyword.keymanagementserviceshort}} génère une clé RSA de 4096 bits. Le service chiffre la clé privée puis renvoie la clé publique et un jeton d'importation que vous pouvez utiliser pour chiffrer et importer votre matériel de clé racine. 

Lorsque vous êtes prêt à [importer une clé racine](/docs/services/key-protect?topic=key-protect-import-root-keys#api) dans {{site.data.keyword.keymanagementserviceshort}}, vous fournissez le matériel de clé racine chiffré et le jeton d'importation qui est utilisé pour vérifier l'intégrité de la clé publique. Pour effectuer la demande, {{site.data.keyword.keymanagementserviceshort}} utilise la clé privée associée à votre instance de service pour déchiffrer le matériel de clé racine chiffré. Ce processus assure que seule la clé de transport que vous avez générée dans {{site.data.keyword.keymanagementserviceshort}} peut déchiffrer le matériel de clé que vous importez et stockez dans le service.

Vous ne pouvez créer qu'une seule clé de transport par instance de service. Pour en savoir plus sur les limites d'extraction des clés de transport, veuillez consulter la [documentation sur les références d'API de {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### Méthodes de l'API
{: #secure-import-api-methods}

En arrière-plan, l'API {{site.data.keyword.keymanagementserviceshort}} exécute le processus de création des clés de transport.  

Le tableau suivant répertorie les méthodes de l'API qui configurent un élément de verrouillage et créent des clés de transport pour votre instance de service.

<table>
  <tr>
    <th>Méthode</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Créer une clé de transport</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Extraire des métadonnées de clé de transport</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Extraire une clé de transport et un jeton d'importation</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tableau 2. Description des méthodes de l'API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Pour plus d'informations sur la gestion de vos clés à l'aide d'un programme dans {{site.data.keyword.keymanagementserviceshort}}, voir la [documentation de référence de l'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/key-protect){: new_window}.

## Etapes suivantes

- Pour savoir comment créer une clé de transport pour votre instance de service, voir [Création d'une clé de transport](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- Pour en savoir plus sur l'importation des clés dans le service, voir [Importation de clés racine](/docs/services/key-protect?topic=key-protect-import-root-keys). 

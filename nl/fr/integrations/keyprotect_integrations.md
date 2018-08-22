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

# Intégration de services
{: #integrations}

{{site.data.keyword.keymanagementservicefull}} s'intègre aux solutions de données et de stockage pour vous aider à apporter et gérer votre propre chiffrement dans le cloud.
{: shortdesc}

[Après avoir créé une instance du service](/docs/services/keymgmt/keyprotect_provision.html), vous pouvez intégrer {{site.data.keyword.keymanagementserviceshort}} aux services pris en charge suivants :

<table>
    <tr>
        <th>Maintenance</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>Ajoutez le [chiffrement d'enveloppe](/docs/services/keymgmt/concepts/keyprotect_envelope.html) à vos compartiments de stockage à l'aide de {{site.data.keyword.keymanagementserviceshort}}. Utilisez les clés racine que vous gérez dans {{site.data.keyword.keymanagementserviceshort}} pour protéger les clés de chiffrement de données qui chiffrent vos données au repos. </p>
          <p>Pour plus d'informations, voir [Intégration à {{site.data.keyword.cos_full_notm}}](/docs/services/keymgmt/integrations/keyprotect_cloud-object-storage.html).</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">Tableau 1. Description des intégrations qui sont disponibles pour {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

## Compréhension de votre intégration 
{: #understand_integration}

Lorsque vous intégrez un service pris en charge à {{site.data.keyword.keymanagementserviceshort}}, vous activez le [chiffrement d'enveloppe](/docs/services/keymgmt/concepts/keyprotect_envelope.html) pour ce service. Cette intégration vous permet d'utiliser une clé racine que vous stockez dans {{site.data.keyword.keymanagementserviceshort}} pour encapsuler les clés de chiffrement de données qui chiffrent vos données au repos.  

Par exemple, vous pouvez créer une clé racine, gérer la clé dans {{site.data.keyword.keymanagementserviceshort}} et utiliser la clé racine pour protéger les données qui sont stockées dans différents services cloud. 

![Diagramme illustrant une vue contextuelle de votre intégration {{site.data.keyword.keymanagementserviceshort}}.](../images/kp-integrations_min.svg)

### Méthodes d'API {{site.data.keyword.keymanagementserviceshort}}
{: #api_methods}

En arrière-plan, l'API {{site.data.keyword.keymanagementserviceshort}} exécute la procédure de chiffrement d'enveloppe.   

Le tableau suivant répertorie les méthodes d'API qui ajoutent ou retirent le chiffrement d'enveloppe sur une ressource :

<table>
  <tr>
    <th>Méthode</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_wrap_keys.html">Encapsule (chiffre) une clé de chiffrement de données</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/keymgmt/keyprotect_unwrap_keys.html">Désencapsule (déchiffre) une clé de chiffrement de données</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tableau 2. Description des méthodes d'API {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Pour en savoir plus sur la gestion des clés à l'aide d'un programme dans {{site.data.keyword.keymanagementserviceshort}}, voir les exemples de code dans la[documentation de référence d'API {{site.data.keyword.keymanagementserviceshort}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/apidocs/639){: new_window}.
{: tip}

## Intégration d'un service pris en charge
{: #grant_access}

Pour ajouter une intégration, créez une autorisation entre les services à l'aide du tableau de bord {{site.data.keyword.iamlong}}. Les autorisations activent des règles d'accès de service à service pour vous permettre d'associer une ressource de votre service de données de cloud à une [clé racine](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que vous gérez dans {{site.data.keyword.keymanagementserviceshort}}.

Prenez soin de mettre à disposition les deux services dans la même région avant de créer une autorisation. Pour en savoir plus sur les autorisations de service, voir [Octroi de droits d'accès entre les services ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](/docs/iam/authorizations.html){: new_window}.
{: tip}

Lorsque vous êtes prêt à intégrer un service, procédez comme suit pour créer une autorisation :

1. [Connectez-vous à la console {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/){: new_window}.
2. Dans la barre de menus, cliquez sur **Gérer** &gt; **Sécurité** &gt; **Identity and Access**, puis sélectionnez **Autorisations**. 
3. Cliquez sur **Créer**.
4. Sélectionnez une source et une cible pour l'autorisation.
 
  - Pour **Service source**, sélectionnez le service de données de cloud que vous souhaitez intégrer à {{site.data.keyword.keymanagementserviceshort}}. Par exemple, **Cloud Object Storage**.
  - Pour **Service cible**, sélectionnez **{{site.data.keyword.keymanagementservicelong_notm}}**. 
4. Pour accorder un accès en lecture seule entre les services, cochez la case **Lecteur**.

    Avec les droits _Lecteur_, votre service source peut parcourir les clés racine mises à disposition dans l'instance {{site.data.keyword.keymanagementserviceshort}} indiquée.
5. Cliquez sur **Autoriser**.

### Etapes suivantes

Ajoutez un chiffrement avancé à vos ressources de cloud en créant une clé racine dans {{site.data.keyword.keymanagementserviceshort}}. Ajoutez une nouvelle ressource à un service de données de cloud pris en charge, puis sélectionnez la clé racine que vous souhaitez utiliser pour le chiffrement avancé. 

- Pour en savoir plus sur la création de clés racine avec le service {{site.data.keyword.keymanagementserviceshort}}, voir [Création de clés racine](/docs/services/keymgmt/keyprotect_create_root.html).
- Pour en savoir plus sur l'apport de vos propres clés racine au service {{site.data.keyword.keymanagementserviceshort}}, voir [Importation de clés racine](/docs/services/keymgmt/keyprotect_import_root.html).



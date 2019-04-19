---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Key Protect CLI plug-in, CLI reference

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

# Informations de référence sur l'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}
{: #cli-reference}

Vous pouvez utiliser le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}} pour gérer les clés de votre instance de {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

Pour installer le plug-in d'interface de ligne de commande, voir [Configuration de l'interface de ligne de commande](/docs/services/key-protect?topic=key-protect-set-up-cli). 

Lorsque vous vous connectez à l'[interface de ligne de commande d'{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}, vous êtes averti lorsque des mises à jour sont disponibles. Veillez à tenir à jour votre interface de ligne de commande de manière à pouvoir utiliser les commandes et indicateurs disponibles pour le plug-in d'interface de ligne de commande de {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

## Commandes ibmcloud kp
{: #ibmcloud-kp-commands}

Vous pouvez spécifier l'une des commandes suivantes :

<table summary="Commandes de gestion des clés"> 
    <thead>
        <th colspan="5">Commandes de gestion des clés</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Table 1. Commandes de gestion des clés</caption> 
 </table>

 <table summary="Commandes de gestion des règles de clés">
    <thead>
        <th colspan="5">Commandes de gestion des règles de clés</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-policy-list">kp policy list</a></td>
            <td><a href="#kp-policy-get">kp policy get</a></td>
            <td><a href="#kp-policy-set">kp policy set</a></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tableau 2. Commandes de gestion des règles de clés</caption> 
 </table>

## kp create
{: #kp-create}

[Créer une clé racine](/docs/services/key-protect?topic=key-protect-create-root-keys) dans l'instance de service {{site.data.keyword.keymanagementserviceshort}} que vous indiquez. 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

### Paramètres requis
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>Alias unique et lisible à affecter à votre clé.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Elément de clé codée en base64 que vous voulez stocker et gérer dans le service. Pour importer une clé existante, indiquez une clé de 256 bits. Pour générer une nouvelle clé, omettez le paramètre <code>--key-material</code>.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Définissez ce paramètre uniquement si vous voulez créer une <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">clé standard</a>. Pour créer une clé racine, omettez le paramètre <code>--standard-key</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Supprimer une clé](/docs/services/key-protect?topic=key-protect-delete-keys) stockée dans votre service {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Paramètres requis
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>ID de la clé à supprimer. Pour obtenir la liste des clés disponibles, exécutez la commande <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

## kp list
{: #kp-list}

Répertorier les 200 dernières clés disponibles dans votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Paramètres requis
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Extrayez les détails d'une clé, comme les métadonnées de clé et le matériel de clé.

Si la clé a été conçue en tant que clé racine, le système ne peut pas renvoyer le matériel de clé pour cette clé.

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Paramètres requis
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé que vous souhaitez extraire. Pour obtenir la liste des clés disponibles, exécutez la commande <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Procéder à la rotation d'une clé racine](/docs/services/key-protect?topic=key-protect-wrap-keys) stockée dans votre service {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL] 
```
{: pre}

### Paramètres requis
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé racine à laquelle appliquer la rotation.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Elément de clé codée en base64 que vous voulez utiliser pour procéder à la rotation d'une clé racine existante. Pour procéder à la rotation d'une clé initialement importée dans le service, fournissez une nouvelle clé de 256 bits. Pour procéder à la rotation d'une clé initialement générée dans {{site.data.keyword.keymanagementserviceshort}}, omettez le paramètre <code>--key-material</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Encapsuler une clé de chiffrement de données](/docs/services/key-protect?topic=key-protect-wrap-keys) à l'aide d'une clé racine stockée dans l'instance de service {{site.data.keyword.keymanagementserviceshort}} que vous spécifiez.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Paramètres requis
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé racine que vous voulez utiliser pour l'encapsulage.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Données d'authentification supplémentaires (AAD) utilisées pour sécuriser davantage une clé. Si elles sont fournies à l'encapsulage, elles doivent l'être au désencapsulage.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>Clé de chiffrement de données codée en base64 (clé DEK) que vous voulez gérer et protéger. Pour importer une clé existante, indiquez une clé de 256 bits. Pour générer et encapsuler une nouvelle clé DEK, omettez le paramètre <code>--plaintext</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Désencapsuler une clé de chiffrement de données](/docs/services/key-protect?topic=key-protect-unwrap-keys) à l'aide d'une clé racine stockée dans votre instance de service {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Paramètres requis
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé racine que vous avez utilisée pour la demande d'encapsulage initiale.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>Clé de chiffrement de données renvoyée au cours de l'opération d'encapsulage initiale.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Données d'authentification supplémentaires (AAD) utilisées pour sécuriser davantage une clé. Vous pouvez entrer jusqu'à 255 chaînes séparées par une virgule. Si vous indiquez des données d'authentification supplémentaires à l'encapsulage, vous devez indiquer les mêmes données lors du désencapsulage.</p><p><b>Important :</b> le service {{site.data.keyword.keymanagementserviceshort}} ne sauvegarde pas les données d'authentification supplémentaires. Si vous indiquez des données d'authentification supplémentaires, sauvegardez ces données dans un emplacement sécurisé afin de pouvoir y accéder et fournir les mêmes données lors des demandes de désencapsulage ultérieures.</p></dd>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

Répertorier les règles associées à la clé racine spécifiée.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Paramètres requis
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé racine à demander. Pour obtenir la liste des clés disponibles, exécutez la commande <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #policy-list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Extraire les détails d'une règle de clé, comme l'intervalle de rotation automatique de la clé.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Paramètres requis
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé à demander. Pour obtenir la liste des clés disponibles, exécutez la commande <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #policy-get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Créer ou remplacer la règle associée à la clé racine spécifiée.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Paramètres requis
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>ID de la clé à demander. Pour obtenir la liste des clés disponibles, exécutez la commande <a href="#kp-list">kp list</a>.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Spécifiez le type de règle à définir. Pour définir une règle de rotation, utilisez <code>--set-type rotation</code>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID instance {{site.data.keyword.cloud_notm}} qui identifie votre instance de service {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Paramètres facultatifs
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Spécifiez l'intervalle de rotation (en mois) pour une clé. La valeur par défaut est 1.</dd>
    <dt><code>--output</code></dt>
        <dd>Définissez le format de sortie de l'interface de ligne de commande. Par défaut, toutes les commandes impriment au format tableau. Pour modifier le format de sortie à JSON, utilisez <code>--output json</code>.</dd>
</dl>


---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Referenzinformationen zur Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}
{: #key-protect-cli}

Mithilfe des Plug-ins für die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.keymanagementserviceshort}} können Sie Schlüssel in Ihrer Instanz von {{site.data.keyword.keymanagementserviceshort}} verwalten.
{:shortdesc}

Informationen zur Installation der Befehlszeilenschnittstelle finden Sie in [Befehlszeilenschnittstelle einrichten](/docs/services/key-protect/set-up-cli.html).  

Wenn Sie sich bei der [Befehlszeilenschnittstelle von {{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/index.html#overview){: new_window} anmelden, erhalten Sie eine Benachrichtigung, sobald Aktualisierungen verfügbar sind. Stellen Sie sicher, dass sich die Befehlszeilenschnittstelle stets auf dem neuesten Stand befindet, damit Sie die Befehle und Flags nutzen können, die für das Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} verfügbar sind.
{: tip}

## ibmcloud kp commands
{: #ibmcloud-kp-commands}

Sie können einen der folgenden Befehle angeben:

<table summary="Befehle für die Verwaltung von Schlüsseln">
    <thead>
        <th colspan="5">Befehle für die Verwaltung von Schlüsseln</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
        </tr>
        <tr>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabelle 1. Befehle für die Verwaltung von Schlüsseln</caption> 
 </table>

## kp create
{: #kp-create}

[Erstellen Sie einen Rootschlüssel](/docs/services/key-protect/create-root-keys.html) in der angegebenen {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz.  

```sh
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
```
{:pre}

### Erforderliche Parameter
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>Ein eindeutiger, lesbarer Alias, der dem Schlüssel zugewiesen wird.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Die mit einer Base64-Codierung verschlüsselten Schlüsselinformationen, die im Service gespeichert und verwaltet werden sollen. Geben Sie zum Importieren eines bereits vorhandenen Schlüssels einen 256-Bit-Schlüssel an. Wenn Sie einen neuen Schlüssel generieren möchten, geben Sie den Parameter <code>--key-material</code> nicht an. </dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Legen Sie den Parameter nur fest, wenn Sie einen <a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">Standardschlüssel</a> erstellen möchten. Wenn Sie einen Rootschlüssel erstellen möchten, geben Sie den Parameter <code>--standard-key</code> nicht an. </dd>
</dl>

## kp delete
{: #kp-delete}

[Löschen Sie einen Schlüssel](/docs/services/key-protect/delete-keys.html), der im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert ist. 

```sh
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Erforderliche Parameter
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>Die ID des Schlüssels, der gelöscht werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen. </dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

## kp list
{: #kp-list}

Listen Sie die letzten 200 Schlüssel auf, die in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verfügbar sind. 

```sh
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Erforderliche Parameter
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Führen Sie eine Rotation für einen Rootschlüssel durch](/docs/services/key-protect/wrap-keys.html), der im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert ist. 

```sh
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --key-material KEY_MATERIAL] 
```
{: pre}

### Erforderliche Parameter
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels, für den eine Rotation durchgeführt werden soll. </dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Die mit einer Base64-Codierung verschlüsselten Schlüsselinformationen, die für die Rotation eines vorhandenen Rootschlüssels verwendet werden sollen. Geben Sie für die Rotation eines Schlüssels, der ursprünglich in den Service importiert wurde, einen neuen 256-Bit-Schlüssel an. Geben Sie für die Rotation eines Schlüssels, der ursprünglich in {{site.data.keyword.keymanagementserviceshort}} generiert wurde, den Parameter <code>--key-material</code> nicht an. </dd>
</dl>

## kp wrap
{: #kp-wrap}

[Führen Sie Wrapping für einen Datenverschlüsselungsschlüssel durch](/docs/services/key-protect/wrap-keys.html), indem Sie einen Rootschlüssel verwenden, der in der angegebenen {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gespeichert ist. 

```sh
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Erforderliche Parameter
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels, der für das Wrapping verwendet werden soll. </dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Die zusätzlichen Authentifizierungsdaten (AAD), die für den zusätzlichen Schutz eines Schlüssels verwendet werden. Werden die Daten beim Wrapping angegeben, müssen sie auch beim Aufheben des Wrappings angegeben werden. </dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>Der mit einer Base64-Codierung verschlüsselte Datenverschlüsselungsschlüssel (DEK), der verwaltet und geschützt werden soll. Geben Sie zum Importieren eines bereits vorhandenen Schlüssels einen 256-Bit-Schlüssel an. Geben Sie für die Generierung und das Wrapping eines neuen DEK den Parameter <code>--plaintext</code> nicht an. </dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Heben Sie das Wrapping eines Datenverschlüsselungsschlüssels auf](/docs/services/key-protect/unwrap-keys.html), indem Sie einen Rootschlüssel verwenden, der in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gespeichert ist. 

```sh
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Erforderliche Parameter
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels der ursprünglichen Wrapping-Anforderung. </dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>Der verschlüsselte Datenschlüssel, der bei der ursprünglichen Wrapping-Operation zurückgegeben wurde. </dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Die zusätzlichen Authentifizierungsdaten (AAD), die für den zusätzlichen Schutz eines Schlüssels verwendet wurden. Es können bis zu 255 Zeichenfolgen, jeweils durch Kommas getrennt, angegeben werden. Wenn die AAD beim Wrapping angegeben wurden, müssen dieselben AAD beim Aufheben des Wrappings angegeben werden. </p><p><b>Wichtig:</b> Der {{site.data.keyword.keymanagementserviceshort}}-Service speichert keine zusätzlichen Authentifizierungsdaten. Wenn AAD angegeben werden, speichern Sie die Daten an einer sicheren Position, um sicherzustellen, dass Sie bei nachfolgenden Anforderungen zum Aufheben des Wrappings auf dieselben AAD zugreifen und diese angeben können.</p></dd>
</dl>




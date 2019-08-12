---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect CLI plug-in, CLI reference

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

# Referenzinformationen zur Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}
{: #cli-reference}

Mithilfe des Plug-ins für die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.keymanagementserviceshort}} können Sie Schlüssel in Ihrer Instanz von {{site.data.keyword.keymanagementserviceshort}} verwalten.
{:shortdesc}

Informationen zur Installation der Befehlszeilenschnittstelle finden Sie in [Befehlszeilenschnittstelle einrichten](/docs/services/key-protect?topic=key-protect-set-up-cli). 

Wenn Sie sich an der [{{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle](/docs/cli?topic=cloud-cli-getting-started){: external} anmelden, werden Sie bei Verfügbarkeit von Aktualisierungen benachrichtigt. Stellen Sie sicher, dass sich die Befehlszeilenschnittstelle stets auf dem neuesten Stand befindet, damit Sie die Befehle und Flags nutzen können, die für das Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} verfügbar sind.
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
    <caption style="caption-side:bottom;">Tabelle 1. Befehle für die Verwaltung von Schlüsseln</caption> 
 </table>

 <table summary="Befehle für die Verwaltung von Schlüsselrichtlinien"> 
    <thead>
        <th colspan="5">Befehle für die Verwaltung von Schlüsselrichtlinien</th>
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
    <caption style="caption-side:bottom;">Tabelle 2. Befehle für die Verwaltung von Schlüsselrichtlinien</caption> 
 </table>

## kp create
{: #kp-create}

[Erstellen Sie einen Rootschlüssel](/docs/services/key-protect?topic=key-protect-create-root-keys) in der angegebenen {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz. 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

```sh
$ ibmcloud kp create sample-root-key -i $KP_INSTANCE_ID
SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
```
{:screen}

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
        <dd>Die mit einer Base64-Codierung verschlüsselten Schlüsselinformationen, die im Service gespeichert und verwaltet werden sollen. Geben Sie zum Importieren eines bereits vorhandenen Schlüssels einen 256-Bit-Schlüssel an. Wenn Sie einen neuen Schlüssel generieren möchten, geben Sie den Parameter <code>--key-material</code> nicht an.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Legen Sie den Parameter nur fest, wenn Sie einen <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Standardschlüssel</a> erstellen möchten. Wenn Sie einen Rootschlüssel erstellen möchten, geben Sie den Parameter <code>--standard-key</code> nicht an.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Löschen Sie einen Schlüssel](/docs/services/key-protect?topic=key-protect-delete-keys), der im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert ist.

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp delete 584fb0d9-dec2-47b8-bde5-50f05fd66261 -i $KP_INSTANCE_ID
Deleting key: 584fb0d9-dec2-47b8-bde5-50f05fd66261, from instance: 98d39ab8-cf44-4517-9583-2ad05c7e9bd5...

SUCCESS

Deleted Key
584fb0d9-dec2-47b8-bde5-50f05fd66261
```
{: screen}

### Erforderliche Parameter
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>Die ID des Schlüssels, der gelöscht werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

## kp list
{: #kp-list}

Listen Sie die letzten 200 Schlüssel auf, die in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verfügbar sind.

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp list -i $KP_INSTANCE_ID
Retrieving keys...

SUCCESS

Key ID                                 Key Name
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key
92e5fab3-00e8-40e9-8a2d-864de334b043   sample-imported-root-key
```
{: screen}

### Erforderliche Parameter
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Rufen Sie die Details zu einem Schlüssel ab, wie z. B. Schlüsselmetadaten und Schlüsselinformationen.

Wenn der Schlüssel als Rootschlüssel bezeichnet wurde, kann das System die Schlüsselinformationen für diesen Schlüssel nicht zurückgeben.

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

```sh
$ ibmcloud kp get 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Grabbing info for key id: 3df42bc2-a991-41cb-acc2-3f9eab64a63f...

SUCCESS

Key ID                                 Key Name          Description     Creation Date                   Expiration Date
3df42bc2-a991-41cb-acc2-3f9eab64a63f   sample-root-key   A sample key.   2019-04-02 16:42:47 +0000 UTC   Key does not expire
```
{:screen}

### Erforderliche Parameter
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Schlüssels, der abgerufen werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Führen Sie eine Rotation für einen Rootschlüssel durch](/docs/services/key-protect?topic=key-protect-wrap-keys), der im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert ist.

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL] 
```
{: pre}

```sh
$ ibmcloud kp rotate 3df42bc2-a991-41cb-acc2-3f9eab64a63f -i $KP_INSTANCE_ID
Rotating root key...

SUCCESS
```
{:screen}

### Erforderliche Parameter
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels, für den eine Rotation durchgeführt werden soll.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Die mit einer Base64-Codierung verschlüsselten Schlüsselinformationen, die für die Rotation eines vorhandenen Rootschlüssels verwendet werden sollen. Geben Sie für die Rotation eines Schlüssels, der ursprünglich in den Service importiert wurde, einen neuen 256-Bit-Schlüssel an. Geben Sie für die Rotation eines Schlüssels, der ursprünglich in {{site.data.keyword.keymanagementserviceshort}} generiert wurde, den Parameter <code>--key-material</code> nicht an.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Führen Sie Wrapping für einen Datenverschlüsselungsschlüssel durch](/docs/services/key-protect?topic=key-protect-wrap-keys), indem Sie einen Rootschlüssel verwenden, der in der angegebenen {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gespeichert ist.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Erforderliche Parameter
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels, der für das Wrapping verwendet werden soll.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Die zusätzlichen Authentifizierungsdaten (AAD), die für den zusätzlichen Schutz eines Schlüssels verwendet werden. Werden die Daten beim Wrapping angegeben, müssen sie auch beim Aufheben des Wrappings angegeben werden.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>Der mit einer Base64-Codierung verschlüsselte Datenverschlüsselungsschlüssel (DEK), der verwaltet und geschützt werden soll. Geben Sie zum Importieren eines bereits vorhandenen Schlüssels einen 256-Bit-Schlüssel an. Geben Sie für die Generierung und das Wrapping eines neuen DEK den Parameter <code>--plaintext</code> nicht an.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Heben Sie das Wrapping eines Datenverschlüsselungsschlüssels auf](/docs/services/key-protect?topic=key-protect-unwrap-keys), indem Sie einen Rootschlüssel verwenden, der in der {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz gespeichert ist.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Erforderliche Parameter
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels der ursprünglichen Wrapping-Anforderung.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>Der verschlüsselte Datenschlüssel, der bei der ursprünglichen Wrapping-Operation zurückgegeben wurde.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Die zusätzlichen Authentifizierungsdaten (AAD), die für den zusätzlichen Schutz eines Schlüssels verwendet wurden. Es können bis zu 255 Zeichenfolgen, jeweils durch Kommas getrennt, angegeben werden. Wenn die AAD beim Wrapping angegeben wurden, müssen dieselben AAD beim Aufheben des Wrappings angegeben werden.</p><p><b>Wichtig:</b> Der {{site.data.keyword.keymanagementserviceshort}}-Service speichert keine zusätzlichen Authentifizierungsdaten. Wenn AAD angegeben werden, speichern Sie die Daten an einer sicheren Position, um sicherzustellen, dass Sie bei nachfolgenden Anforderungen zum Aufheben des Wrappings auf dieselben AAD zugreifen und diese angeben können.</p></dd>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

Es werden die Richtlinien aufgeführt, die dem von Ihnen angegebenen Rootschlüssel zugeordnet sind.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Erforderliche Parameter
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Rootschlüssels, der abgefragt werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #policy-list-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Rufen Sie die Details zu einer Schlüsselrichtlinie ab, wie z. B. das automatische Rotationsintervall des Schlüssels.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Erforderliche Parameter
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Schlüssels, der abgefragt werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #policy-get-opt-params}

<dl>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Erstellen Sie die Richtlinie, die dem von Ihnen angegebenen Rootschlüssel zugeordnet ist, oder ersetzen Sie sie.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Erforderliche Parameter
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>Die ID des Schlüssels, der abgefragt werden soll. Führen Sie den Befehl <a href="#kp-list">kp list</a> aus, um eine Liste der verfügbaren Schlüssel abzurufen.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Geben Sie den Typ der Richtlinie an, den Sie festlegen möchten. Verwenden Sie den Befehl <code>--set-type rotation</code>, um eine Rotationsrichtlinie festzulegen.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>Die {{site.data.keyword.cloud_notm}}-Instanz-ID, die die {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz identifiziert.</dd>
</dl>

### Optionale Parameter
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Geben Sie das Rotationszeitintervall (in Monaten) für einen Schlüssel an. Der Standardwert ist 1.</dd>
    <dt><code>-o, --output</code></dt>
        <dd>Legen Sie das CLI-Ausgabeformat fest. Standardmäßig werden alle Befehle im Tabellenformat ausgegeben. Zum Ändern des Ausgabeformats in JSON verwenden Sie <code>--output json</code>.</dd>
</dl>


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

# Einführung in {{site.data.keyword.keymanagementserviceshort}}

{{site.data.keyword.keymanagementservicefull}} unterstützt Sie bei der Bereitstellung verschlüsselter Schlüssel für die Apps Ihrer {{site.data.keyword.cloud_notm}}-Services. Mit diesem Lernprogramm lernen Sie, wie Sie Verschlüsselungsschlüssel mithilfe des {{site.data.keyword.keymanagementserviceshort}}-Dashboards erstellen und hinzufügen, um die Datenverschlüsselung von einem zentralen Ort aus zu verwalten.
{: shortdesc}

## Einführung in Verschlüsselungsschlüssel
{: #get_started_keys}

Mithilfe des {{site.data.keyword.keymanagementserviceshort}}-Dashboards können Sie neue Schlüssel für die Verschlüsselung erstellen bzw. vorhandene Schlüssel importieren. 

Wählen Sie aus zwei Schlüsseltypen aus:

<dl>
  <dt>Rootschlüssel</dt>
    <dd>Rootschlüssel sind symmetrische Key-Wrapping-Schlüssel, die in {{site.data.keyword.keymanagementserviceshort}} vollständig verwaltet werden. Sie können einen Rootschlüssel verwenden, um andere Verschlüsselungsschlüssel mit einer erweiterten Verschlüsselung zu schützen.</dd>
  <dt>Standardschlüssel</dt>
    <dd>Standardschlüssel sind symmetrische Schlüssel, die in der Kryptografie verwendet werden. Sie können einen Standardschlüssel verwenden, um Daten direkt zu verschlüsseln und zu entschlüsseln.</dd>
</dl>

## Neue Schlüssel erstellen
{: #creating_keys}

[Nach dem Erstellen einer Instanz von {{site.data.keyword.keymanagementserviceshort}}](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps) können Sie im Service Schlüssel angeben. 

Führen Sie die folgenden Schritte aus, um Ihren ersten Verschlüsselungsschlüssel zu erstellen. 

1. Klicken Sie im {{site.data.keyword.keymanagementserviceshort}}-Dashboard auf **Verwalten** &gt; **Schlüssel hinzufügen**.
2. Um einen neuen Schlüssel zu erstellen, wählen Sie das Fenster **Neuen Schlüssel generieren** aus.

    Geben Sie die Schlüsseldetails an:

    <table>
      <tr>
        <th>Einstellung</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Ein eindeutiger, lesbarer Alias zur einfachen Identifikation Ihres Schlüssels.</p>
          <p>Stellen Sie aus Datenschutzgründen sicher, dass der Schlüsselname keine personenbezogenen Daten (PII) wie den Namen oder den Standort enthält.</p>
        </td>
      </tr>
      <tr>
        <td>Schlüsseltyp</td>
        <td>Der [Schlüsseltyp](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types), den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für 'Neuen Schlüssel generieren'</caption>
    </table>

3. Geben Sie die Details zum Schlüssel ein und klicken Sie dann zum Bestätigen auf **Schlüssel generieren**. 

Die im Service generierten Schlüssel sind symmetrische 256-Bit-Schlüssel, die vom AES-GCM-Algorithmus unterstützt werden. Um eine höhere Sicherheit zu erhalten, werden die Schlüssel von FIPS 140-2 Level 2-zertifizierten Hardwaresicherheitsmodulen (HSMs) generiert, die sich in sicheren {{site.data.keyword.cloud_notm}}-Datenzentren befinden. 

## Vorhandene Schlüssel hinzufügen
{: #adding_keys}

Sicherheitsvorteile erhalten Sie auch mit einer BYOK-Unterstützung (Bring Your Own Key), wenn Sie dem Service vorhandene Schlüssel hinzufügen. 

Führen Sie die folgenden Schritte aus, um einen vorhandenen Schlüssel hinzuzufügen.

1. Klicken Sie im {{site.data.keyword.keymanagementserviceshort}}-Dashboard auf **Verwalten** &gt; **Schlüssel hinzufügen**.
2. Um einen vorhandenen Schlüssel hochzuladen, wählen Sie das Fenster **Vorhandenen Schlüssel eingeben** aus.

    Geben Sie die Schlüsseldetails an:

    <table>
      <tr>
        <th>Einstellung</th>
        <th>Beschreibung</th>
      </tr>
      <tr>
        <td>Name</td>
        <td>
          <p>Ein eindeutiger, lesbarer Alias zur einfachen Identifikation Ihres Schlüssels.</p>
          <p>Stellen Sie aus Datenschutzgründen sicher, dass der Schlüsselname keine personenbezogenen Daten (PII) wie den Namen oder den Standort enthält.</p>
        </td>
      </tr>
      <tr>
        <td>Schlüsseltyp</td>
        <td>Der [Schlüsseltyp](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types), den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten.</td>
      </tr>
      <tr>
        <td>Schlüsselinformationen</td>
        <td>Nur erforderlich, wenn Sie einen vorhandenen Schlüssel hinzufügen. Bei den Schlüsselinformationen kann es sich um einen beliebigen Typ von Daten handeln (z. B. symmetrischer Schlüssel), die im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert werden sollen. Der bereitgestellte Schlüssel muss base64-codiert sein.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Einstellungen für 'Vorhandenen Schlüssel eingeben'</caption>
    </table>

3. Geben Sie die Details zum Schlüssel ein und klicken Sie dann zum Bestätigen auf **Neuen Schlüssel hinzufügen**. 

Mit dem {{site.data.keyword.keymanagementserviceshort}}-Dashboard können Sie die allgemeinen Merkmale Ihrer neuen Schlüssel überprüfen. 

## Weitere Schritte

Sie können Ihre Schlüssel nun verwenden, um Ihre Apps und Services zu codieren.

- Weitere Informationen zum programmgesteuerten Management Ihrer Schlüssel [finden Sie in der API-Referenzdokumentation von {{site.data.keyword.keymanagementserviceshort}} für Codebeispiele ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/apidocs/639){: new_window}.
- Zur Anzeige eines Beispiels zur Art und Weise, in der Schlüsselspeicher in {{site.data.keyword.keymanagementserviceshort}} eingesetzt werden können, um Daten zu ver- und entschlüsseln, [überprüfen Sie die Beispielapp in GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
- Weitere Informationen zur Integration des {{site.data.keyword.keymanagementserviceshort}}-Service in andere Clouddaten-Lösungen, finden Sie [in der Integrationsdokumentation](/docs/services/keymgmt/keyprotect_integration.html).

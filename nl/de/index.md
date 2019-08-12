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

# Lernprogramm zur Einführung
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} unterstützt Sie bei der Bereitstellung verschlüsselter Schlüssel für die Apps Ihrer {{site.data.keyword.cloud_notm}}-Services. Mit diesem Lernprogramm lernen Sie, wie Sie Verschlüsselungsschlüssel mithilfe des {{site.data.keyword.keymanagementserviceshort}}-Dashboards erstellen und hinzufügen, um die Datenverschlüsselung von einem zentralen Ort aus zu verwalten.
{: shortdesc}

## Einführung in Verschlüsselungsschlüssel
{: #get-started-keys}

Mithilfe des {{site.data.keyword.keymanagementserviceshort}}-Dashboards können Sie neue Schlüssel für die Verschlüsselung erstellen bzw. vorhandene Schlüssel importieren. 

Wählen Sie aus zwei Schlüsseltypen aus:

<dl>
  <dt>Rootschlüssel</dt>
    <dd>Rootschlüssel sind symmetrische Key-Wrapping-Schlüssel, die in {{site.data.keyword.keymanagementserviceshort}} vollständig verwaltet werden. Sie können einen Rootschlüssel verwenden, um andere Verschlüsselungsschlüssel mit einer erweiterten Verschlüsselung zu schützen. Weitere Informationen finden Sie in <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">Daten mit Envelope-Verschlüsselung schützen</a>.</dd>
  <dt>Standardschlüssel</dt>
    <dd>Standardschlüssel sind symmetrische Schlüssel, die in der Kryptografie verwendet werden. Sie können einen Standardschlüssel verwenden, um Daten direkt zu verschlüsseln und zu entschlüsseln.</dd>
</dl>

## Neue Schlüssel erstellen
{: #create-keys}

[Nach dem Erstellen einer Instanz von {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps) können Sie im Service Schlüssel angeben. 

Führen Sie die folgenden Schritte aus, um Ihren ersten Verschlüsselungsschlüssel zu erstellen. 

1. Klicken Sie auf der Seite mit den Anwendungsdetails auf **Verwalten** &gt; **Schlüssel hinzufügen**.
2. Wenn Sie einen neuen Schlüssel erstellen möchten, wählen Sie das Fenster **Schlüssel erstellen** aus.

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
        <td>Der <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Schlüsseltyp</a>, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Einstellungen für <b>Schlüssel erstellen</b></caption>
    </table>

3. Geben Sie die Details zum Schlüssel ein und klicken Sie anschließend zum Bestätigen auf **Schlüssel erstellen**. 

Die im Service generierten Schlüssel sind symmetrische 256-Bit-Schlüssel, die vom AES-CBC-PAD-Algorithmus unterstützt werden. Um eine höhere Sicherheit zu erhalten, werden die Schlüssel von FIPS 140-2 Level 3-zertifizierten Hardwaresicherheitsmodulen (HSMs) generiert, die sich in sicheren {{site.data.keyword.cloud_notm}}-Rechenzentren befinden. 

## Eigene Schlüssel importieren
{: #import-keys}

Sicherheitsvorteile erhalten Sie auch mit einer BYOK-Unterstützung (Bring Your Own Key), wenn Sie dem Service vorhandene Schlüssel hinzufügen. 

Führen Sie die folgenden Schritte aus, um einen vorhandenen Schlüssel hinzuzufügen.

1. Klicken Sie auf der Seite mit den Anwendungsdetails auf **Verwalten** &gt; **Schlüssel hinzufügen**.
2. Wenn Sie einen vorhandenen Schlüssel hochladen möchten, wählen Sie das Fenster **Eigenen Schlüssel importieren ** aus.

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
        <td>Der <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Schlüsseltyp</a>, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten möchten.</td>
      </tr>
      <tr>
        <td>Schlüsselinformationen</td>
        <td>Die Schlüsselinformationen, z. B. ein symmetrischer Schlüssel, die im {{site.data.keyword.keymanagementserviceshort}}-Service gespeichert werden sollen. Der bereitgestellte Schlüssel muss base64-codiert sein.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der Einstellungen für <b>Eigenen Schlüssel importieren</b></caption>
    </table>

3. Geben Sie die Details zum Schlüssel ein und klicken Sie anschließend zum Bestätigen auf **Schlüssel importieren**. 

Mit dem {{site.data.keyword.keymanagementserviceshort}}-Dashboard können Sie die allgemeinen Merkmale Ihrer neuen Schlüssel überprüfen. 

## Weitere Schritte
{: #get-started-next-steps}

Sie können Ihre Schlüssel nun verwenden, um Ihre Apps und Services zu codieren. Wenn Sie einen Rootschlüssel zum Service hinzugefügt haben, können Sie weitere Informationen zur Verwendung des Rootschlüssels für den Schutz der Schlüssel, mit denen Ihre ruhenden Daten verschlüsselt werden, abrufen. Lesen die zum Einstieg [Wrapping für Schlüssel durchführen](/docs/services/key-protect?topic=key-protect-wrap-keys).

- Weitere Informationen zum Verwalten und Schützen der Verschlüsselungsschlüssel mithilfe eines Rootschlüssels finden Sie in [Daten mit Envelope-Verschlüsselung schützen](/docs/services/key-protect?topic=key-protect-envelope-encryption).
- Weitere Informationen zur Integration des {{site.data.keyword.keymanagementserviceshort}}-Service in andere Clouddaten-Lösungen, finden Sie [in der Integrationsdokumentation](/docs/services/key-protect?topic=key-protect-integrate-services).
- Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln finden Sie in der [{{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation](https://cloud.ibm.com/apidocs/key-protect){: external}.

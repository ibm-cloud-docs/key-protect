---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

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

# Eigene Verschlüsselungsschlüssel in der Cloud verwenden
{: #importing-keys}

Verschlüsselungsschlüssel enthalten Untergruppen von Informationen wie z. B. die Metadaten, mit denen Sie den Schlüssel identifizieren können, sowie die _Schlüsselinformationen_, mit denen Daten ver- und entschlüsselt werden.
{: shortdesc}

Wenn Sie zum Erstellen von Schlüsseln {{site.data.keyword.keymanagementserviceshort}} verwenden, generiert der Service kryptographische Schlüsselinformationen in Ihrem Namen, die ihren Stamm in cloudbasierten Hardwaresicherheitsmodulen (HSMs) haben. Abhängig von Ihren Geschäftsanforderungen jedoch müssen Sie möglicherweise Schlüsselinformationen in Ihrer internen Lösung generieren und anschließend Ihre lokale Schlüsselmanagement-Infrastruktur auf die Cloud erweitern, indem Sie Schlüssel in {{site.data.keyword.keymanagementserviceshort}} importieren.

<table>
  <th>Vorteil</th>
  <th>Beschreibung</th>
  <tr>
    <td>Bring Your Own Keys (BYOK) </td>
    <td>Sie möchten Ihre Schlüsselmanagementverfahren vollständig kontrollieren und stärken, indem Sie starke Schlüssel in Ihrem lokalen Sicherheitsmodul (HSM) generieren. Wenn Sie die Option zum Exportieren von symmetrischen Schlüsseln aus Ihrer internen Schlüsselmanagement-Infrastruktur wählen, können Sie {{site.data.keyword.keymanagementserviceshort}} verwenden, um sie sicher in die Cloud zu bringen.</td>
  </tr>
  <tr>
    <td>Sicherer Import von Rootschlüsselinformationen</td>
    <td>Wenn Sie Ihre Schlüssel in die Cloud exportieren, benötigen Sie die Gewissheit, dass die Schlüsselinformationen während ihrer Ausführung geschützt sind. Sie können das Risiko von Man-in-the-Middle- (MITM-) Angriffen mindern, indem Sie <a href="#transport-keys">einen Transportschlüssel verwenden</a>, um Rootschlüsselinformationen sicher in Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz zu importieren.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Vorteile des Importierens von Schlüsselinformationen</caption>
</table>


## Vorausplanung für das Importieren von Schlüsselinformationen
{: #plan-ahead}

Beachten Sie die folgenden Hinweise, wenn Sie bereit sind, Rootschlüsselinformationen in den Service zu importieren.

<dl>
  <dt>Informieren Sie sich über Ihre Optionen für das Erstellen von Schlüsselinformationen</dt>
    <dd>Erfahren Sie mehr über Ihre Optionen zum Erstellen von symmetrischen 256-Bit-Verschlüsselungsschlüsseln auf Grundlage Ihrer Sicherheitsanforderungen. Sie können beispielsweise Ihr internes Schlüsselmanagementsystem verwenden, das von einem FIPS-validierten lokalen Sicherheitsmodul (HSM) unterstützt wird, um Schlüsselinformationen zu generieren, bevor Sie Schlüssel in die Cloud bringen. Wenn Sie einen Konzeptnachweis schaffen wollen, können Sie auch ein Kryptografie-Toolkit verwenden, z. B. <a href="https://www.openssl.org/" target="_blank">OpenSSL</a>, um Schlüsselinformationen zu generieren, die Sie für Ihre Testanforderungen {{site.data.keyword.keymanagementserviceshort}} importieren können.</dd>
  <dt>Wählen Sie eine Option zum Importieren von Schlüsselinformationen in {{site.data.keyword.keymanagementserviceshort}} aus.</dt>
    <dd>Wählen Sie aus zwei Optionen für den Import von Rootschlüsseln auf der Sicherheitsebene, die für Ihre Umgebung oder Workload erforderlich ist. Standardmäßig verschlüsselt {{site.data.keyword.keymanagementserviceshort}} Ihre Schlüsselinformationen, während diese mit dem TLS-Protokoll (Transport Layer Security) 1.2 übertragen werden. Wenn Sie einen Konzeptnachweis erstellen oder den Service zum ersten Mal testen, können Sie Rootschlüsselinformationen in {{site.data.keyword.keymanagementserviceshort}} importieren, indem Sie diese Standardoption verwenden. Wenn für Ihre Workload ein Sicherheitsmechanismus über TLS hinaus erforderlich ist, können Sie auch <a href="#transport-keys">einen Transportschlüssel verwenden</a>, um Rootschlüsselinformationen zu verschlüsseln und in den Service zu importieren.</dd>
  <dt>Vorausplanung für das Verschlüsseln von Schlüsselinformationen</dt>
    <dd>Wenn Sie Ihr Schlüsselmaterial unter Verwendung eines Transportschlüssels verschlüsseln möchten, müssen Sie eine Methode für die Ausführung der RSA-Verschlüsselung für die Schlüsselinformationen festlegen. Sie müssen das Verschlüsselungsschema <code>RSAES_OAEP_SHA_256</code> verwenden, das durch den <a href="https://tools.ietf.org/html/rfc3447" target="_blank">Standard PKCS #1 V2.1- für RSA-Verschlüsselung</a> angegeben ist. Lesen Sie die Informationen zu den Funktionen Ihres internen Schlüsselmanagementsystems oder lokalen HSM, um Ihre Optionen zu ermitteln.</dd>
  <dt>Lebenszyklus von importierten Schlüsselinformationen verwalten</dt>
    <dd>Nachdem Sie die Schlüsselinformationen in den Service importiert haben, beachten Sie, dass Sie für die Verwaltung des gesamten Lebenszyklus Ihres Schlüssels verantwortlich sind. Bei Verwendung der {{site.data.keyword.keymanagementserviceshort}}-API können Sie ein Ablaufdatum für den Schlüssel festlegen, wenn Sie ihn in den Service hochladen möchten. Wenn Sie jedoch <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">einen importierten Rootschlüssel rotieren</a>, müssen Sie neue Schlüsselinformationen generieren und bereitstellen, um den vorhandenen Schlüssel außer Kraft zu setzen und zu ersetzen. </dd>
</dl>

## Transportschlüssel verwenden
{: #transport-keys}

Transportschlüssel sind derzeit ein Beta-Feature. Beta-Features können jederzeit geändert werden. Mit zukünftigen Aktualisierungen werden möglicherweise Änderungen eingeführt, die mit der neuesten Version nicht kompatibel sind.
{: important}

Wenn Sie Ihre Schlüsselinformationen verschlüsseln möchten, bevor Sie sie in {{site.data.keyword.keymanagementserviceshort}} importieren, können Sie mit der {{site.data.keyword.keymanagementserviceshort}}-API einen Transportverschlüsselungsschlüssel für Ihre Serviceinstanz erstellen. 

Transportschlüssel sind ein Ressourcentyp in {{site.data.keyword.keymanagementserviceshort}}, der den sicheren Import von Rootschlüsselinformationen in Ihre Serviceinstanz ermöglicht. Indem Sie einen Transportschlüssel zum lokalen Verschlüsseln der Schlüsselinformationen verwenden, schützen Sie die Rootschlüssel bei ihrer Ausführung in {{site.data.keyword.keymanagementserviceshort}} auf Grundlage der von Ihnen angegebenen Richtlinien. Sie können beispielsweise eine Richtlinie für den Transportschlüssel festlegen, die die Verwendung des Schlüssels basierend auf der Zeit und der Nutzungszahl begrenzt.

### Funktionsweise
{: #how-transport-keys-work}

Wenn Sie [einen Transportschlüssel](/docs/services/key-protect?topic=key-protect-create-transport-keys) für Ihre Serviceinstanz erstellen, generiert {{site.data.keyword.keymanagementserviceshort}} einen 4096-Bit-RSA-Schlüssel. Der Service verschlüsselt den privaten Schlüssel und gibt dann den öffentlichen Schlüssel und ein Importtoken zurück, die Sie zum Verschlüsseln und Importieren der Rootschlüsselinformationen verwenden können. 

Wenn Sie bereit sind, [einen Rootschlüssel](/docs/services/key-protect?topic=key-protect-import-root-keys#import-root-key-api) in {{site.data.keyword.keymanagementserviceshort}} zu importieren, stellen Sie die verschlüsselten Rootschlüsselinformationen und das Importtoken bereit, die zur Überprüfung der Integrität des öffentlichen Schlüssels verwendet werden. Für die Ausführung der Anforderung verwendet {{site.data.keyword.keymanagementserviceshort}} den privaten Schlüssel, der Ihrer Serviceinstanz zugeordnet ist, um die verschlüsselten Rootschlüsselinformationen zu entschlüsseln. Mit diesem Prozess wird sichergestellt, dass nur der Transportschlüssel, den Sie in {{site.data.keyword.keymanagementserviceshort}} erstellt haben, die Schlüsselinformationen entschlüsseln kann, die Sie in den Service importieren und dort speichern.

Sie können pro Serviceinstanz nur einen Transportschlüssel erstellen. Weitere Informationen zu den Abrufbegrenzungen für Transportschlüssel finden Sie in der [{{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### API-Methoden
{: #secure-import-api-methods}

Im Hintergrund führt die {{site.data.keyword.keymanagementserviceshort}}-API den Transportschlüssel-Erstellungsprozess aus.  

In der folgenden Tabelle sind die API-Methoden aufgelistet, die einen Locker einrichten und Transportschlüssel für Ihre Serviceinstanz erstellen.

<table>
  <tr>
    <th>Methode</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Transportschlüssel erstellen</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Transportschlüssel-Metadaten abrufen</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Transportschlüssel und Importtoken abrufen</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der {{site.data.keyword.keymanagementserviceshort}}-API-Methoden</caption>
</table>

Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln in {{site.data.keyword.keymanagementserviceshort}} finden Sie in der [{{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/key-protect){: new_window}.

## Weitere Schritte

- Weitere Informationen zum Erstellen eines Transportschlüssels für Ihre Serviceinstanz finden Sie unter [Transportschlüssel erstellen](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- Weitere Informationen zum Importieren von Schlüsseln in den Service finden Sie unter [Rootschlüssel importieren](/docs/services/key-protect?topic=key-protect-import-root-keys). 

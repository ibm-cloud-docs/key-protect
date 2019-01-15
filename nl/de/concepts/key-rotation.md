---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Schlüsselrotation
{: #key-rotation}

Die Schlüsselrotation (der turnusmäßige Wechsel von Schlüsseln) findet statt, wenn ein Verschlüsselungsschlüssel außer Kraft gesetzt und durch die Generierung von neuen Verschlüsselungsschlüsselinformationen ersetzt wird. 

Die regelmäßige Schlüsselrotation unterstützt Sie bei der Einhaltung branchenspezifischer Vorgaben und dem Einsatz bewährter Verschlüsselungsverfahren. Die folgenden Tabelle enthält Beschreibungen der wichtigsten Vorteile der Schlüsselrotation: 

<table>
  <th>Vorteil</th>
  <th>Beschreibung</th>
  <tr>
    <td>Verwaltung von Verschlüsselungsperioden für Schlüssel</td>
    <td>Durch die Schlüsselrotation wird die Menge der Informationen begrenzt, die durch einen einzelnen Schlüssel geschützt werden. Durch eine regelmäßige Rotation der Rootschlüssel wird auch die Verschlüsselungsperiode der Schlüssel verkürzt. Je länger die Laufzeit eines Verschlüsselungsschlüssels ist, desto höher ist die Wahrscheinlichkeit eines Sicherheitsverstoßes. </td>
  </tr>
  <tr>
    <td>Schadensbegrenzung bei Vorfällen</td>
    <td>Wenn Ihre Organisation ein Sicherheitsproblem feststellt, können Sie eine sofortige Rotation des Schlüssels durchführen, um Kosten im Zusammenhang mit einem beeinträchtigen Schlüssel zu mindern oder zu reduzieren. </td>
  </tr>

  <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Vorteile der Schlüsselrotation</caption>
</table>

Die Schlüsselrotation wird in der Dokumentation 'NIST Special Publication 800-57, Recommendation for Key Management' erläutert. Weitere Informationen finden Sie in [NIST SP 800-57 Pt. 1 Rev. 4. ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}.
{: tip}

## Funktionsweise der Schlüsselrotation
{: #how-rotation-works}

Verschlüsselungsschlüssel durchlaufen während ihrer Laufzeit verschiedene [Schlüsselstatus](/docs/services/key-protect/concepts/key-states.html). Mit Schlüsseln im Status _Aktiv_ werden Daten verschlüsselt und entschlüsselt. Mit Schlüsseln im Status _Inaktiviert_ können keine Daten verschlüsselt werden, diese Schlüssel bleiben jedoch für die Entschlüsselung verfügbar. Schlüssel im Status _Gelöscht_ können weder für die Verschlüsselung noch für die Entschlüsselung verwendet werden. 

Bei der Schlüsselrotation werden Schlüsselinformationen vom Status _Aktiv_ auf sichere Weise in den Status _Inaktiviert_ versetzt. Zum Ersetzen der außer Kraft gesetzten Schlüsselinformationen werden neue Schlüsselinformationen in den Status _Aktiv_ versetzt und so für Verschlüsselungsoperationen verfügbar. 

In {{site.data.keyword.keymanagementserviceshort}} kann für Rootschlüssel eine bedarfsgesteuerte Rotation durchgeführt werden, ohne dass eine Verfolgung der außer Kraft gesetzten Rootschlüsselinformationen erforderlich ist. Im folgenden Diagramm wird eine kontextbezogene Ansicht der Schlüsselrotationsfunktionalität dargestellt.
![Diagramm mit einer kontextbezogenen Ansicht der Schlüsselrotation](../images/key-rotation_min.svg)

Die Rotation ist nur für Rootschlüssel verfügbar.
{: note}

### Rotation von Rootschlüsseln
{: #rotating-key}

Bei jeder Rotationsanforderung ordnet {{site.data.keyword.keymanagementserviceshort}} dem Rootschlüssel neue Schlüsselinformationen zu.  

![Diagramm mit einer Mikroansicht des Rootschlüsselstacks.](../images/root-key-stack_min.svg)

Nach dem Abschluss einer Rotation werden die neuen Rootschlüsselinformationen für den Schutz zukünftiger Datenverschlüsselungsschlüssel (DEKs) mit [Envelope-Verschlüsselung](/docs/services/key-protect/concepts/envelope-encryption.html) verfügbar. Außer Kraft gesetzte Schlüsselinformationen werden in den Status _Inaktiviert_ versetzt. In diesem Status können sie nur dazu verwendet werden, das Wrapping für ältere DEKs, die noch nicht durch die neuesten Rootschlüsselinformationen geschützt werden, aufzuheben und auf diese DEKs zuzugreifen. Wenn {{site.data.keyword.keymanagementserviceshort}} feststellt, dass außer Kraft gesetzte Rootschlüsselinformationen für das Aufheben des Wrappings eines älteren DEK verwendet werden, verschlüsselt der Service den DEK automatisch erneut und gibt einen Datenverschlüsselungsschlüssel mit Wrapping (Wrapped Data Encryption Key, WDEK) zurück, der auf den neuesten Rootschlüsselinformationen basiert. Speichern Sie den neuen WDEK und verwenden Sie ihn für zukünftige Operationen zum Aufheben des Wrappings. Auf diese Weise schützen Sie Ihre DEKs mit den neuesten Rootschlüsselinformationen. 

Weitere Informationen zur Verwendung der {{site.data.keyword.keymanagementserviceshort}}-API für die Rootschlüsselrotation finden Sie in [Schlüsselrotation](/docs/services/key-protect/rotate-keys.html). 

Für die Schlüsselrotation in {{site.data.keyword.keymanagementserviceshort}} fallen keine zusätzlichen Gebühren an. Sie können das Wrapping für WDEKs mit außer Kraft gesetzten Schlüsselinformationen weiterhin aufheben, ohne dass zusätzliche Kosten anfallen. Weitere Informationen zu den verfügbaren Preisoptionen finden Sie auf der [{{site.data.keyword.keymanagementserviceshort}}-Katalogseite](https://{/catalog/services/key-protect). {: tip}

## Häufigkeit der Schlüsselrotation
{: #rotation-frequency}

Nach der Generierung eines Rootschlüssels in {{site.data.keyword.keymanagementserviceshort}} legen Sie die Rotationshäufigkeit fest. Gründe für die Schlüsselrotation können Mitarbeiterfluktuation, Prozessstörungen oder interne Richtlinien der Organisation hinsichtlich der Gültigkeitsdauer von Schlüsseln sein.  

Führen Sie eine regelmäßige Schlüsselrotation durch, z. B. alle 30 Tage. Dies entspricht den bewährten Verschlüsselungsverfahren. {{site.data.keyword.keymanagementserviceshort}} lässt für jeden Rootschlüssel eine Rotation pro Stunde zu. 

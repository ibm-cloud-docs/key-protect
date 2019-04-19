---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sicherheit und Konformität
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} verfügt über Datensicherheitsstrategien, die Ihre Konformitätsanforderungen erfüllen und sicherstellen, dass Ihre Daten in der Cloud sicher und geschützt bleiben.
{: shortdesc}

## Datensicherheit und Verschlüsselung
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} verwendet [{{site.data.keyword.cloud_notm}}-Hardwaresicherheitsmodule (HSMs) ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/hardware-security-module) zum Generieren von Provider-verwalteten Schlüsselinformationen und Durchführen von Operationen der [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-envelope-encryption). HSMs sind manipulationssichere Hardwareeinheiten, die kryptografische Schlüsselinformationen speichern und verwenden, ohne die Schlüssel außerhalb einer kryptografischen Grenze zugänglich zu machen.

Der Zugriff zum Service erfolgt über HTTPS. Die interne {{site.data.keyword.keymanagementserviceshort}}-Kommunikation verwendet das Transport Layer Security (TLS) 1.2-Protokoll für die Verschlüsselung von Daten bei ihrer Übertragung.

Weitere Informationen darüber, inwiefern {{site.data.keyword.keymanagementserviceshort}} Ihre Konformitätsanforderungen erfüllt, finden Sie unter [Plattform- und Servicekonformität](/docs/overview?topic=overview-security#compliancetable).

## Datenlöschung
{: #data-deletion}

Wenn Sie einen Schlüssel löschen, markiert der Service den Schlüssel als gelöscht und der Schlüssel wird in den Status _Gelöscht_ versetzt. Schlüssel in diesem Status sind nicht mehr wiederherstellbar. Die Cloud-Services, die den Schlüssel verwenden, können dann keine Daten mehr entschlüsseln, die dem Schlüssel zugeordnet sind. Ihre Daten verbleiben in diesen Services in verschlüsselter Form. Die zugehörigen Metadaten für den Schlüssel werden in der {{site.data.keyword.keymanagementserviceshort}}-Datenbank aufbewahrt. 

Das Löschen eines Schlüssels in {{site.data.keyword.keymanagementserviceshort}} ist eine zerstörerische Operation. Beachten Sie, dass die Aktion nach dem Löschen eines Schlüssels nicht mehr rückgängig gemacht werden kann und dass alle Daten, die dem Schlüssel zugeordnet sind, bei der Löschung des Schlüssels umgehend verloren gehen. Bevor Sie einen Schlüssel löschen, überprüfen Sie die Daten, die dem Schlüssel zugeordnet sind, und stellen Sie sicher, dass Sie keinen Zugriff mehr darauf benötigen. Löschen Sie keinen Schlüssel, der aktiv Daten in Ihren Produktionsumgebungen schützt. 

Um festzustellen, welche Daten durch einen Schlüssel geschützt sind, können Sie anzeigen, wie Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz Ihren vorhandenen Cloud-Services zugeordnet ist, indem Sie `ibmcloud iam authorization-policies` ausführen. Weitere Informationen zum Anzeigen von Serviceautorisierungen über die Konsole finden Sie unter [Zugriff zwischen Services erteilen](/docs/iam?topic=iam-serviceauth).
{: note}

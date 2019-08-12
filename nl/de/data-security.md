---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Sicherheit und Konformität
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} verfügt über Datensicherheitsstrategien, die Ihre Konformitätsanforderungen erfüllen und sicherstellen, dass Ihre Daten in der Cloud sicher und geschützt bleiben.
{: shortdesc}

## Sicherheitsbereitschaft
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} gewährleistet die Sicherheitsbereitschaft durch die Einhaltung bewährter {{site.data.keyword.IBM_notm}}-Verfahren für Systeme, Vernetzung und sichere Entwicklung. 

Weitere Informationen zu Sicherheitsmaßnahmen in {{site.data.keyword.cloud_notm}} finden Sie unter [Wie weiß ich, dass meine Daten sicher sind?](/docs/overview?topic=overview-security#security).
{: tip}

### Datenverschlüsselung
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} verwendet [{{site.data.keyword.cloud_notm}}-Hardwaresicherheitsmodule (HSMs)](https://www.ibm.com/cloud/hardware-security-module){: external} zum Generieren von Provider-verwalteten Schlüsselinformationen und Durchführen von Operationen der [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-envelope-encryption). HSMs sind manipulationssichere Hardwareeinheiten, die kryptografische Schlüsselinformationen speichern und verwenden, ohne die Schlüssel außerhalb einer kryptografischen Grenze zugänglich zu machen.

Der Zugriff zum Service erfolgt über HTTPS. Die interne {{site.data.keyword.keymanagementserviceshort}}-Kommunikation verwendet das Transport Layer Security (TLS) 1.2-Protokoll für die Verschlüsselung von Daten bei ihrer Übertragung.

### Datenlöschung
{: #data-deletion}

Wenn Sie einen Schlüssel löschen, markiert der Service den Schlüssel als gelöscht und der Schlüssel wird in den Status _Gelöscht_ versetzt. Schlüssel in diesem Status sind nicht mehr wiederherstellbar. Die Cloud-Services, die den Schlüssel verwenden, können dann keine Daten mehr entschlüsseln, die dem Schlüssel zugeordnet sind. Ihre Daten verbleiben in diesen Services in verschlüsselter Form. Die zugehörigen Metadaten für den Schlüssel werden in der {{site.data.keyword.keymanagementserviceshort}}-Datenbank aufbewahrt. 

Das Löschen eines Schlüssels in {{site.data.keyword.keymanagementserviceshort}} ist eine zerstörerische Operation. Beachten Sie, dass die Aktion nach dem Löschen eines Schlüssels nicht mehr rückgängig gemacht werden kann und dass alle Daten, die dem Schlüssel zugeordnet sind, bei der Löschung des Schlüssels umgehend verloren gehen. Bevor Sie einen Schlüssel löschen, überprüfen Sie die Daten, die dem Schlüssel zugeordnet sind, und stellen Sie sicher, dass Sie keinen Zugriff mehr darauf benötigen. Löschen Sie keinen Schlüssel, der aktiv Daten in Ihren Produktionsumgebungen schützt. 

Um festzustellen, welche Daten durch einen Schlüssel geschützt sind, können Sie anzeigen, wie Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz Ihren vorhandenen Cloud-Services zugeordnet ist, indem Sie `ibmcloud iam authorization-policies` ausführen. Weitere Informationen zum Anzeigen von Serviceautorisierungen über die Konsole finden Sie unter [Zugriff zwischen Services erteilen](/docs/iam?topic=iam-serviceauth).
{: note}

## Konformitätsbereitschaft
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} entspricht den Kontrollmechanismen für globale, branchenspezifische und regionale Konformitätsstandards, einschließlich DSGVO, HIPAA und ISO 27001/27017/27018 und andere. 

Eine vollständige Liste der {{site.data.keyword.cloud_notm}}-Konformitätszertifizierungen finden Sie unter [Konformität in {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### EU-Unterstützung
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} verfügt über zusätzliche Kontrollmechanismen, um Ihre {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Europäischen Union (EU) zu schützen. 

Wenn Sie {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region Frankfurt (Deutschland) verwenden, um personenbezogene Daten für europäische Bürgerinnen und Bürger zu verarbeiten, können Sie die Einstellung 'Unterstützung in der EU' für Ihr {{site.data.keyword.cloud_notm}}-Konto aktivieren. Weitere Informationen finden Sie unter [Einstellung 'Unterstützung in der EU' aktivieren](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) und [Unterstützung für Ressourcen in der Europäischen Union anfordern](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### Datenschutz-Grundverordnung (DSGVO)
{: #gdpr}

Mit der DSGVO soll ein harmonisierter datenschutzrechtlicher Rahmen in der gesamten EU geschaffen werden, der darauf abzielt, den Bürgern die Kontrolle über ihre personenbezogenen Daten zurückzugeben, und gleichzeitig strenge Vorschriften für diejenigen vorsieht, bei denen sich diese Daten befinden und die sie verarbeiten, und zwar überall auf der Welt.

{{site.data.keyword.IBM_notm}} ist bestrebt, unseren Kunden und {{site.data.keyword.IBM_notm}} Business Partnern innovative Datenschutz-, Sicherheits- und Governance-Lösungen bereitzustellen, um sie auf ihrem Weg zur DSGVO-Bereitschaft zu unterstützen.

Um die DSGVO-Konformität für Ihre {{site.data.keyword.keymanagementserviceshort}}-Ressourcen sicherzustellen, [aktivieren Sie die Einstellung 'Unterstützung in der EU'](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) für Ihr {{site.data.keyword.cloud_notm}}-Konto. Weitere Informationen dazu, wie {{site.data.keyword.keymanagementserviceshort}} personenbezogene Daten verarbeitet und schützt, können Sie den folgenden Dokumenten entnehmen.

- [{{site.data.keyword.keymanagementservicefull_notm}}-Datenblatt zu Datenverarbeitung und Datenschutz](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} - Ergänzende Bedingungen zur Auftragsverarbeitung](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### HIPAA-Unterstützung
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} entspricht den Kontrollmechanismen des US-amerikanischen Health Insurance Portability and Accountability Act (HIPAA), um die Absicherung geschützter Diagnoseinformationen sicherzustellen. 

Wenn Sie oder Ihr Unternehmen entsprechend der HIPAA-Definition ein Unternehmen im Gesundheitswesen sind/ist, können Sie die Einstellung für die Unterstützung von HIPAA für Ihr {{site.data.keyword.cloud_notm}}-Konto aktivieren. Weitere Informationen finden Sie unter [Einstellung für HIPAA-Unterstützung aktivieren](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} ist ISO 27001-, 27017-, 27018-zertifiziert. Sie können Konformitätszertifizierungen anzeigen, indem Sie [Kompatibilität in {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external} aufrufen. 

### SOC 2 Typ 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} ist SOC 2 Typ 1-zertifiziert. Informationen zum Anfordern eines {{site.data.keyword.cloud_notm}}-SOC 2-Berichts finden Sie unter [Konformität in {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

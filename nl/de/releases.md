---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# Neuerungen
{: #releases}

Bleiben Sie auf dem Laufenden mit den neuen Features, die für {{site.data.keyword.keymanagementservicefull}} verfügbar sind. 
{: shortdesc}

## Juni 2019
{: #june-2019}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für {{site.data.keyword.at_full_notm}}
{: #added-at-logdna-support}
Neu zum 22.06.2019

Sie können jetzt API-Aufrufe für den {{site.data.keyword.keymanagementserviceshort}}-Service mithilfe von {{site.data.keyword.at_full_notm}} überwachen. 

Weitere Informationen zur Überwachung der {{site.data.keyword.keymanagementserviceshort}}-Aktivität finden Sie unter [Activity Tracker-Ereignisse](/docs/services/key-protect?topic=key-protect-at-events).

## Mai 2019
{: #may-2019}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} aktualisiert HSMs auf FIPS 140-2 Level 3
{: #upgraded-hsms}
Neu zum 22.05.2019

{{site.data.keyword.keymanagementserviceshort}} verwendet jetzt {{site.data.keyword.cloud_notm}} Hardware Security Module 7.0 für kryptografische Speicherung und Operationen. Ihre {{site.data.keyword.keymanagementserviceshort}}-Schlüssel werden für alle Regionen in FIPS 140-2 Level 3-konformer Hardware mit Manipulationsnachweis gespeichert. 

Weitere Informationen zu den Features und Vorteilen von {{site.data.keyword.cloud_notm}} HSM 7.0 finden Sie auf der [Produktseite](https://www.ibm.com/cloud/hardware-security-module){: external}.

### Ende der Unterstützung: Cloud Foundry-basierte {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanzen
{: #legacy-service-eol}
Neu zum 15.05.2019

Der traditionelle {{site.data.keyword.keymanagementserviceshort}}-Service, basierend auf Cloud Foundry, wird seit dem 15. Mai 2019 nicht mehr unterstützt. Cloud Foundry-verwaltete {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanzen werden nicht mehr unterstützt, und Aktualisierungen für den traditionellen Service werden nicht mehr bereitgestellt. Den Kunden wird geraten, IAM-verwaltete {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanzen zu verwenden, um von den neuesten Funktionen für den Service zu profitieren.

Wenn Sie Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz nach dem 15. Dezember 2017 erstellt haben, ist Ihre Serviceinstanz IAM-verwaltet und von dieser Änderung nicht betroffen. Wenden Sie sich bei weiteren Fragen an Terry Mosbaugh unter [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).

Müssen Sie eine {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz aus dem Abschnitt **Cloud Foundry-Services** Ihrer {{site.data.keyword.cloud_notm}}-Ressourcenliste entfernen? Sie können uns im [Support Center](https://{DomainName}/unifiedsupport/cases/add) erreichen, indem Sie eine Anforderung zum Entfernen des Eintrags aus Ihrer Konsolenansicht einreichen.
{: tip}

## März 2019
{: #mar-2019}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für richtlinienbasierte Schlüsselrotation.
{: #added-support-policy-key-rotation}
Neu zum 22.03.2019

Sie können Ihren Rootschlüsseln jetzt mit {{site.data.keyword.keymanagementserviceshort}} eine Rotationsrichtlinie zuordnen.

Weitere Informationen finden Sie in [Rotationsrichtlinie festlegen](/docs/services/key-protect?topic=key-protect-set-rotation-policy). Weitere Informationen zu den Optionen für Ihre Schlüsselrotation in {{site.data.keyword.keymanagementserviceshort}} finden Sie unter [Vergleich Ihrer Optionen für die Schlüsselrotation](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für Transportschlüssel
Neu zum 20.03.2019

Aktivieren Sie den sicheren Import von Verschlüsselungsschlüsseln in die Cloud, indem Sie Transportverschlüsselungsschlüssel für Ihren {{site.data.keyword.keymanagementserviceshort}}-Service erstellen.

Weitere Informationen finden Sie in [Eigene Verschlüsselungsschlüssel in der Cloud verwenden](/docs/services/key-protect?topic=key-protect-importing-keys).

Transportschlüssel sind derzeit ein Beta-Feature. Beta-Features können jederzeit geändert werden. Mit zukünftigen Aktualisierungen werden möglicherweise Änderungen eingeführt, die mit der neuesten Version nicht kompatibel sind.
{: important}

## Februar 2019
{: #feb-2019}

### Geändert: traditionelle {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz
{: #changed-legacy-service-instances}

Neu zum 13.02.2019

{{site.data.keyword.keymanagementserviceshort}}-Serviceinstanzen, die vor dem 15. Dezember 2017 bereitgestellt wurden, werden auf einer traditionellen, auf Cloud Foundry basierenden Infrastruktur ausgeführt. Dieser traditionelle {{site.data.keyword.keymanagementserviceshort}}-Service wird am 15. Mai 2019 stillgelegt.

**Was bedeutet das für Sie?**

Wenn Sie über aktive Produktionsschlüssel in einer älteren {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verfügen, achten Sie darauf, die Schlüssel bis zum 15. Mai 2019 auf eine neue Serviceinstanz zu migrieren, um sich den Zugriff auf die verschlüsselten Daten zu erhalten. Sie können prüfen, ob Sie eine traditionelle Instanz verwenden, indem Sie über die [{{site.data.keyword.cloud_notm}}-Konsole](https://{DomainName}/) zu Ihrer Ressourcenliste navigieren. Wenn Ihre {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz im Abschnitt **Cloud Foundry Services** der {{site.data.keyword.cloud_notm}}-Ressourcenliste aufgeführt ist oder wenn Sie den API-Endpunkt `https://ibm-key-protect.edge.bluemix.net` als Ziel der Operationen für den Service verwenden, verwenden Sie eine traditionelle Instanz von {{site.data.keyword.keymanagementserviceshort}}. Nach dem 15. Mai 2019 ist der traditionelle Endpunkt nicht mehr zugänglich und Sie können den Service nicht mehr als Ziel für Operationen verwenden.

Benötigen Sie Hilfe bei der Migration Ihrer Verschlüsselungsschlüssel in eine neue Serviceinstanz? Ausführliche Informationen zu den nötigen Schritten finden Sie unter [Migrations-Client in GitHub](https://github.com/IBM-Cloud/kms-migration-client){: external}. Bei weiteren Fragen zum Status Ihrer Schlüssel oder zum Migrationsprozess erreichen Sie Terry Mosbaugh unter [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Dezember 2018
{: #dec-2018}

### Geändert: {{site.data.keyword.keymanagementserviceshort}}-API-Endpunkte
{: #changed-api-endpoints}

Neu zum 19.12.2018

Entsprechend der neuen, vereinheitlichten Benutzererfahrung von {{site.data.keyword.cloud_notm}} hat {{site.data.keyword.keymanagementserviceshort}} die Basis-URLs für seine Service-APIs aktualisiert.

Sie können Ihre Anwendungen jetzt so aktualisieren, dass sie auf die neuen `cloud.ibm.com`-Endpunkte verweisen.

- `keyprotect.us-south.bluemix.net` ist jetzt `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` ist jetzt `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` ist jetzt `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` ist jetzt `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` ist jetzt `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` ist jetzt `jp-tok.kms.cloud.ibm.com` 

Beide URLs für jeden regionalen Serviceendpunkt werden zu diesem Zeitpunkt unterstützt. 

## Oktober 2018
{: #oct-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Tokio'
{: #added-tokyo-region}

Neu zum 31.10.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Tokio' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect?topic=key-protect-regions).

### Hinzugefügt: Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}
{: #added-cli-plugin}

Neu zum 02.10.2018

Mithilfe des Plug-ins für die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.keymanagementserviceshort}} können Sie nun Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verwalten.

Informationen zur Installation des Plug-ins finden Sie in [Befehlszeilenschnittstelle einrichten](/docs/services/key-protect?topic=key-protect-set-up-cli). Weitere Informationen zur Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} finden Sie in der [CLI-Referenzdokumentation](/docs/services/key-protect?topic=key-protect-cli-reference).

## September 2018
{: #sept-2018}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für bedarfsgesteuerte Schlüsselrotation.
{: #added-key-rotation}

Neu zum 28.09.2018

{{site.data.keyword.keymanagementserviceshort}} kann nun verwendet werden, um eine bedarfsgesteuerte Rotation von Rootschlüsseln durchzuführen.

Weitere Informationen finden Sie in [Schlüsselrotation](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Hinzugefügt: Lernprogramm zur End-to-End-Sicherheit für die Cloud-App
{: #added-security-tutorial}

Neu zum: 14.09.2018

Suchen Sie nach Codebeispielen, die Sie beim Verschlüsseln von Speicherbucketinhalten mit eigenen Verschlüsselungsschlüsseln unterstützen?

Sie können sich nun mit der Vorgehensweise für das Hinzufügen von End-to-End-Sicherheit zu Ihrer Cloudanwendung vertraut machen, indem Sie das [neue Lernprogramm](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security) ausführen.

Weitere Informationen finden Sie in der [Beispielapp in GitHub](https://github.com/IBM-Cloud/secure-file-storage){: external}.

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Washington DC'
{: #added-wdc-region}

Neu zum: 10.09.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Washington DC' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect?topic=key-protect-regions).

## August 2018
{: #aug-2018}

### Geändert: URL der API-Dokumentation zu {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-doc-url}

Neu zum 28.08.2018

Die API-Referenz zu {{site.data.keyword.keymanagementserviceshort}} ist nun an anderer Stelle verfügbar. 

Zugriff auf die API-Dokumentation besteht nun über
https://{DomainName}/apidocs/key-protect. 

## März 2018
{: #mar-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Frankfurt'
{: #added-frankfurt-region}

Neu zum 21.03.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Frankfurt' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect?topic=key-protect-regions).

## Januar 2018
{: #jan-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Sydney'
{: #added-sydney-region}

Neu zum 21.01.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Sydney' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect?topic=key-protect-regions).

## Dezember 2017
{: #dec-2017}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für BYOK (Bring Your Own Key)
{: #added-byok-support}

Neu zum 15.12.2017

{{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für BYOK (Bring Your Own Key) und eine vom Kunden verwaltete Verschlüsselung.

- [Rootschlüssel](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), auch als CRKs (Customer Root Keys, Benutzerrootschlüssel) bezeichnet, wurden im Service als primäre Ressourcen eingeführt. 
- [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) für {{site.data.keyword.cos_full_notm}}-Buckets wurde ermöglicht.

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'London'
{: #added-london-region}

Neu zum 15.12.2017

{{site.data.keyword.keymanagementserviceshort}} ist nun in der Region 'London' verfügbar. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect?topic=key-protect-regions).

### Geändert: {{site.data.keyword.iamshort}}-Rollen
{: #changed-iam-roles}

Neu zum 15.12.2017

{{site.data.keyword.iamshort}}-Rollen, die die Aktionen bestimmen, die Sie für {{site.data.keyword.keymanagementserviceshort}}-Ressourcen ausführen können, wurden geändert.

- `Administrator` ist nun `Manager`
- `Bearbeiter` ist nun `Schreibberechtigter`
- `Anzeigeberechtigter` ist nun `Leseberechtigter`

Weitere Informationen finden Sie in [Benutzerzugriff verwalten](/docs/services/key-protect?topic=key-protect-manage-access).

## September 2017
{: #sept-2017}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für Cloud IAM
{: #added-iam-support}

Neu zum 19.09.2017

Sie können {{site.data.keyword.iamshort}} nun dazu verwenden, Zugriffsrichtlinien für {{site.data.keyword.keymanagementserviceshort}}-Ressourcen festzulegen und zu verwalten.

Weitere Informationen finden Sie in [Benutzerzugriff verwalten](/docs/services/key-protect?topic=key-protect-manage-access).

---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen
{: #faqs}

Mit den folgenden häufig gestellten Fragen erhalten Sie Unterstützung bei der Arbeit mit {{site.data.keyword.keymanagementservicelong}}.

## Wie ist die Preisstruktur für {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} bietet einen [mehrstufigen Plan](https://{DomainName}/catalog/services/key-protect) an, der für Benutzer, die maximal 20 Schlüssel benötigen, gebührenfrei ist. Pro {{site.data.keyword.cloud_notm}}-Konto können Sie kostenlos 20 Schlüssel erhalten. Wenn für Ihr Team mehrere Instanzen von {{site.data.keyword.keymanagementserviceshort}} erforderlich sind, fügt {{site.data.keyword.cloud_notm}} Ihre aktiven Schlüssel in allen Instanzen innerhalb des Kontos hinzu und wendet dann die Preisstruktur an. 

## Was ist ein aktiver Verschlüsselungsschlüssel?
{: #what-is-active-encryption-key}
{: faq}

Wenn Sie Verschlüsselungsschlüssel in {{site.data.keyword.keymanagementserviceshort}} importieren oder wenn Sie {{site.data.keyword.keymanagementserviceshort}} verwenden, um Schlüssel aus den zugehörigen HSMs zu generieren, werden diese Schlüssel zu _aktiven_ Schlüsseln. Die Preisstruktur basiert auf allen aktiven Schlüsseln innerhalb eines {{site.data.keyword.cloud_notm}}-Kontos. 

## Wie sollte ich meine Schlüssel gruppieren und verwalten?
{: #how-to-group-keys}
{: faq}

In Hinblick auf die Preisstruktur besteht die beste Art der Verwendung von {{site.data.keyword.keymanagementserviceshort}} darin, eine begrenzte Anzahl von Rootschlüsseln zu erstellen und diese dann zum Verschlüsseln der Datenverschlüsselungsschlüssel zu verwenden, die von einer externen App oder vom Clouddatenservice erstellt werden. 

Weitere Informationen zur Verwendung von Rootschlüsseln für den Schutz von Datenverschlüsselungsschlüsseln finden Sie in [Daten mit Envelope-Verschlüsselung schützen](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Was ist ein Rootschlüssel?
{: #what-is-root-key}
{: faq}

Rootschlüssel sind primäre Ressourcen in {{site.data.keyword.keymanagementserviceshort}}. Es sind symmetrische Key-Wrapping-Schlüssel, die als vertrauenswürdige Roots für den Schutz von anderen Schlüsseln verwendet werden, welche in einem Datenservice mit [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-envelope-encryption) gespeichert wurden. Sie können mit {{site.data.keyword.keymanagementserviceshort}} den Lebenszyklus von Rootschlüsseln erstellen, speichern und verwalten, um den uneingeschränkten Zugriff auf andere in der Cloud gespeicherten Schlüssel zu erhalten. 

## Was ist eine Envelope-Verschlüsselung?
{: #what-is-envelope-encryption}
{: faq}

Die Envelope-Verschlüsselung ist das Vorgehen, Daten mit einem _Datenverschlüsselungsschlüssel_ zu verschlüsseln und anschließend den Datenverschlüsselungsschlüssel mit einem hoch sicheren _Key-Wrapping-Schlüssel_ zu verschlüsseln.  Ihre ruhenden Daten werden durch Anwendung mehrerer Verschlüsselungsebenen geschützt. Weitere Informationen zum Aktivieren der Envelope-Verschlüsselung für Ihre {{site.data.keyword.cloud_notm}}-Ressourcen finden Sie unter [Services integrieren](/docs/services/key-protect?topic=key-protect-integrate-services).

## Wie lang darf ein Schlüsselname sein?
{: #key-names}
{: faq}

Es kann ein Schlüsselname mit einer Länge von maximal 90 Zeichen verwendet werden.

## Kann ich personenbezogene Daten als Metadaten für meine Schlüssel speichern?
{: #personal-data}
{: faq}

Zum Schutz der Vertraulichkeit Ihrer personenbezogenen Daten speichern Sie keine persönlichen Daten (personally identifiable information, PII) als Metadaten für Ihre Schlüssel. Zu den personenbezogenen Daten gehören Ihr Name, Ihre Adresse, Telefonnummer, E-Mail-Adresse und andere Informationen, anhand derer Sie, Ihre Kunden oder andere Personen identifiziert, kontaktiert oder lokalisiert werden könnten.

Es liegt in Ihrer Verantwortung, die Sicherheit aller Informationen sicherzustellen, die Sie als Metadaten für {{site.data.keyword.keymanagementserviceshort}}-Ressourcen und -Verschlüsselungsschlüssel speichern. Weitere Beispiele für personenbezogene Daten finden Sie in Abschnitt 2.2 des Dokuments [NIST Special Publication 800-122 ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
{: important}

## Können Schlüssel, die in einer Region erstellt werden, in einer anderen Region verwendet werden?
{: #keys-across-regions}
{: faq}

Ihre Verschlüsselungsschlüssel sind auf die Region beschränkt, in der Sie sie erstellt haben. {{site.data.keyword.keymanagementserviceshort}} kopiert oder exportiert Verschlüsselungsschlüssel nicht in andere Regionen.

## Wie kann ich steuern, wer Zugriff auf die Schlüssel hat?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} unterstützt ein zentralisiertes Zugriffssteuerungssystem, das mit {{site.data.keyword.iamlong}} reguliert wird, um Sie bei der Verwaltung von Benutzern und beim Zugriff auf Ihre Verschlüsselungsschlüssel zu unterstützen. Wenn Sie der Sicherheitsadministrator für Ihren Service sind, dann können Sie [den jeweiligen {{site.data.keyword.keymanagementserviceshort}}-Berechtigungen entsprechende Cloud IAM-Rollen](/docs/services/key-protect?topic=key-protect-manage-access#roles), die Sie Mitgliedern Ihres Teams erteilen wollen, zuweisen.

## Wie kann ich API-Aufrufe an {{site.data.keyword.keymanagementserviceshort}} überwachen?
{: faq}

Mit dem {{site.data.keyword.cloudaccesstrailfull_notm}}-Service können Sie verfolgen, wie Benutzer und Anwendungen mit Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz interagieren. Beispiel: Wenn Sie einen Schlüssel in {{site.data.keyword.keymanagementserviceshort}} erstellen, importieren, löschen oder lesen, wird ein {{site.data.keyword.cloudaccesstrailshort}}-Ereignis generiert. Diese Ereignisse werden automatisch an den {{site.data.keyword.cloudaccesstrailshort}}-Service in derselben Region weitergeleitet, in der auch der {{site.data.keyword.keymanagementserviceshort}}-Service bereitgestellt wird.

Weitere Informationen finden Sie unter [Activity Tracker-Ereignisse](/docs/services/key-protect?topic=key-protect-activity-tracker-events).

## Was passiert, wenn ich einen Schlüssel lösche?
{: #key-destruction}
{: faq}

Wenn Sie einen Schlüssel löschen, markiert der Service den Schlüssel als gelöscht und der Schlüssel wird in den Status _Gelöscht_ versetzt. Schlüssel in diesem Status sind nicht mehr wiederherstellbar. Die Cloud-Services, die den Schlüssel verwenden, können dann keine Daten mehr entschlüsseln, die dem Schlüssel zugeordnet sind. Ihre Daten verbleiben in diesen Services in verschlüsselter Form. Die zugehörigen Metadaten für den Schlüssel werden in der {{site.data.keyword.keymanagementserviceshort}}-Datenbank aufbewahrt. 

Stellen Sie vor dem Löschen eines Schlüssels sicher, dass Sie keinen Zugriff auf Daten mehr benötigen, die dem betreffenden Schlüssel zugeordnet sind. Diese Aktion kann nicht rückgängig gemacht werden.

## Was passiert, wenn ich die Einrichtung meiner Serviceinstanz zurücknehmen muss?
{: #deprovision-service}
{: faq}

Wenn Sie sich entscheiden, von {{site.data.keyword.keymanagementserviceshort}} zu wechseln, müssen Sie alle verbleibenden Schlüssel aus Ihrer Serviceinstanz löschen, bevor Sie die Einrichtung des Service wieder zurücknehmen können. Nachdem Sie Ihre Serviceinstanz gelöscht haben, verwendet {{site.data.keyword.keymanagementserviceshort}} die [Envelope-Verschlüsselung](/docs/services/key-protect?topic=key-protect-envelope-encryption) zum verschlüsselten Schreddern aller Daten, die der Serviceinstanz zugeordnet sind. 

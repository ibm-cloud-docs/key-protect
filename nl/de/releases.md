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

# Neuerungen
{: #releases}

Bleiben Sie auf dem Laufenden mit den neuen Features, die für {{site.data.keyword.keymanagementservicefull}} verfügbar sind. 

## Oktober 2018
{: #oct-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Asien/Pazifik (Norden)'. 
Neu zum 31.10.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Asien/Pazifik (Norden)' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect/regions.html).

### Hinzugefügt: Plug-in der Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}}
Neu zum 02.10.2018

Mithilfe des Plug-ins für die Befehlszeilenschnittstelle (CLI) von {{site.data.keyword.keymanagementserviceshort}} können Sie nun Schlüssel in Ihrer {{site.data.keyword.keymanagementserviceshort}}-Serviceinstanz verwalten. 

Informationen zur Installation des Plug-ins finden Sie in [Befehlszeilenschnittstelle einrichten](/docs/services/key-protect/set-up-cli.html). Weitere Informationen zur Befehlszeilenschnittstelle von {{site.data.keyword.keymanagementserviceshort}} [finden Sie in der Referenzdokumentation zur Befehlszeilenschnittstelle](/docs/services/key-protect/cli-reference.html). 

## September 2018
{: #sept-2018}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für bedarfsgesteuerte Schlüsselrotation. 
Neu zum 28.09.2018

{{site.data.keyword.keymanagementserviceshort}} kann nun verwendet werden, um eine bedarfsgesteuerte Rotation von Rootschlüsseln durchzuführen. 

Weitere Informationen finden Sie in [Schlüsselrotation](/docs/services/key-protect/rotate-keys.html). 

### Hinzugefügt: Lernprogramm zur End-to-End-Sicherheit für die Cloud-App
Neu zum: 14.09.2018

Suchen Sie nach Codebeispielen, die Sie beim Verschlüsseln von Speicherbucketinhalten mit eigenen Verschlüsselungsschlüsseln unterstützen? 

Sie können sich nun mit der Vorgehensweise für das Hinzufügen von End-to-End-Sicherheit zu Ihrer Cloudanwendung vertraut machen, indem Sie das [neue Lernprogramm](/docs/tutorials/cloud-e2e-security.html) ausführen. 

Weitere Informationen finden Sie in der [Beispielapp in GitHub ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}. 

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'USA (Osten)'. 
Neu zum: 10.09.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'USA (Osten)' erstellen.  

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect/regions.html).

## August 2018
{: #aug-2018}

### Geändert: URL der API-Dokumentation zu {{site.data.keyword.keymanagementserviceshort}}
Neu zum 28.08.2018

Die API-Referenz zu {{site.data.keyword.keymanagementserviceshort}} ist nun an anderer Stelle verfügbar.  

Zugriff auf die API-Dokumentation besteht nun über 
https://console.bluemix.net/apidocs/key-protect. 

## März 2018
{: #mar-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Frankfurt'
Neu zum 21.03.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Frankfurt' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect/regions.html).

## Januar 2018
{: #jan-2018}

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'Sydney'
Neu zum 21.01.2018

Sie können nun {{site.data.keyword.keymanagementserviceshort}}-Ressourcen in der Region 'Sydney' erstellen. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect/regions.html).

## Dezember 2017
{: #dec-2017}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für BYOK (Bring Your Own Key)
Neu zum 15.12.2017

{{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für BYOK (Bring Your Own Key) und eine vom Kunden verwaltete Verschlüsselung.

- [Rootschlüssel](/docs/services/key-protect/concepts/envelope-encryption.html#key-types), auch als CRKs (Customer Root Keys, Benutzerrootschlüssel) bezeichnet, wurden im Service als primäre Ressourcen eingeführt. 
- [Envelope-Verschlüsselung](/docs/services/key-protect/integrations/integrate-cos.html#kp-cos-how) für {{site.data.keyword.cos_full_notm}}-Buckets wurde ermöglicht.

### Hinzugefügt: Erweiterung von {{site.data.keyword.keymanagementserviceshort}} auf die Region 'London'
Neu zum 15.12.2017

{{site.data.keyword.keymanagementserviceshort}} ist nun in der Region 'London' verfügbar. 

Weitere Informationen finden Sie in [Regionen und Standorte](/docs/services/key-protect/regions.html).

### Geändert: {{site.data.keyword.iamshort}}-Rollen
Neu zum 15.12.2017

{{site.data.keyword.iamshort}}-Rollen, die die Aktionen bestimmen, die Sie für {{site.data.keyword.keymanagementserviceshort}}-Ressourcen ausführen können, wurden geändert.

- `Administrator` ist nun `Manager`
- `Bearbeiter` ist nun `Schreibberechtigter`
- `Anzeigeberechtigter` ist nun `Leseberechtigter`

Weitere Informationen finden Sie in [Benutzerzugriff verwalten](/docs/services/key-protect/manage-access.html).

## September 2017
{: #sept-2017}

### Hinzugefügt: {{site.data.keyword.keymanagementserviceshort}} bietet nun Unterstützung für Cloud IAM
Neu zum 19.09.2017

Sie können {{site.data.keyword.iamshort}} nun dazu verwenden, Zugriffsrichtlinien für {{site.data.keyword.keymanagementserviceshort}}-Ressourcen festzulegen und zu verwalten.

Weitere Informationen finden Sie in [Benutzerzugriff verwalten](/docs/services/key-protect/manage-access.html).

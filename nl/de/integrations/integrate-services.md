---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Services integrieren
{: #integrate-services}

{{site.data.keyword.keymanagementservicefull}} wird in Daten- und Speicherlösungen integriert, um Sie dabei zu unterstützen, eine eigene Verschlüsselung in der Cloud zu implementieren und zu verwalten.
{: shortdesc}

[Nach der Erstellung einer Instanz des Service](/docs/services/key-protect/provision.html) können Sie {{site.data.keyword.keymanagementserviceshort}} in die folgenden unterstützten Services integrieren:

<table>
    <tr>
        <th>Service</th>
        <th>Beschreibung</th>
    </tr>
    <tr>
        <td>
          <p>{{site.data.keyword.cos_full_notm}}</p>
        </td>
        <td>
          <p>Fügen Sie eine [Envelope-Verschlüsselung](/docs/services/key-protect/concepts/envelope-encryption.html) mithilfe von {{site.data.keyword.keymanagementserviceshort}} zu den Speicherbuckets hinzu. Verwenden Sie Rootschlüssel, die Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten, um die Datenverschlüsselungsschlüssel zu schützen, mit denen die ruhenden Daten verschlüsselt sind.</p>
          <p>Weitere Informationen finden Sie in [Integration mit {{site.data.keyword.cos_full_notm}}](/docs/services/key-protect/integrations/integrate-cos.html).</p>
        </td>
    </tr>
   <caption style="caption-side:bottom;">Tabelle 1. Beschreibung der Integrationen, die für {{site.data.keyword.keymanagementserviceshort}} verfügbar sind</caption>
</table>

## Informationen zur Integration 
{: #understand-integration}

Bei der Integration eines unterstützten Service mit {{site.data.keyword.keymanagementserviceshort}} aktivieren Sie die [Envelope-Verschlüsselung](/docs/services/key-protect/concepts/envelope-encryption.html) für diesen Service. Diese Integration ermöglicht die Verwendung eines Rootschlüssels, den Sie in {{site.data.keyword.keymanagementserviceshort}} speichern, um die Datenverschlüsselungsschlüssel einzuschließen (Key-Wrapping), mit denen die ruhenden Daten verschlüsselt sind. 

Beispiel: Sie können einen Rootschlüssel erstellen, diesen Schlüssel in {{site.data.keyword.keymanagementserviceshort}} verwalten und den Rootschlüssel dazu verwenden, die Daten zu schützen, die in verschiedenen Cloud-Services gespeichert sind.

![Diagramm mit einer Kontextansicht der {{site.data.keyword.keymanagementserviceshort}}-Integration](../images/kp-integrations_min.svg)

### {{site.data.keyword.keymanagementserviceshort}}-API-Methoden
{: #api-methods}

Im Hintergrund führt die {{site.data.keyword.keymanagementserviceshort}}-API den Envelope-Verschlüsselungsprozess aus.  

In der folgenden Tabelle sind die API-Methoden aufgeführt, mit denen die Envelope-Verschlüsselung für eine Ressource hinzugefügt oder entfernt wird.

<table>
  <tr>
    <th>Methode</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=wrap</code></td>
    <td><a href="/docs/services/key-protect/wrap-keys.html">Einschließen (Verschlüsseln) eines Datenverschlüsselungsschlüssels</a></td>
  </tr>
  <tr>
    <td><code>POST /keys/{root_key_ID}?action=unwrap</code></td>
    <td><a href="/docs/services/key-protect/unwrap-keys.html">Ausschließen (Entschlüsseln) eines Datenverschlüsselungsschlüssels</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 2. Beschreibung der {{site.data.keyword.keymanagementserviceshort}}-API-Methoden</caption>
</table>

Weitere Informationen zur programmgesteuerten Verwaltung von Schlüsseln in {{site.data.keyword.keymanagementserviceshort}} finden Sie in der [{{site.data.keyword.keymanagementserviceshort}}-API-Referenzdokumentation ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/apidocs/kms){: new_window}.
{: tip}

## Unterstützten Service integrieren
{: #grant-access}

Wenn Sie eine Integration hinzufügen möchten, richten Sie über das {{site.data.keyword.iamlong}}-Dashboard eine Berechtigung zwischen den Services ein. Mit den Berechtigungen werden Service-zu-Service-Zugriffsrichtlinien aktiviert, die es Ihnen ermöglichen, eine Ressource im Clouddatenservice einem [Rootschlüssel](/docs/services/key-protect/concepts/envelope-encryption.html#key-types) zuzuordnen, den Sie in {{site.data.keyword.keymanagementserviceshort}} verwalten.

Stellen Sie sicher, dass beide Services in derselben Region bereitgestellt werden, bevor Sie eine Berechtigung erstellen. Weitere Informationen zu Serviceberechtigungen finden Sie in [Zugriff zwischen Services erteilen![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam/authorizations.html){: new_window}.
{: tip}

Wenn Sie bereit sind, einen Service zu integrieren, führen Sie die folgenden Schritte aus, um eine Berechtigung zu erstellen:

1. [Melden Sie sich bei der {{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.bluemix.net/){: new_window} an.
2. Klicken Sie in der Menüleiste auf **Verwalten** &gt; **Sicherheit** &gt; **Identität und Zugriff**. Wählen Sie dann **Berechtigungen** aus. 
3. Klicken Sie auf **Erstellen**.
4. Wählen Sie eine Quelle und ein Ziel für die Berechtigung aus.
 
  - Wählen Sie für **Quellenservice** den Clouddatenservice aus, der mit {{site.data.keyword.keymanagementserviceshort}} integriert werden soll. Zum Beispiel **Cloud Object Storage**.
  - Wählen Sie für **Zielservice** den Eintrag **{{site.data.keyword.keymanagementservicelong_notm}}** aus. 
4. Um einen Lesezugriff zwischen den Services zu erteilen, aktivieren Sie das Kontrollkästchen **Leseberechtigter** aus.

    Mit den Berechtigungen eines _Leseberechtigten_ kann der Quellenservice die Rootschlüssel anzeigen, die in der angegebenen Instanz von {{site.data.keyword.keymanagementserviceshort}} bereitgestellt sind.
5. Klicken Sie auf **Berechtigen**.

### Weitere Schritte

Fügen Sie erweiterte Verschlüsselung zu Ihren Cloudressourcen hinzu, indem Sie einen Rootschlüssel in {{site.data.keyword.keymanagementserviceshort}} erstellen. Fügen Sie eine neue Ressource zu einem unterstützten Clouddatenservice hinzu und wählen Sie dann den Rootschlüssel aus, der für die erweitertet Verschlüsselung verwendet werden soll.

- Weitere Informationen zum Erstellen von Rootschlüsseln mit dem {{site.data.keyword.keymanagementserviceshort}}-Service finden Sie in [Rootschlüssel erstellen](/docs/services/key-protect/create-root-keys.html).
- Weitere Informationen zur Verwendung eigener Rootschlüssel im {{site.data.keyword.keymanagementserviceshort}}-Service finden Sie in [Rootschlüssel importieren](/docs/services/key-protect/import-root-keys.html).



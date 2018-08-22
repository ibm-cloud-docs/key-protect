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

# Benutzerzugriff verwalten
{: #managing-access-iam}

{{site.data.keyword.keymanagementservicefull}} unterstützt ein zentralisiertes Zugriffssteuerungssystem, das mit {{site.data.keyword.iamlong}} reguliert wird, um Sie bei der Verwaltung von Benutzern und beim Zugriff auf Ihre Verschlüsselungsschlüssel zu unterstützen.
{: shortdesc}

Es hat sich bewährt, Zugriffsberechtigungen zu erteilen, während Sie neue Benutzer zur Nutzung Ihres Kontos oder Service einladen. Beachten Sie hierbei z. B. die folgenden Leitlinien:

- **Aktivieren Sie den Benutzerzugriff auf die Ressourcen in Ihrem Konto, indem Sie Cloud IAM-Rollen zuweisen.**
    Anstatt Ihre Administratorberechtigungsnachweise gemeinsam zu nutzen, sollten Sie neue Richtlinien für die Benutzer erstellen, die Zugriff auf die Verschlüsselungsschlüssel in Ihrem Konto benötigen. Wenn Sie der Administrator Ihres Kontos sind, dann wird Ihnen automatisch eine _Manager_-Richtlinie mit Zugriff auf alle Ressourcen zugewiesen, die sich unter Ihrem Konto befinden.
- **Erteilen Sie Rollen und Berechtigungen für den kleinsten erforderlichen Geltungsbereich.**
    Wenn ein Benutzer beispielsweise nur Zugriff auf eine übergeordnete Ansicht der Schlüssel in einem bestimmten Bereich benötigt, dann ordnen Sie ihm die Rolle _Leseberechtigter_ für diesen Bereich zu.
- **Überprüfen Sie in regelmäßigen Zeitabständen, wer zur Verwaltung der Zugriffssteuerung und zum Löschen von Schlüsselressourcen berechtigt ist.**
    Beachten Sie dabei, dass die Erteilung einer Rolle als _Manager_ für einen Benutzer bedeutet, dass dieser Benutzer Servicerichtlinien für andere Benutzer ändern und außerdem Ressourcen löschen kann.

## Rollen und Berechtigungen
{: #roles}

Mit {{site.data.keyword.iamshort}} (IAM) können Sie den Zugriff für Benutzer und Ressourcen in Ihrem Konto verwalten und definieren.

Zur Vereinfachung des Zugriffs orientiert sich {{site.data.keyword.keymanagementserviceshort}} an den Cloud IAM-Rollen, sodass jeder Benutzer über eine andere Ansicht des Service verfügt, und zwar abhängig von der Rolle, die diesem Benutzer zugewiesen wurde. Wenn Sie der Sicherheitsadministrator für Ihren Service sind, dann können Sie Cloud IAM-Rollen zuweisen, die den speziellen {{site.data.keyword.keymanagementserviceshort}}-Berechtigungen entsprechen, die Sie Mitgliedern Ihres Teams erteilen wollen.

In der folgenden Tabelle erhalten Sie Informationen dazu, wie die Identitäts- und Zugriffsrollen den {{site.data.keyword.keymanagementserviceshort}}-Berechtigungen zugeordnet werden:
<table>
  <tr>
    <th>Servicezugriffsrolle</th>
    <th>Beschreibung</th>
    <th>Aktionen</th>
  </tr>
  <tr>
    <td><p>Leseberechtigter</p></td>
    <td><p>Ein Leseberechtigter kann die übergeordnete Ansicht von Schlüsseln durchsuchen und Wrapping- und Unwrapping-Aktionen durchführen. Leseberechtigte können nicht auf Schlüsselinformationen zugreifen und nicht bearbeiten.</p></td>
    <td>
      <p>
        <ul>
          <li>Schlüssel anzeigen</li>
          <li>Schlüssel einschließen</li>
          <li>Schlüssel ausschließen</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Schreibberechtigter</p></td>
    <td><p>Ein Schreibberechtigter kann Schlüssel erstellen und bearbeiten und auf die Schlüsselinformationen zugreifen.</p></td>
    <td>
      <p>
        <ul>
          <li>Schlüssel erstellen</li>
          <li>Schlüssel anzeigen</li>
          <li>Schlüssel einschließen</li>
          <li>Schlüssel ausschließen</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Manager</p></td>
    <td><p>Ein Manager kann alle Aktionen ausführen, die ein Lese- und ein Schreibberechtigter ausführen können. Dies umfasst auch die Berechtigung zum Löschen von Schlüsseln, zum Einladen neuer Benutzer und zum Zuordnen von Zugriffsrichtlinien für andere Benutzer.</p></td>
    <td>
      <p>
        <ul>
          <li>Alle Aktionen, die ein Lese- oder ein Schreibberechtigter ausführen kann</li>
          <li>Schlüssel löschen</li>
          <li>Zugriffsrichtlinie zuweisen</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 1. Informationen zur Zuordnung von Identitäts- und Zugriffsrollen zu {{site.data.keyword.keymanagementserviceshort}}-Berechtigungen</caption>
</table>

**Hinweis**: Cloud IAM-Benutzerrollen bieten Zugriff auf Service- und auf Serviceinstanzebene. [Cloud Foundry-Rollen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam/cfaccess.html){: new_window} sind separat und definieren den Zugriff auf Organisations- oder Bereichsebene. 

Weitere Informationen zu {{site.data.keyword.iamshort}} finden Sie in [Benutzerrollen und -berechtigungen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam/users_roles.html#userroles){: new_window}.

### Weitere Schritte

Kontoeigner und Administratoren können Benutzer einladen und Servicerichtlinien festlegen, die den {{site.data.keyword.keymanagementserviceshort}}-Aktionen entsprechen, die die Benutzer ausführen können.

- Weitere Informationen zur Zuweisung von Benutzerrollen in der {{site.data.keyword.cloud_notm}}-Benutzerschnittstelle finden Sie in [IAM-Zugriff verwalten ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/iam/mngiam.html){: new_window}.
- Informationen zum Erteilen erweiterter Berechtigungen für den Zugriff auf bestimmte Verschlüsselungsschlüssel finden Sie in [Zugriff auf Schlüssel verwalten](/docs/services/keymgmt/keyprotect_manage_access_api.html).

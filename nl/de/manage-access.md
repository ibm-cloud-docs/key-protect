---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: user permissions, manage access, IAM roles

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

# Benutzerzugriff verwalten
{: #manage-access}

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
  <col width="20%">
  <col width="40%">
  <col width="40%">
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
          <li>Wrapping für Schlüssel durchführen</li>
          <li>Wrapping für Schlüssel aufheben</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Schreibberechtigter</p></td>
    <td><p>Ein Schreibberechtigter kann Schlüssel erstellen, bearbeiten, turnusmäßig wechseln und auf die Schlüsselinformationen zugreifen.</p></td>
    <td>
      <p>
        <ul>
          <li>Schlüssel erstellen</li>
          <li>Schlüssel anzeigen</li>
          <li>Schlüssel turnusmäßig wechseln</li>
          <li>Wrapping für Schlüssel durchführen</li>
          <li>Wrapping für Schlüssel aufheben</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Manager</p></td>
    <td><p>Ein Manager kann alle Aktionen ausführen, die ein Lese- oder ein Schreibberechtigter ausführen kann. Dies umfasst auch die Berechtigung zum Festlegen von Rotationsrichtlinien für Schlüssel, Löschen von Schlüsseln, zum Einladen neuer Benutzer und zum Zuordnen von Zugriffsrichtlinien für andere Benutzer.</p></td>
    <td>
      <p>
        <ul>
          <li>Alle Aktionen, die ein Lese- oder ein Schreibberechtigter ausführen kann</li>
          <li>Benutzerzugriffsrichtlinien zuweisen</li>
          <li>Richtlinien für die Schlüsselrotation festlegen</li>
          <li>Schlüssel löschen</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Tabelle 1. Informationen zur Zuordnung von Identitäts- und Zugriffsrollen zu {{site.data.keyword.keymanagementserviceshort}}-Berechtigungen</caption>
</table>

Cloud-IAM-Benutzerrollen bieten Zugriff auf Service- oder Serviceinstanzebene. [Cloud Foundry-Rollen](/docs/iam?topic=iam-cfaccess){: external} sind separat und definieren den Zugriff auf Organisations- oder Bereichsebene. Weitere Informationen zu {{site.data.keyword.iamshort}} finden Sie unter [Benutzerrollen und -berechtigungen](/docs/iam?topic=iam-userroles){: external}.
{: note}

## Weitere Schritte
{: #manage-access-next-steps}

Kontoeigner und Administratoren können Benutzer einladen und Servicerichtlinien festlegen, die den {{site.data.keyword.keymanagementserviceshort}}-Aktionen entsprechen, die die Benutzer ausführen können.

- Weitere Informationen zur Zuweisung von Benutzerrollen in der {{site.data.keyword.cloud_notm}}-Benutzerschnittstelle finden Sie unter [IAM-Zugriff verwalten](/docs/iam?topic=iam-getstarted){: external}.


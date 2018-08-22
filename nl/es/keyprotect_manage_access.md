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

# Gestión de acceso de usuario
{: #managing-access-iam}

{{site.data.keyword.keymanagementservicefull}} da soporte a un sistema de control de acceso centralizado, gobernado por {{site.data.keyword.iamlong}}, para ayudarle a gestionar usuarios y acceder a sus claves de cifrado.
{: shortdesc}

Una buena práctica es garantizar los permisos de acceso, cuando invita a nuevos usuarios a su cuenta o servicio. Por ejemplo, tenga en cuenta las directrices siguientes:

- **Habilitar el acceso de usuario a los recursos de la cuenta asignando roles de Cloud IAM.**
    En lugar de compartir las credenciales admin, crear nuevas políticas para los usuarios que necesitan acceso a las claves de cifrado en su cuenta. Si usted es el administrador de la cuenta, se le asigna automáticamente una política de _Gestor_ con acceso a todos los recursos de la cuenta.
- **Conceder roles y permisos en el ámbito más pequeño necesario.**
    Por ejemplo, si un usuario necesita acceder solo a una vista de alto nivel de claves dentro de un espacio especificado, otorgue el rol _Lector_ al usuario para ese espacio.
- **Auditar de forma regular a quien puede gestionar el control de acceso y suprimir los recursos de clave.**
    Recuerde que otorgando un rol de _Gestor_ a un usuario significa que el usuario puede modificar políticas de servicio para otros usuarios, además de destruir recursos.

## Roles y permisos
{: #roles}

Con {{site.data.keyword.iamshort}} (IAM), podrá gestionar y definir el acceso para usuarios y recursos en su cuenta.

Para simplificar el acceso, {{site.data.keyword.keymanagementserviceshort}} se alinea con los roles Cloud IAM para que cada usuario tenga una vista distinta del servicio, según el rol que se haya asignado al usuario. Si es un administrador de seguridad para su servicio, puede asignar roles de Cloud IAM que correspondan a los permisos {{site.data.keyword.keymanagementserviceshort}} específicos que desea garantizar a los miembros de su equipo.

La siguiente tabla muestra cómo los roles identidad y acceso se correlacionan con los permisos de {{site.data.keyword.keymanagementserviceshort}}:
<table>
  <tr>
    <th>Rol de acceso de servicio</th>
    <th>Descripción</th>
    <th>Acciones</th>
  </tr>
  <tr>
    <td><p>Lector</p></td>
    <td><p>Un lector puede examinar una vista de alto nivel de claves y realizar acciones envolver y desenvolver. Los lectores no pueden acceder ni modificar el material de las claves.</p></td>
    <td>
      <p>
        <ul>
          <li>Ver claves</li>
          <li>Envolver claves</li>
          <li>Desenvolver claves</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Escritor</p></td>
    <td><p>Un escritor puede crear claves, modificar claves y acceder al material de las claves.</p></td>
    <td>
      <p>
        <ul>
          <li>Crear claves</li>
          <li>Ver claves</li>
          <li>Envolver claves</li>
          <li>Desenvolver claves</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td><p>Gestor</p></td>
    <td><p>Un gestor puede realizar todas las acciones que un lector y un escritor pueden realizar, incluida la capacidad de suprimir claves, invitar a nuevos usuarios y asignar políticas de acceso para otros usuarios.</p></td>
    <td>
      <p>
        <ul>
          <li>Todas las acciones que un lector o un escritor puedes realizar</li>
          <li>Suprimir claves</li>
          <li>Asignar políticas de acceso</li>
        </ul>
      </p>
    </td>
  </tr>
  <caption style="caption-side:bottom;">Tabla 1. Describe cómo los roles de acceso e identidad se correlacionan con los permisos de {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

**Nota**: Los roles de usuario de Cloud IAM proporcionan acceso al nivel de servicio o instancia de servicio. Los [roles de Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/iam/cfaccess.html){: new_window} son independientes y definen el acceso en la organización o el nivel de espacio.

Para obtener más información sobre {{site.data.keyword.iamshort}}, consulte [Roles de usuario y permisos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/iam/users_roles.html#userroles){: new_window}.

### Qué hacer a continuación

Los propietarios y administradores de cuentas pueden invitar a usuarios y establecer políticas de servicio que corresponden a los usuarios pueden realizar acciones de {{site.data.keyword.keymanagementserviceshort}}.

- Para obtener más información sobre la asignación de roles de usuario en la IU de {{site.data.keyword.cloud_notm}}, consulte [Gestión de acceso de IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/iam/mngiam.html){: new_window}.
- Para obtener información sobre la concesión de permisos avanzados para acceder a claves de cifrado específicas, consulte [Gestión de claves de acceso](/docs/services/keymgmt/keyprotect_manage_access_api.html).

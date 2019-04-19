---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: release notes, changelog, what's new, service updates, service bulletin

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

# Novedades
{: #releases}

Consulte información actualizada sobre las nuevas características disponibles para {{site.data.keyword.keymanagementservicefull}}. 

## Marzo de 2019
{: #mar-2019}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} añade soporte para la rotación de claves basada en políticas
{: #added-support-policy-key-rotation}
Novedad desde: 22-03-2019

Ahora puede utilizar {{site.data.keyword.keymanagementserviceshort}} para asociar una política de rotación para las claves raíz.

Para obtener más información, consulte [Establecimiento de una política de rotación](/docs/services/key-protect?topic=key-protect-set-rotation-policy). Para obtener más información acerca de las opciones de rotación claves en {{site.data.keyword.keymanagementserviceshort}}, consulte [Comparación de las opciones de rotación de claves](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options).

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} añade soporte beta para las claves de transporte
Novedad desde: 20-03-2019

Habilite la importación segura de claves de cifrado en la nube creando claves de cifrado de transporte para el servicio {{site.data.keyword.keymanagementserviceshort}}.

Para obtener más información, consulte [Cómo traer las claves de cifrado a la nube](/docs/services/key-protect?topic=key-protect-importing-keys).

Actualmente, las claves de transporte son una característica beta. Las características beta pueden cambiar en cualquier momento, y las actualizaciones futuras pueden introducir cambios incompatibles con la última versión.
{: important}

## Febrero de 2019
{: #feb-2019}

### Se ha modificado: instancias de servicio de {{site.data.keyword.keymanagementserviceshort}} heredadas
{: #changed-legacy-service-instances}

Novedad desde: 13-02-2019

Las instancias de servicio de {{site.data.keyword.keymanagementserviceshort}} suministradas antes del 15 de diciembre de 2017 se ejecutan en una infraestructura heredada basada en Cloud Foundry. Este servicio de {{site.data.keyword.keymanagementserviceshort}} heredado estará fuera de servicio el 15 de mayo de 2019.

**Qué significa esto para usted**

Si tiene claves de producción activas en una instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} más antigua, asegúrese de migrar las claves a una instancia de servicio nueva para el 15 de mayo de 2019 para evitar perder el acceso a los datos cifrados. Puede comprobar si está utilizando una instancia heredada navegando hasta la lista de recursos desde la [consola de {{site.data.keyword.cloud_notm}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/). Si la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} se lista en la sección **Servicios de Cloud Foundry** de la lista de recursos de {{site.data.keyword.cloud_notm}}, o si utiliza el punto final de la API `https://ibm-key-protect.edge.bluemix.net` como destino para las operaciones del servicio, esto significa que está utilizando una instancia heredada de {{site.data.keyword.keymanagementserviceshort}}. A partir del 15 de mayo de 2019, el punto final heredado ya no será accesible y no podrá utilizar el servicio como destino para las operaciones.

¿Necesita ayuda para migrar las claves de cifrado a una instancia de servicio nueva? Para ver los pasos detallados, consulte el [cliente de migración en GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM-Cloud/kms-migration-client){: new_window}. Si tiene preguntas adicionales sobre el estado de sus claves o el proceso de migración, póngase en contacto con Terry Mosbaugh en [mosbaugh@us.ibm.com](mailto:mosbaugh@us.ibm.com).
{: tip}

## Diciembre de 2018
{: #dec-2018}

### Se ha modificado: puntos finales de la API de {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-endpoints}

Novedad desde: 19-12-2018

Para adaptarse a la nueva experiencia unificada de {{site.data.keyword.cloud_notm}}, {{site.data.keyword.keymanagementserviceshort}} ha actualizado los URL base de sus API de servicio.

Ahora puede actualizar sus aplicaciones para que hagan referencia a los nuevos puntos finales de `cloud.ibm.com`.

- `keyprotect.us-south.bluemix.net` es ahora `us-south.kms.cloud.ibm.com` 
- `keyprotect.us-east.bluemix.net` es ahora `us-east.kms.cloud.ibm.com` 
- `keyprotect.eu-gb.bluemix.net` es ahora `eu-gb.kms.cloud.ibm.com` 
- `keyprotect.eu-de.bluemix.net` es ahora `eu-de.kms.cloud.ibm.com` 
- `keyprotect.au-syd.bluemix.net` es ahora `au-syd.kms.cloud.ibm.com` 
- `keyprotect.jp-tok.bluemix.net` es ahora `jp-tok.kms.cloud.ibm.com` 

En este momento se admiten los dos URL de cada uno de los puntos finales de servicio regional. 

## Octubre de 2018
{: #oct-2018}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} se expande a la región de Tokyo
{: #added-tokyo-region}

Novedad desde: 31-10-2018

Ahora puede crear recursos de {{site.data.keyword.keymanagementserviceshort}} en la región de Tokyo. 

Para obtener más información, consulte [Regiones y ubicaciones](/docs/services/key-protect?topic=key-protect-regions).

### Se ha añadido: Plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}
{: #added-cli-plugin}

Novedad desde: 02-10-2018

Ahora puede utilizar el plugin de CLI de {{site.data.keyword.keymanagementserviceshort}} para gestionar claves en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

Para aprender a instalar el plugin, consulte [Configuración de la CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). Para obtener más información sobre la CLI de {{site.data.keyword.keymanagementserviceshort}}, [consulte el documento de consulta de la CLI](/docs/services/key-protect?topic=key-protect-cli-reference).

## Septiembre de 2018
{: #sept-2018}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} añade soporte para la rotación de claves bajo demanda
{: #added-key-rotation}

Novedad desde: 28-09-2018

Ahora puede utilizar {{site.data.keyword.keymanagementserviceshort}} para rotar sus claves raíz bajo demanda.

Para obtener más información, consulte [Rotación de claves](/docs/services/key-protect?topic=key-protect-rotate-keys).

### Se ha añadido: Guía de aprendizaje completa para su app de nube
{: #added-security-tutorial}

Novedad desde: 14-09-2018

¿Está buscando ejemplos de código que le ayuden a cifrar contenido de depósito de almacenamiento con sus propias claves de cifrado?

Ahora puede practicar añadiendo seguridad global para su aplicación de nube siguiendo [la nueva guía de aprendizaje](/docs/tutorials?topic=solution-tutorials-cloud-e2e-security).

Para obtener más información, [consulte la app de ejemplo en GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM-Cloud/secure-file-storage){: new_window}.

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} se expande a la región de Washington DC
{: #added-wdc-region}

Novedad desde: 10-09-2018

Ahora puede crear recursos de {{site.data.keyword.keymanagementserviceshort}} en la región de Washington DC. 

Para obtener más información, consulte [Regiones y ubicaciones](/docs/services/key-protect?topic=key-protect-regions).

## Agosto de 2018
{: #aug-2018}

### Se ha modificado: URL de documentación de la API de {{site.data.keyword.keymanagementserviceshort}}
{: #changed-api-doc-url}

Novedad desde: 28-08-2018

La consulta de la API de {{site.data.keyword.keymanagementserviceshort}} se ha trasladado. 

Ahora puede acceder a la documentación de la API en
https://{DomainName}/apidocs/key-protect. 

## Marzo de 2018
{: #mar-2018}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} se expande a la región de Frankfurt
{: #added-frankfurt-region}

Novedad desde: 21-03-2018

Ahora puede crear recursos de {{site.data.keyword.keymanagementserviceshort}} en la región de Frankfurt. 

Para obtener más información, consulte [Regiones y ubicaciones](/docs/services/key-protect?topic=key-protect-regions).

## Enero de 2018
{: #jan-2018}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} se expande a la región de Sídney
{: #added-sydney-region}

Novedad desde: 31-01-2018

Ahora puede crear recursos de {{site.data.keyword.keymanagementserviceshort}} en la región de Sídney. 

Para obtener más información, consulte [Regiones y ubicaciones](/docs/services/key-protect?topic=key-protect-regions).

## Diciembre de 2017
{: #dec-2017}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} añade soporte para Bring Your Own Key (BYOK)
{: #added-byok-support}

Novedad desde: 15-12-2017

{{site.data.keyword.keymanagementserviceshort}} ahora da soporte a Bring Your Own Key (BYOK) y al cifrado gestionado por el cliente.

- Se han introducido [claves raíz](/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types), también denominadas Customer Root Keys (CRK), como recursos primarios en el servicio. 
- Se ha habilitado el [cifrado de sobre](/docs/services/key-protect?topic=key-protect-integrate-cos#kp-cos-how) para depósitos de {{site.data.keyword.cos_full_notm}}.

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} se expande a la región de Londres
{: #added-london-region}

Novedad desde: 15-12-2017

{{site.data.keyword.keymanagementserviceshort}} ahora está disponible en la región de Londres. 

Para obtener más información, consulte [Regiones y ubicaciones](/docs/services/key-protect?topic=key-protect-regions).

### Se ha modificado: Roles de {{site.data.keyword.iamshort}}
{: #changed-iam-roles}

Novedad desde: 15-12-2017

Los roles de {{site.data.keyword.iamshort}}, que determinan las acciones que puede realizar en los recursos de {{site.data.keyword.keymanagementserviceshort}}, se han modificado.

- `Administrador` es ahora `Gestor`
- `Editor` es ahora `Escritor`
- `Visor` es ahora `Lector`

Para obtener más información, consulte [Gestión de acceso de usuario](/docs/services/key-protect?topic=key-protect-manage-access).

## Septiembre de 2017
{: #sept-2017}

### Se ha añadido: {{site.data.keyword.keymanagementserviceshort}} añade soporte para Cloud IAM
{: #added-iam-support}

Novedad desde: 19-09-2017

Ahora puede utilizar {{site.data.keyword.iamshort}} para establecer y gestionar políticas de acceso para los recursos de {{site.data.keyword.keymanagementserviceshort}}.

Para obtener más información, consulte [Gestión de acceso de usuario](/docs/services/key-protect?topic=key-protect-manage-access).

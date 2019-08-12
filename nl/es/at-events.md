---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: IBM, activity tracker, LogDNA, event, security, KMS API calls, monitor KMS events

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

<!-- Include your AT events file in the Reference nav group in your toc file. -->

<!-- Make sure that the AT events file has the H1 ID set to: {: #at_events} -->

# Sucesos de Activity Tracker
{: #at-events}

Como gestor, auditor o responsable de seguridad, puede utilizar el servicio Activity Tracker para realizar un seguimiento de cómo los usuarios y las aplicaciones interactúan con {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

<!-- There are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones.--> 

<!-- Scenario 3. Add if your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker  -->

Activity Tracker registra actividades iniciadas por los usuarios que cambian el estado de un servicio en {{site.data.keyword.cloud_notm}}. Puede utilizar este servicio para investigar cualquier actividad anómala y acciones críticas y cumplir con los requisitos de auditoría normativa. Además, puede recibir alertas sobre acciones a medida que se producen. Los sucesos que se han recopilado cumplen con la normativa de Cloud Auditing Data Federation (CADF). 

Actualmente hay dos servicios Activity Tracker disponibles en el catálogo de {{site.data.keyword.cloud_notm}}. {{site.data.keyword.keymanagementserviceshort}} envía sucesos a ambos servicios, y puede utilizar cualquiera de ellos para supervisar la actividad de {{site.data.keyword.keymanagementserviceshort}} en {{site.data.keyword.cloud_notm}}. Sin embargo, {{site.data.keyword.cloudaccesstrailfull}} está en desuso, no se pueden crear nuevas instancias y el soporte para las instancias de servicio existentes sólo está disponible hasta el 30 de septiembre de 2019.

Para obtener más información, consulte:
* [{{site.data.keyword.at_full}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started)
* [{{site.data.keyword.cloudaccesstrailfull}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (en desuso)

<!-- If you have multiple events that might not be related, you can create different sections to group them. -->

## Lista de sucesos
{: #at-actions}

La tabla siguiente lista las acciones de {{site.data.keyword.keymanagementserviceshort}} que generan un suceso:

| Acción                   | Descripción                 |
| ------------------------ | --------------------------- |
| `kms.secrets.create`     | Crear una clave                |
| `kms.secrets.read`       | Recuperar una clave por ID        |
| `kms.secrets.delete`     | Suprimir una clave por ID          |
| `kms.secrets.list`       | Recuperar una lista de claves     |
| `kms.secrets.head`       | Recuperar el número de claves |
| `kms.secrets.wrap`       | Envolver una clave                  |
| `kms.secrets.unwrap`     | Desenvolver una clave                |
| `kms.policies.read`      | Ver una política para una clave     |
| `kms.policies.write`     | Establecer una política para una clave      |
{: caption="Tabla 1. Acciones de {{site.data.keyword.keymanagementserviceshort}} que generan sucesos de Activity Tracker" caption-side="top"}

## Visualización de sucesos
{: #at-ui}

Puede ver los sucesos de Activity Tracker que están asociados con la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} mediante [{{site.data.keyword.at_full_notm}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started#getting-started) o [{{site.data.keyword.cloudaccesstrailfull_notm}}](/docs/services/cloud-activity-tracker?topic=cloud-activity-tracker-getting-started) (en desuso).

<!-- As in the previous section, there are multiple scenarios depending on which version of Activity Tracker is enabled in your service. Choose the scenario that best suits your service, and delete the other ones. --> 

<!-- Scenario 3: If your service is AT-enabled for IBM Cloud Activity Tracker with LogDNA and also for IBM Cloud Activity Tracker, add the information that is relevant from scenario 1 and scenario 2. -->

<!-- Option 2: Location based service: A location-based service generates events in the same location where the service instance is provisioned. For example, Certificate Manager. -->

### Uso de {{site.data.keyword.at_full_notm}}
{: #at-ui-logdna}

Los sucesos que están generados por una instancia de {{site.data.keyword.keymanagementserviceshort}} se reenvían automáticamente a la instancia de servicio de {{site.data.keyword.at_full_notm}} que está disponible en la misma ubicación. 

{{site.data.keyword.at_full_notm}} solo puede tener una instancia por ubicación. Para ver los sucesos, debe acceder a la interfaz de usuario web del servicio {{site.data.keyword.at_full_notm}} en la misma ubicación en la que está disponible la instancia de servicio. Para obtener más información, consulta [Inicio de la interfaz de usuario web a través de la interfaz de usuario de IBM Cloud](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-launch#launch_step2).

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

### Uso de {{site.data.keyword.cloudaccesstrailfull_notm}} (en desuso)
{: #at-ui-legacy}

Los sucesos de {{site.data.keyword.cloudaccesstrailshort}} están disponibles en el **dominio de la cuenta** de {{site.data.keyword.cloudaccesstrailshort}} disponible en la región de {{site.data.keyword.cloud_notm}} donde se han generado los sucesos. Para obtener más información, consulte [Visualización de sucesos](/docs/services/cloud-activity-tracker/how-to/manage-events-ui?topic=cloud-activity-tracker-getting-started#gs_step4).


## Analizar sucesos
{: #at-events-analyze}

<!-- Provide information about the events in your service that add additional information in requestData and responseData. See the IAM Events topic for a sample topic that includes this section: https://cloud.ibm.com/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-at_events_iam.  -->

Debido a la confidencialidad de la información para una clave de cifrado, cuando se genera un suceso como resultado de una llamada de API al servicio de {{site.data.keyword.keymanagementserviceshort}}, el suceso generado no incluye información detallada sobre la clave. El suceso incluye un ID de correlación que puede utilizar para identificar la clave internamente en el entorno de nube. El ID de correlación es un campo que se devuelve como parte del campo `responseBody.content`. Puede utilizar esta información para correlacionar la clave de {{site.data.keyword.keymanagementserviceshort}} con la información de la acción de la que se ha informado a través del suceso de {{site.data.keyword.cloudaccesstrailshort}}.

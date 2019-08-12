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

# Seguridad y conformidad
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} tiene estrategias de seguridad de datos para satisfacer sus necesidades de conformidad y asegurarse de que sus datos permanezcan seguros y protegidos en la nube.
{: shortdesc}

## Seguridad
{: #security-ready}

{{site.data.keyword.keymanagementserviceshort}} garantiza la seguridad mediante el cumplimiento con las prácticas recomendadas de {{site.data.keyword.IBM_notm}} para sistemas, redes e ingeniería segura. 

Para obtener más información sobre los controles de seguridad en {{site.data.keyword.cloud_notm}}, consulte [¿Cómo sé que mis datos son seguros?](/docs/overview?topic=overview-security#security).
{: tip}

### Cifrado de datos
{: #data-encryption}

{{site.data.keyword.keymanagementserviceshort}} utiliza [módulos de seguridad de hardware (HSM) de {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/hardware-security-module){: external} para generar material de claves gestionadas por el proveedor y llevar a cabo operaciones de [cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption). Los HSM son dispositivos de hardware que no permite manipulaciones y que almacenan y utilizan material de claves de cifrado sin exponer las claves fuera de un límite de cifrado.

El acceso al servicio tiene lugar por HTTPS, y la comunicación interna de {{site.data.keyword.keymanagementserviceshort}} utiliza el protocolo TLS 1.2 (Transport Layer Security) para cifrar los datos en tránsito.

### Supresión de datos
{: #data-deletion}

Cuando se suprime una clave, el servicio marca la clave como suprimida y la clave pasa al estado _Destruida_. Las claves que están en este estado ya no son recuperables, y los servicios de nube que utilizan la clave ya no pueden descifrar los datos asociados a la clave. Los datos siguen estando en estos servicios de forma encriptada. Los metadatos asociados con la clave como, por ejemplo, el nombre y el historial de transiciones de la clave, se mantienen en la base de datos de {{site.data.keyword.keymanagementserviceshort}}. 

La supresión de una clave en {{site.data.keyword.keymanagementserviceshort}} es una operación destructiva. Recuerde que después de suprimir una clave, no se puede revertir la acción y que los datos asociados a la clave se pierden de forma inmediata en el momento en que se suprime la clave. Antes de suprimir una clave, revise los datos que tiene asociados y asegúrese de que ya no necesita acceder a ellos. No suprima una clave que esté protegiendo activamente datos en los entornos de producción. 

Para ayudarle a determinar qué datos están protegidos por una clave, puede ver cómo se correlaciona la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} con los servicios de nube existentes ejecutando `ibmcloud iam authorization-policies`. Para obtener más información sobre cómo visualizar las autorizaciones de servicio desde la consola, consulte [Concesión de acceso entre los servicios](/docs/iam?topic=iam-serviceauth).
{: note}

## Conformidad
{: #compliance-ready}

{{site.data.keyword.keymanagementserviceshort}} cumple con los controles de los estándares de conformidad globales, regionales y del sector, incluido el GDPR, la HIPAA, la ISO 27001/27017/27018 y otras normativas. 

Para obtener una lista completa de certificaciones de conformidad de {{site.data.keyword.cloud_notm}}, consulte [Conformidad en {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.
{: tip}

### Soporte en la UE
{: #eu-support}

{{site.data.keyword.keymanagementserviceshort}} aplica controles adicionales para proteger los recursos de {{site.data.keyword.keymanagementserviceshort}} en la Unión Europea (UE). 

Si utiliza recursos de {{site.data.keyword.keymanagementserviceshort}} en la región de Frankfurt, Alemania para procesar datos personales para ciudadanos europeos, puede habilitar el valor Soporte en la UE en la cuenta de {{site.data.keyword.cloud_notm}}. Para obtener más información, consulte [Habilitación del valor Soporte en la UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) y [Solicitud de soporte para recursos en la Unión Europea](/docs/get-support?topic=get-support-getting-customer-support#eusupported).

### Reglamento General de Protección de Datos (GDPR)
{: #gdpr}

El GDPR busca crear un marco de ley de protección de datos unificado en la Unión Europea y trata de devolver a los ciudadanos el control de sus datos personales, mientras impone reglas estrictas a quienes alojan y procesan los datos, en cualquier parte del mundo.

{{site.data.keyword.IBM_notm}} está comprometido con ofrecer a nuestros clientes y Business Partners de {{site.data.keyword.IBM_notm}} soluciones innovadoras de privacidad, seguridad y control de los datos para ayudarles en la implantación de GDPR.

Para garantizar la conformidad con el GDPR para los recursos de {{site.data.keyword.keymanagementserviceshort}}, [habilite el valor Soporte en la UE](/docs/account?topic=account-eu-hipaa-supported#bill_eusupported) en la cuenta de {{site.data.keyword.cloud_notm}}. Puede obtener más información sobre cómo {{site.data.keyword.keymanagementserviceshort}} procesa y protege los datos personales revisando los anexos siguientes.

- [{{site.data.keyword.keymanagementservicefull_notm}} Data Sheet Addendum (DSA)](https://www.ibm.com/software/reports/compatibility/clarity-reports/report/html/softwareReqsForProduct?deliverableId=180A0EC0658B11E5A8DABB56563AC132){: external}
- [{{site.data.keyword.IBM_notm}} Data Processing Addendum (DPA)](https://www.ibm.com/support/customer/csol/terms/?cat=dpa){: external}

### Soporte de HIPAA
{: #hipaa-ready}

{{site.data.keyword.keymanagementserviceshort}} cumple con los controles de la ley HIPAA (Health Insurance Portability and Accountability Act) de EE. UU. para garantizar la protección de la información sanitaria protegida (PHI). 

Si usted o su empresa forman parte de una entidad adherida a HIPAA, pueden habilitar el valor Soporte de HIPAA para la cuenta de {{site.data.keyword.cloud_notm}}. Para obtener más información, consulte [Habilitación del valor Soporte de HIPAA](/docs/account?topic=account-eu-hipaa-supported#enabling-hipaa).

### ISO 27001, 27017, 27018
{: #iso}

{{site.data.keyword.keymanagementserviceshort}} dispone de los certificados ISO 27001, 27017, 27018. Puede ver las certificaciones de conformidad visitando [Conformidad en {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}. 

### SOC 2 Tipo 1
{: #soc2-type1}

{{site.data.keyword.keymanagementserviceshort}} dispone de la certificación SOC 2 Tipo 1. Para obtener información sobre la solicitud de un informe de {{site.data.keyword.cloud_notm}} SOC 2, consulte [Conformidad en {{site.data.keyword.cloud_notm}}](https://www.ibm.com/cloud/compliance){: external}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: data security, Key Protect compliance, encryption key deletion

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

# Seguridad y conformidad
{: #security-and-compliance}

{{site.data.keyword.keymanagementservicefull}} tiene estrategias de seguridad de datos para satisfacer sus necesidades de conformidad y asegurarse de que sus datos permanezcan seguros y protegidos en la nube.
{: shortdesc}

## Seguridad de datos y cifrado
{: #data-security}

{{site.data.keyword.keymanagementserviceshort}} utiliza [módulos de seguridad de hardware (HSM) de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/hardware-security-module) para generar material de claves gestionadas por el proveedor y llevar a cabo operaciones de [cifrado de sobre](/docs/services/key-protect/envelope-encryption.html). Los HSM son dispositivos de hardware que no permite manipulaciones y que almacenan y utilizan material de claves de cifrado sin exponer las claves fuera de un límite de cifrado.

El acceso al servicio tiene lugar por HTTPS, y la comunicación interna de {{site.data.keyword.keymanagementserviceshort}} utiliza el protocolo TLS 1.2 (Transport Layer Security) para cifrar los datos en tránsito.

Para obtener más información acerca de cómo {{site.data.keyword.keymanagementserviceshort}} cumple los requisitos de conformidad, consulte [Conformidad de plataforma y servicio](/docs/overview/security.html#compliancetable).

## Supresión de datos
{: #data-deletion}

Cuando se suprime una clave, el servicio marca la clave como suprimida y la clave pasa al estado _Destruida_. Las claves que están en este estado ya no son recuperables, y los servicios de nube que utilizan la clave ya no pueden descifrar los datos asociados a la clave. Los datos siguen estando en estos servicios de forma encriptada. Los metadatos asociados con la clave como, por ejemplo, el nombre y el historial de transiciones de la clave, se mantienen en la base de datos de {{site.data.keyword.keymanagementserviceshort}}. 

La supresión de una clave en {{site.data.keyword.keymanagementserviceshort}} es una operación destructiva. Recuerde que después de suprimir una clave, no se puede revertir la acción y que los datos asociados a la clave se pierden de forma inmediata en el momento en que se suprime la clave. Antes de suprimir una clave, revise los datos que tiene asociados y asegúrese de que ya no necesita acceder a ellos. No suprima una clave que esté protegiendo activamente datos en los entornos de producción. 

Para ayudarle a determinar qué datos están protegidos por una clave, puede ver cómo se correlaciona la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} con los servicios de nube existentes ejecutando `ibmcloud iam authorization-policies`. Para obtener más información sobre cómo visualizar las autorizaciones de servicio desde la consola, consulte [Concesión de acceso entre los servicios](/docs/iam/authorizations.html#serviceauth).
{: note}

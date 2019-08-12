---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect availability, Key Protect disaster recovery

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

# Alta disponibilidad y recuperación tras desastre
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} es un servicio regional de alta disponibilidad con funciones automáticas que le ayudan a mantener sus aplicaciones seguras y operativas.
{: shortdesc}

Utilice esta página para obtener más información sobre las estrategias de disponibilidad y de recuperación tras desastre de {{site.data.keyword.keymanagementserviceshort}}.

## Ubicaciones, arrendamiento y disponibilidad
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} es un servicio regional multiarrendatario. 

Puede crear recursos de {{site.data.keyword.keymanagementserviceshort}} en una de las [regiones de {{site.data.keyword.cloud_notm}}](/docs/services/key-protect?topic=key-protect-regions#regions) admitidas, que representan la zona geográfica donde se pueden gestionar y procesar sus solicitudes de {{site.data.keyword.keymanagementserviceshort}}. Cada región de {{site.data.keyword.cloud_notm}} contiene [varias zonas de disponibilidad](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external} para cumplir los requisitos de acceso, baja latencia y seguridad de la región.

A medida que planifique la estrategia de cifrado en reposo con {{site.data.keyword.cloud_notm}}, tenga presente que suministrar {{site.data.keyword.keymanagementserviceshort}} en una región que esté cercana es más probable que dé lugar a conexiones más rápidas y fiables al interactuar con las API de {{site.data.keyword.keymanagementserviceshort}}. Elija una región específica si los usuarios, apps o servicios que dependen de un recurso de {{site.data.keyword.keymanagementserviceshort}} están concentrados geográficamente. Recuerde que los usuarios y servicios que están lejos de la región pueden experimentar una latencia más alta. 

Las claves de cifrado están limitadas a la región donde se crean. {{site.data.keyword.keymanagementserviceshort}} no copia ni exporta claves de cifrado a otras regiones.
{: note}

## Recuperación tras desastre
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} tiene establecida una recuperación tras desastre regional con un objetivo de tiempo de recuperación (RTO) de una hora. El servicio sigue los requisitos de {{site.data.keyword.cloud_notm}} para planificar y recuperarse de sucesos de desastre. Para obtener más información, consulte [Recuperación tras desastre](/docs/overview?topic=overview-zero-downtime#disaster-recovery).



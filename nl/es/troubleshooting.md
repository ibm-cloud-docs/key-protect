---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

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
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}

# Resolución de problemas
{: #troubleshooting}

Entre los problemas generales relacionados con la utilización de {{site.data.keyword.keymanagementservicefull}} se pueden incluir el suministro de cabeceras o credenciales correctas al interactuar con la API. En muchos de los casos, puede solucionar estos problemas siguiendo unos sencillos pasos.
{: shortdesc}

## No se han podido crear ni suprimir claves
{: #unable-to-create-keys}

Al acceder a la interfaz de usuario de {{site.data.keyword.keymanagementserviceshort}} no visualiza las opciones para añadir ni suprimir claves.

Desde el panel de control de {{site.data.keyword.cloud_notm}}, seleccione su instancia del servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Puede ver una lista de claves, pero no ve las opciones para agregar o suprimir claves. 

No dispone de la autorización adecuada para realizar acciones de {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifique con el administrador que se le haya asignado el rol correcto en el grupo de recursos o la instancia de servicio. Para obtener más información sobre los roles, consulte [Roles y permisos](/docs/services/key-protect?topic=key-protect-manage-access#roles).
{: tsResolve}

## Obtención de ayuda y soporte
{: #getting-help}

Si tiene problemas o pregunta relacionadas con el uso de {{site.data.keyword.keymanagementserviceshort}}, puede consultar {{site.data.keyword.cloud_notm}} u obtener ayuda buscando información o realizando preguntas mediante un foro. También puede abrir una incidencia de soporte.
{: shortdesc}

Puede comprobar si {{site.data.keyword.cloud_notm}} está disponible desde la [página de estado](https://{DomainName}/status?tags=platform,runtimes,services){: external}.

Puede revisar los foros para ver si otros usuarios han tenido el mismo problema. Cuando utilice foros para hacer una pregunta, etiquete su pregunta para que la vean los equipos de desarrollo de {{site.data.keyword.cloud_notm}}.

- Si tiene preguntas técnicas sobre {{site.data.keyword.keymanagementserviceshort}}, publique su pregunta en [Stack Overflow](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: external} y etiquete su pregunta con "ibm-cloud" y "key-protect".
- Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [dW Answers de IBM developerWorks](https://developer.ibm.com/answers/topics/key-protect/){: external}. Incluya las etiquetas "ibm-cloud" y "key-protect".

Consulte [Obtención de soporte](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: external} para obtener más información sobre el uso de los foros.

Para obtener más información sobre cómo abrir una incidencia de soporte de {{site.data.keyword.IBM_notm}} o sobre los niveles de soporte y las gravedades de las incidencias, consulte [Cómo obtener soporte](/docs/get-support?topic=get-support-getting-customer-support){: external}.

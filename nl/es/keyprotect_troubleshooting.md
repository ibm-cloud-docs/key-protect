---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms} 
{:tsCauses: .tsCauses} 
{:tsResolve: .tsResolve}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Resolución de problemas
{: #troubleshooting}

Entre los problemas generales relacionados con la utilización de {{site.data.keyword.keymanagementservicefull}} se pueden incluir el suministro de cabeceras o credenciales correctas al interactuar con la API. En muchos de los casos, puede solucionar estos problemas siguiendo unos sencillos pasos.
{: shortdesc}

## No se puede acceder a la interfaz de usuario
{: #unabletoaccessUI}

Cuando accede a la interfaz de usuario de {{site.data.keyword.keymanagementserviceshort}}, la IU no se carga como se espera.

Desde el panel de control de {{site.data.keyword.cloud_notm}}, seleccione su instancia del servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Aparecerá el siguiente error: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

El 15 de diciembre de 2017, añadimos nuevas funciones, como el [cifrado de sobre](/docs/services/keymgmt/concepts/keyprotect_envelope.html), al servicio de {{site.data.keyword.keymanagementserviceshort}}. Ahora podía suministrar el servicio de {{site.data.keyword.keymanagementserviceshort}} a nivel global, sin necesidad de especificar una organización y espacio de Cloud Foundry.
{: tsCauses}

Estos cambios afectaban la interfaz de usuario de instancias anteriores del servicio. Si creó su instancia de {{site.data.keyword.keymanagementserviceshort}} antes del 28 de septiembre de 2017, es posible que la interfaz no funcione como se espera.

Estamos trabajando para solucionar el problema. Como solución temporal, puede seguir gestionando las claves utilizando la API de {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Puede utilizar el punto final `https://ibm-key-protect.edge.bluemix.net` existente para interactuar con el servicio de {{site.data.keyword.keymanagementserviceshort}}.

**Solicitud de ejemplo**

```cURL
curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <señal_acceso>' \
-H 'bluemix-org: <GUID_organización>' \
-H 'bluemix-space: <GUID_espacio>' \
```
{: codeblock}

## No se han podido crear ni suprimir claves
{: #unabletomanagekeys}

Al acceder a la interfaz de usuario de {{site.data.keyword.keymanagementserviceshort}} no visualiza las opciones para añadir ni suprimir claves.

Desde el panel de control de {{site.data.keyword.cloud_notm}}, seleccione su instancia del servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Puede ver una lista de claves, pero no ve las opciones para agregar o suprimir claves. 

No dispone de la autorización adecuada para realizar acciones de {{site.data.keyword.keymanagementserviceshort}}.
{: tsCauses} 

Verifique con el administrador que se le haya asignado el rol correcto en el grupo de recursos o la instancia de servicio. Para obtener más información sobre los roles, consulte [Roles y permisos](/docs/services/keymgmt/keyprotect_manage_access.html#roles).
{: tsResolve}

## Obtención de ayuda y soporte
{: #getting_help}

Si tiene problemas o pregunta relacionadas con el uso de {{site.data.keyword.keymanagementserviceshort}}, puede consultar {{site.data.keyword.cloud_notm}} u obtener ayuda buscando información o realizando preguntas mediante un foro. También puede abrir una incidencia de soporte.
{: shortdesc}

Puede comprobar si {{site.data.keyword.cloud_notm}} está disponible desde la [página de estado de ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/status?tags=platform,runtimes,services).

Puede revisar los foros para ver si otros usuarios han tenido el mismo problema. Cuando utilice foros para hacer una pregunta, etiquete su pregunta para que la vean los equipos de desarrollo de {{site.data.keyword.cloud_notm}}.

- Si tiene preguntas técnicas sobre {{site.data.keyword.keymanagementserviceshort}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} y etiquete su pregunta con "ibm-cloud" y "key-protect".
- Para preguntas sobre el servicio y para obtener instrucciones para empezar, utilice el foro [IBM developerWorks dW Answers ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/key-protect/?smartspace=bluemix){: new_window}. Incluya las etiquetas "ibm-cloud"
y "key-protect".

Consulte [Obtención de ayuda ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/support/index.html#getting-help){: new_window} para obtener más información sobre el uso de los foros.

Para obtener más información sobre cómo abrir una incidencia de soporte de {{site.data.keyword.IBM_notm}} o sobre los niveles de soporte y las gravedades de las incidencias, consulte [Cómo obtener soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/support/index.html#contacting-support){: new_window}.

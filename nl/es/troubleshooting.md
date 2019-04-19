---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: can't delete service, can't use Key Protect, can't create key, can't delete key

subcollection: key-protect

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
{:note: .note}
{:important: .important}

# Resolución de problemas
{: #troubleshooting}

Entre los problemas generales relacionados con la utilización de {{site.data.keyword.keymanagementservicefull}} se pueden incluir el suministro de cabeceras o credenciales correctas al interactuar con la API. En muchos de los casos, puede solucionar estos problemas siguiendo unos sencillos pasos.
{: shortdesc}

## No se puede suprimir la instancia de servicio de Cloud Foundry
{: #unable-to-delete-service}

Cuando intenta suprimir su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}, el servicio no realiza la supresión como se espera.

Desde el panel de control de {{site.data.keyword.cloud_notm}}, vaya a **Servicios de Cloud Foundry** y seleccione su instancia de {{site.data.keyword.keymanagementserviceshort}}. Pulse el icono ⋮ para abrir una lista de opciones de la oferta de servicio y pulse **Suprimir servicio**.
{: tsSymptoms}

El servicio no realiza la supresión y se muestra el siguiente error: 
```
403 Prohibido: Esta acción no se puede completar porque tiene secretos existentes en el servicio de Key Protect. Primero debe suprimir los secretos antes de eliminar el servicio.
```
{: screen}

El 15 de diciembre de 2017, {{site.data.keyword.keymanagementserviceshort}} pasó de utilizar organizaciones, espacios y roles de Cloud Foundry a IAM y grupos de recursos. Ahora puede suministrar el servicio de {{site.data.keyword.keymanagementserviceshort}} dentro de un grupo de recursos, sin necesidad de especificar una organización y espacio de Cloud Foundry.
{: tsCauses}

Estos cambios afectaban al modo en que la anulación de suministro funciona para las instancias anteriores del servicio. Si creó su instancia de {{site.data.keyword.keymanagementserviceshort}} antes del 28 de septiembre de 2017, es posible que la supresión de servicios no funcione como se espera.

Para suprimir una instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} anterior, primero debe suprimir las claves existentes utilizando el punto final `https://ibm-key-protect.edge.bluemix.net` existente para interactuar con el servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: tsResolve}

Para suprimir las claves y la instancia de servicio:

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la CLI de {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: codeblock}

    **Nota:** Si el inicio de sesión falla, ejecute el mandato `bx login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.

2. Seleccione la región, organización y espacio de {{site.data.keyword.cloud_notm}} que contiene su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    Anote los nombres de la organización y el espacio en la salida de CLI. También puede ejecutar `ibmcloud cf target` para tomar como destino su organización y espacio de Cloud Foundry.

    ```sh
    ibmcloud cf target -o <nombre_organización> -s <nombre_espacio>
    ```
    {: codeblock}

3. Recupere los GUID de espacio y organización de {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud iam org <nombre_organización> --guid
    ibmcloud iam space <nombre_espacio> --guid
    ```
    {: codeblock}
    Sustituya `<organization_name>` y `<space_name>` por los alias exclusivos que ha asignado a su organización y espacio.

4. Recupere la señal de acceso.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

5. Liste las claves que están almacenadas en la instancia de servicio ejecutando el siguiente mandato cURL.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <señal_acceso>' \
-H 'bluemix-org: <GUID_organización>' \
-H 'bluemix-space: <GUID_espacio>' \
    ```
    {: codeblock}

    Sustituya `<access_token>`, `<organization_GUID>` y `<space_GUID>` por los valores que ha recuperado en los pasos 3 - 4. 

6. Copie el valor de ID de cada clave almacenada en la instancia de servicio.

7. Ejecute el mandato cURL siguiente para suprimir permanentemente una clave y su contenido.

    ```cURL
    curl -X DELETE \
    https://ibm-key-protect.edge.bluemix.net/api/v2/keys/<ID_clave> \
    -H 'authorization: Bearer <señal_acceso>' \
    -H 'bluemix-org: <GUID_organización>' \
    -H 'bluemix-space: <GUID_espacio>' \
    ```
    {: codeblock}

    Sustituya `<access_token>`, `<organization_GUID>`, `<space_GUID>` y `<key_ID>` por los valores que ha recuperado en los pasos 3 - 5. Repita el mandato para cada clave.    

8. Verifique que las claves se han suprimido ejecutando el siguiente mandato cURL.

    ```cURL
    curl -X GET \
https://ibm-key-protect.edge.bluemix.net/api/v2/keys \
-H 'accept: application/vnd.ibm.collection+json' \
-H 'authorization: Bearer <señal_acceso>' \
-H 'bluemix-org: <GUID_organización>' \
-H 'bluemix-space: <GUID_espacio>' \
    ```
    {: codeblock}

    Sustituya `<access_token>`, `<organization_GUID>` y `<space_GUID>` por los valores que ha recuperado en los pasos 3 - 4. 

9. Suprima la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud cf delete-service "<nombre_instancia_servicio>"
    ```
    {: codeblock}

10. Opcional: Verifique que se ha suprimido la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} yendo al panel de control de {{site.data.keyword.cloud_notm}}.

    También puede listar los servicios de Cloud Foundry disponibles en el espacio de destino ejecutando el siguiente mandato.

    ```sh
    ibmcloud cf service list
    ```
    {: codeblock}


## No se puede acceder a la interfaz de usuario
{: #unable-to-access-ui}

Cuando accede a la interfaz de usuario de {{site.data.keyword.keymanagementserviceshort}}, la IU no se carga como se espera.

Desde el panel de control de {{site.data.keyword.cloud_notm}}, seleccione su instancia del servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: tsSymptoms}

Aparecerá el siguiente error: 
```
404 Not Found: Requested route ('ibm-key-protect-ui.ng.bluemix.net') does not exist.
```
{: screen}

El 15 de diciembre de 2017, añadimos nuevas funciones, como el [cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption), al servicio de {{site.data.keyword.keymanagementserviceshort}}. Ahora puede suministrar el servicio de {{site.data.keyword.keymanagementserviceshort}} dentro de un grupo de recursos, sin necesidad de especificar una organización y espacio de Cloud Foundry.
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

Puede comprobar si {{site.data.keyword.cloud_notm}} está disponible desde la [página de estado de ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/status?tags=platform,runtimes,services).

Puede revisar los foros para ver si otros usuarios han tenido el mismo problema. Cuando utilice foros para hacer una pregunta, etiquete su pregunta para que la vean los equipos de desarrollo de {{site.data.keyword.cloud_notm}}.

- Si tiene preguntas técnicas sobre {{site.data.keyword.keymanagementserviceshort}}, publique su pregunta en [Stack Overflow ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://stackoverflow.com/search?q=key-protect+ibm-cloud){: new_window} y etiquete su pregunta con "ibm-cloud" y "key-protect".
- Para formular preguntas sobre el servicio y obtener instrucciones de iniciación, utilice el foro [IBM developerWorks dW Answers ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/key-protect/){: new_window}. Incluya las etiquetas "ibm-cloud"
y "key-protect".

Consulte [Obtención de soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support?topic=get-support-getting-customer-support#using-avatar){: new_window} para obtener más información sobre el uso de los foros.

Para obtener más información sobre cómo abrir una incidencia de soporte de {{site.data.keyword.IBM_notm}} o sobre los niveles de soporte y las gravedades de las incidencias, consulte [Cómo obtener soporte ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/get-support?topic=get-support-getting-customer-support){: new_window}.

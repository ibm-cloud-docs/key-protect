---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: automatic key rotation, set rotation policy, policy-based, key rotation

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

# Establecimiento de una política de rotación
{: #set-rotation-policy}

Puede establecer una política de rotación automática para una clave raíz utilizando {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

Cuando se establece una política de rotación automática para una clave raíz, se acorta el tiempo de vida de la clave a intervalos regulares y se limita la cantidad de información que protege dicha clave.

Puede crear una política de rotación sólo para las claves raíz generadas en {{site.data.keyword.keymanagementserviceshort}}. Si inicialmente ha importado la clave raíz, debe proporcionar nuevo material de clave con codificación base64 para rotar la clave. Para obtener más información, consulte [Rotación de claves raíz bajo demanda](/docs/services/key-protect?topic=key-protect-rotate-keys#rotate-keys).
{: note}

¿Desea obtener más información acerca de las opciones de rotación claves en {{site.data.keyword.keymanagementserviceshort}}? Consulte [Comparación de las opciones de rotación de claves](/docs/services/key-protect?topic=key-protect-key-rotation#compare-key-rotation-options) para obtener más información.
{: tip}

## Gestión de las políticas de rotación en la GUI
{: #manage-policies-gui}

Si prefiere gestionar las políticas de sus claves raíz utilizando una interfaz gráfica, puede utilizar la GUI de {{site.data.keyword.keymanagementserviceshort}}.

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/){: new_window}.
2. Vaya a **Menú** &gt; **Lista de recursos** para ver una lista de sus recursos.
3. Desde la lista de recursos de {{site.data.keyword.cloud_notm}} seleccione su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.
4. En la página de detalles de la aplicación, utilice la tabla de **Claves** tabla para examinar las claves en el servicio.
5. Pulse el icono ⋯ para abrir una lista de opciones para una clave específica.
6. En el menú de opciones, pulse **Gestionar política** para gestionar la política de rotación de la clave.
7. En la lista de opciones de rotación, seleccione una frecuencia de rotación en meses.

    Si la clave tiene una política de rotación existente, la interfaz muestra el período de rotación de clave existente.

8. Pulse **Crear política** para establecer la política para la clave.

Cuando es hora de rotar la clave, en base al intervalo de rotación que ha especificado, {{site.data.keyword.keymanagementserviceshort}} sustituye automáticamente la clave raíz por material de clave nuevo.

## Gestión de políticas de rotación con la API
{: #manage-rotation-policies-api}

### Visualización de una política de rotación
{: #view-rotation-policy-api}

Para obtener una vista de alto nivel, puede examinar las políticas asociadas a una clave raíz realizando una llamada `GET` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies
```
{: codeblock}

1. [Recupere las credenciales de servicio y autenticación](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Puede recuperar la política de rotación para una clave especificada ejecutando el mandato cURL siguiente.

    ```cURL
    curl -X GET \
      https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'correlation-id: <ID_correlación>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> El identificador exclusivo de la clave raíz que tiene una política de rotación existente.</td>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td><strong>Obligatorio.</strong> Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td><strong>Obligatorio.</strong> El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para crear una política de rotación con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una respuesta satisfactoria de `GET api/v2/keys/{id}/policies` devuelve detalles de política asociados a la clave. El objeto JSON siguiente muestra una respuesta de ejemplo para una clave raíz que tiene una política de rotación existente.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

    El valor de `interval_month` indica la frecuencia de rotación de la clave en meses.

### Creación de una política de rotación
{: #create-rotation-policy-api}

Cree una política de rotación para la clave raíz realizando una llamada `PUT` al punto final siguiente.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies
```
{: codeblock}

1. [Recupere las credenciales de servicio y autenticación](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Puede crear una política de rotación para una clave especificada ejecutando el mandato cURL siguiente.

    ```cURL
    curl -X PUT \
      https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'correlation-id: <ID_correlación>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <intervalo_rotación>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> El identificador exclusivo de la clave raíz para la que desea crear una política de rotación.</td>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td><strong>Obligatorio.</strong> Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td><strong>Obligatorio.</strong> El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
      <tr>
        <td><varname>intervalo_rotación</varname></td>
        <td><strong>Obligatorio.</strong> Un valor entero que determina el tiempo de intervalo de rotación de la clave en meses. El valor mínimo es <code>1</code> y el máximo es <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para crear una política de rotación con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una respuesta satisfactoria de `PUT api/v2/keys/{id}/policies` devuelve detalles de política asociados a la clave. El objeto JSON siguiente muestra una respuesta de ejemplo para una clave raíz que tiene una política de rotación existente.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 1
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-503CKNRHR7",
            "updatedat": "2019-03-06T16:31:05Z"
        }
      ]
    }
    ```
    {:screen}

### Actualización de una política de rotación
{: #update-rotation-policy-api}

Actualice una política existente para una clave raíz realizando una llamada `PUT` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies
```
{: codeblock}

1. [Recupere las credenciales de servicio y autenticación](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Puede sustituir la política de rotación para una clave especificada ejecutando el mandato cURL siguiente.

    ```cURL
    curl -X PUT \
      https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>/policies \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'correlation-id: <ID_correlación>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.policy+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.policy+json",
       "rotation": {
         "interval_month": <intervalo_rotación_nuevo>
        }
       }
      ]
    }'
    ```
    {: codeblock}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> El identificador exclusivo de la clave raíz para la que desea sustituir una política de rotación.</td>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td><strong>Obligatorio.</strong> Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-access-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td><strong>Obligatorio.</strong> El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
      <tr>
        <td><varname>intervalo_rotación_nuevo</varname></td>
        <td><strong>Obligatorio.</strong> Un valor entero que determina el tiempo de intervalo de rotación de la clave en meses. El valor mínimo es <code>1</code> y el máximo es <code>12</code>.
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para crear una política de rotación con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una respuesta satisfactoria de `PUT api/v2/keys/{id}/policies` devuelve detalles de política actualizados asociados a la clave. El objeto JSON siguiente muestra una respuesta de ejemplo para una clave raíz con una política de rotación actualizada.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.policy+json"
        },
    "resources": [
      {
            "id": "a1769941-9805-4593-b6e6-290e42dd1cb5",
            "rotation": {
                "interval_month": 2
            },
            "createdby": "IBMid-503CKNRHR7",
            "createdat": "2019-03-06T16:31:05Z",
            "updatedby": "IBMid-820DPWINC2",
            "updatedat": "2019-03-10T12:24:22Z"
        }
      ]
    }
    ```
    {:screen}

    Los valores de `interval_month` y `updatedat` se actualizan en los detalles de política de la clave. Si otro usuario distinto actualiza una política para una clave que inicialmente ha creado usted, también cambia el valor `updatedby` también cambia y muestra el identificador de la persona que ha enviado la solicitud.

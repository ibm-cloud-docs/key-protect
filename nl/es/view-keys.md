---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Visualización de claves
{: #view-keys}

{{site.data.keyword.keymanagementservicefull}} proporciona un sistema centralizado para ver, gestionar y auditar sus claves de cifrado. Audite sus claves y restricciones de acceso a claves para garantizar la seguridad de sus recursos.
{: shortdesc}

Audite la configuración de las claves de forma regular:

- Examine cuando se crean las claves y determine si es momento de rotar la clave.
- [Supervise llamadas API a {{site.data.keyword.keymanagementserviceshort}} con {{site.data.keyword.cloudaccesstrailshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-activity-tracker/tutorials/manage_events_cli.html){: new_window}.
- Inspeccione qué usuarios tienen acceso a las claves y si el nivel de acceso es apropiado.

Para obtener más información sobre cómo auditar el acceso a sus recursos, consulte [Gestión del acceso de usuarios con IAM Cloud](/docs/services/key-protect/manage-access.html).

## Visualización de claves con GUI
{: #gui}

Si prefiere examinar las claves en el servicio mediante una interfaz gráfica, puede utilizar el panel de control de {{site.data.keyword.keymanagementserviceshort}}.

[Después de crear o importar sus claves existentes en el servicio](/docs/services/key-protect/create-root-keys.html), complete los siguientes pasos para visualizar sus claves.

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/).
2. Desde el panel de control de {{site.data.keyword.cloud_notm}} seleccione su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.
3. Examine las características generales de sus claves desde el panel de control de {{site.data.keyword.keymanagementserviceshort}}:

    <table>
      <tr>
        <th>Columna</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td>Nombre</td>
        <td>Alias descriptivo único que se asignó a su clave.</td>
      </tr>
      <tr>
        <td>ID</td>
        <td>ID de clave exclusivo que se asignó a su clave con el servicio de {{site.data.keyword.keymanagementserviceshort}}. Puede utilizar el valor de ID para realizar llamadas al servicio con la [API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/apidocs/kms).</td>
      </tr>
      <tr>
        <td>Estado</td>
        <td>[Estado de clave](/docs/services/key-protect/concepts/key-states.html) que se basa en el documento [NIST Special Publication 800-57, Recommendation for Key Management ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf). Incluye los estados <i>Pre-activa</i>, <i>Activa</i>, <i>Desactivada</i> y <i>Destruida</i>.</td>
      </tr>
      <tr>
        <td>Tipo</td>
        <td>[Tipo de clave](/docs/services/key-protect/concepts/envelope-encryption.html#key-types) que describe el propósito designado de la clave dentro del servicio.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe la tabla <b>Claves</b></caption>
    </table>

## Visualización de claves con la API
{: #api}

Puede recuperar el contenido de sus claves utilizando la API {{site.data.keyword.keymanagementserviceshort}}.

### Recuperación de una lista de las claves
{: #retrieve-keys-api}

Para obtener una vista de alto nivel, puede examinar claves que se gestionan en su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}} haciendo una llamada `GET` al siguiente punto final.

```
https://keyprotect.<región>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect/access-api.html).

2. Ejecute el siguiente mandato cURL para visualizar las características generales acerca de las claves.

    ```cURL
    curl -X GET \
    https://keyprotect.<region>.bluemix.net/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <IAM_token>' \
    -H 'bluemix-instance: <instance_ID>' \
    -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    Para trabajar con claves dentro de un espacio y organización de Cloud Foundry en su cuenta, sustituya `Bluemix-Instance` con las cabeceras adecuadas de `Bluemix-org` y `Bluemix-space`. [Para obtener más información, consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/apidocs/kms){: new_window}.
    {: tip}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td>La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect/regions.html#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td>Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api.html#retrieve-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td>El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>Opcional: El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 2. Describe las variables necesarias para visualizar claves con la API {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una solicitud satisfactoria de `GET /v2/keys` devuelve un conjunto de claves disponibles en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.collection+json",
        "collectionTotal": 2
      },
    "resources": [
      {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Standard key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": true
        },
        {
          "id": "...",
          "type": "application/vnd.ibm.kms.key+json",
          "name": "Root key",
          "description": "...",
          "state": 1,
          "crn": "...",
          "algorithmType": "AES",
          "createdBy": "...",
          "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
          "algorithmMetadata": {
            "bitLength": "256",
            "mode": "GCM"
          },
          "extractable": false
        }
      ]
    }
    ```
    {:screen}

    De forma predeterminada, `GET /keys` devuelve las primeras 2000 claves, pero puede ajustar este límite utilizando el parámetro `limit` al momento. Para obtener más información sobre `limit` y `offset`, consulte [Recuperación de un subconjunto de claves](#retrieve_subset_keys_api).
    {: tip}

### Recuperación de un subconjunto de claves
{: #retrieve-subset-keys-api}

Al especificar los parámetros `limit` y `offset` al momento, puede recuperar un subconjunto de sus claves, empezando por el valor `offset` que especifique. 

Por ejemplo, puede tener 3000 claves totales almacenadas en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}, pero desea recuperar 200 o 300 claves cuando realice una solicitud de `GET /keys`. 

Puede utilizar la solicitud de ejemplo siguiente para recuperar un conjunto distinto de claves.

  ```cURL
  curl -X GET \
  https://keyprotect.<región>.bluemix.net/api/v2/keys?offset=<desplazamiento>&limit=<límite> \ -H 'accept: application/vnd.ibm.collection+json' \
  -H 'authorization: Bearer <señal_IAM>' \
  -H 'bluemix-instance: <ID_instancia>' \
  -H 'correlation-id: <ID_correlación>' \
  ```
  {: codeblock}

  Sustituya las variables `limit` y `offset` en su solicitud de acuerdo con la tabla siguiente.
  <table>
    <tr>
      <th>Variable</th>
      <th>Descripción</th>
    </tr>
    <tr>
      <td><p><varname>desplazamiento</varname></p></td>
      <td>
        <p>Opcional: El número de claves a omitir.</p> 
        <p>Por ejemplo, si dispone de 50 claves en su instancia y desea listar de 26 a 50 claves, utilice <code>../keys?offset=25</code>. También puede conectar <code>offset</code> con <code>limit</code> para navegar por los recursos disponibles.</p>
      </td>
    </tr>
    <tr>
      <td><p><varname>límite</varname></p></td>
      <td>
        <p>Opcional: El número de claves a recuperar.</p> 
        <p>Por ejemplo, si dispone de 100 claves en su instancia y desea listar solo 10 claves, utilice <code>../keys?limit=10</code>. El valor máximo de <code>limit</code> es 5000.</p>
      </td>
    </tr>
    <caption style="caption-side:bottom;">Tabla 2. Describe las variables <code>limit</code> y <code>offset</code></caption>
  </table>

Para obtener notas de uso, compruebe los ejemplos siguientes para configurar los parámetros de consulta `limit` y `offset`.

<table>
  <tr>
    <th>URL</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>.../keys</code></td>
    <td>Lista todos los recursos disponibles, hasta las 2000 primeras claves.</td>
  </tr>
  <tr>
    <td><code>.../keys?limit=10</code></td>
    <td>Lista las 10 primeras claves.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=25&limit=50</code></td>
    <td>Lista las claves entre 26 y 50.</td>
  </tr>
  <tr>
    <td><code>.../keys?offset=3000&limit=50</code></td>
    <td>Lista las claves entre 3001 y 3050.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabla 3. Proporciona notas de uso para el límite y el desplazamiento de los parámetros de query</caption>
</table>

El desplazamiento es la ubicación de una clave concreta en un conjunto de datos. El valor `offset` está basado en cero, lo que significa que la clave de cifrado número 10 en un conjunto de datos, es la 9 en el desplazamiento.
{: tip}

### Recuperación de una clave por ID
{: #retrieve-key-api}

Para ver información detallada sobre una clave específica, puede realizar una llamada `GET` al siguiente punto final.

```
https://keyprotect.<región>.bluemix.net/api/v2/keys/<ID_clave>
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect/access-api.html).

2. Recuperar el ID de la clave que desea acceder o gestionar.

    El valor de la ID se utiliza para acceder a la información detallada acerca de la clave, como el propio material de la clave. Puede recuperar la ID para una clave específica realizando una solicitud `GET /v2/keys`, o accediendo a la GUI de {{site.data.keyword.keymanagementserviceshort}}.

3. Ejecute el siguiente mandato cURL para obtener detalles sobre su clave y el material de la clave.

    ```cURL
    curl -X GET \
      https://keyprotect.<region>.bluemix.net/api/v2/keys/<key_ID> \
      -H 'accept: application/vnd.ibm.kms.key+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'correlation-id: <correlation_ID>' \
    ```
    {: codeblock}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.

    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td>La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Consulte los <a href="/docs/services/key-protect/regions.html#endpoints">puntos de servicio regionales</a> para obtener más información.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td>Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api.html#retrieve-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td>El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>Opcional: El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td>El identificador para una clave que ha recuperado en el [paso 1](#retrieve-keys-api).</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 4. Describe las variables necesarias para visualizar una clave especificada con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una respuesta `GET v2/keys/<key_ID>` satisfactoria devuelve detalles sobre la clave y el material de la clave. El siguiente objeto JSON muestra un valor devuelto de ejemplo para una clave estándar.

    ```
    {
        "metadata": {
            "collectionTotal": 1,
            "collectionType": "application/vnd.ibm.kms.key+json"
        },
    "resources": [
      {
            "id": "...",
            "type": "application/vnd.ibm.kms.key+json",
            "name": "Standard key",
            "description": "...",
            "state": 1,
            "crn": "...",
            "algorithmType": "AES",
            "payload": "...",
            "createdBy": "...",
            "creationDate": "YYYY-MM-DDTHH:MM:SSZ",
            "algorithmMetadata": {
                "bitLength": "256",
                "mode": "GCM"
            },
            "extractable": true
        }
      ]
    }
    ```
    {:screen}

    Para obtener una descripción detallada de los parámetros disponibles, consulte la [documentación de referencia de la API REST ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/apidocs/kms){: new_window} de {{site.data.keyword.keymanagementserviceshort}}.

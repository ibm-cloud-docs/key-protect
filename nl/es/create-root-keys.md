---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Creación de claves raíz
{: #create-root-keys}

Utilice {{site.data.keyword.keymanagementservicefull}} para crear claves raíz utilizando la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}} o mediante programación con la API de {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Las claves raíz son claves para envolver claves simétricas que se utilizan para proteger la seguridad de los datos cifrados en la nube. Para obtener información sobre las claves raíz, consulte [Cifrado de sobre](/docs/services/key-protect/concepts/envelope-encryption.html). 

## Creación de claves raíz con la interfaz gráfica de usuario
{: #gui}

[Después de crear una instancia del servicio](/docs/services/key-protect/provision.html), siga los siguientes pasos para crear una clave raíz con la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}}.

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}.
2. Vaya a **Menú** &gt; **Lista de recursos** para ver una lista de sus recursos.
3. Desde la lista de recursos de {{site.data.keyword.cloud_notm}} seleccione su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.
4. Para crear una nueva clave, pulse **Añadir clave** seleccione la ventana **Crear una clave**.

    Especifique los detalles de la clave:

    <table>
      <tr>
        <th>Valor</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td>Nombre</td>
        <td>
          <p>Un alias descriptivo exclusivo para identificar con facilidad su clave.</p>
          <p>Para proteger su privacidad, asegúrese de que el nombre de clave no contiene información de identificación personal (PII), como el nombre o la ubicación.</p>
        </td>
      </tr>
      <tr>
        <td>Tipo de clave</td>
        <td><a href="/docs/services/key-protect/concepts/envelope-encryption.html#key-types">Tipo de clave</a> que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}. En la lista de tipos de claves, seleccione <b>Clave raíz</b>.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe los valores de <b>Crear una clave</b></caption>
    </table>

5. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Crear una clave** para confirmar. 

## Creación de claves raíz con la API
{: #api}

Cree una clave raíz realizando una llamada `POST` al siguiente punto final.

```
https://keyprotect.<región>.bluemix.net/api/v2/keys
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect/access-api.html).

2. Llame a la API de [{{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window} con el mandato de cURL siguiente.

    ```cURL
    curl -X POST \
      https://keyprotect.<region>.bluemix.net/api/v2/keys \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <correlation_ID>' \
      -d '{
     "metadata": {
       "collectionType": "application/vnd.ibm.kms.key+json",
       "collectionTotal": 1
     },
    "resources": [
      {
       "type": "application/vnd.ibm.kms.key+json",
       "name": "<alias_clave>",
       "description": "<descripción_clave>",
       "expirationDate": "<YYYY-MM-DDTHH:MM:SS.SSZ>",
       "extractable": <tipo_clave>
       }
     ]
    }'
    ```
    {: codeblock}

    Para trabajar con claves dentro de un espacio y organización de Cloud Foundry en su cuenta, sustituya `Bluemix-Instance` con las cabeceras adecuadas de `Bluemix-org` y `Bluemix-space`. [Para obtener más información, consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td><varname>región</varname></td>
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect/regions.html#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>señal_IAM</varname></td>
        <td><strong>Obligatorio.</strong> Su señal de acceso de {{site.data.keyword.cloud_notm}}. Incluya el contenido completo de la señal <code>IAM</code>, incluido el valor de Bearer, en la solicitud cURL. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api#retrieve-token">Recuperación de una señal de acceso</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_instancia</varname></td>
        <td><strong>Obligatorio.</strong> El único identificador que está asignado a su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect/access-api.html#retrieve-instance-ID">Recuperación de un ID de instancia</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_correlación</varname></td>
        <td>El único identificador que se ha utilizado para rastrear y correlacionar transacciones.</td>
      </tr>
      <tr>
        <td><varname>alias_clave</varname></td>
        <td>
          <p><strong>Obligatorio.</strong> Nombre descriptivo exclusivo para identificar con facilidad su clave.</p>
          <p>Importante: Para proteger su privacidad, no almacene datos personales como metadatos para la clave.</p>
        </td>
      </tr>
      <tr>
        <td><varname>descripción_clave</varname></td>
        <td>
          <p>Una descripción ampliada para su clave.</p>
          <p>Importante: Para proteger su privacidad, no almacene datos personales como metadatos para la clave.</p>
        </td>
      </tr>
      <tr>
        <td><varname>DD-MM-AAA</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Fecha y hora de caducidad de la clave en el sistema, en formato RFC 3339. Si el atributo <code>expirationDate</code> se omite, la clave no caducará. </td>
      </tr>
      <tr>
        <td><varname>tipo_clave</varname></td>
        <td>
          <p>Valor booleano que determina si el material de clave puede dejar el servicio.</p>
          <p>Cuando establece el atributo <code>extractable</code> en <code>false</code>, el servicio crea una clave raíz que se puede utilizar para <code>envolver</code> o <code>desenvolver</code> operaciones.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para añadir una clave raíz con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Para proteger la confidencialidad de sus datos personales, evite especificar información de identificación personal (PII), como el nombre o la ubicación, cuando añades claves al servicio. Para obtener más ejemplos sobre la PII, consulte la sección 2.2 de la [NIST Special Publication 800-122 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
    {: important}

    Una respuesta `POST api/v2/keys` satisfactoria devuelve el valor del ID para la clave, junto con otros metadatos. El ID es un identificador exclusivo que se asigna a su clave y que posteriores llamadas lo utilizan para la API de {{site.data.keyword.keymanagementserviceshort}}.

3. Opcional: Verifique que la clave se ha creado ejecutando la siguiente llamada para examinar las claves en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://keyprotect.<region>.bluemix.net/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>'
    ```
    {: codeblock}

    Después de crear una clave raíz con el servicio, la clave permanece dentro de los límites de {{site.data.keyword.keymanagementserviceshort}}, y su material clave no se puede recuperar.
    {: note} 

### Qué hacer a continuación

- Para obtener más información sobre la protección de claves con cifrado de sobre, consulte [Claves de envolvimiento](/docs/services/key-protect/wrap-keys.html).
- Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

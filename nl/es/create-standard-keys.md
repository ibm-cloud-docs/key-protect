---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: create standard encryption key, create secret, persist secret, create encryption key, standard encryption key API examples

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

# Creación de claves estándar
{: #create-standard-keys}

Puede crear una clave de cifrado estándar con la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}} o mediante programación con la API de {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

## Creación de claves estándar con la interfaz gráfica de usuario
{: #create-standard-key-gui}

[Después de crear una instancia del servicio](/docs/services/key-protect?topic=key-protect-provision), siga los siguientes pasos para crear una clave estándar con la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}}.

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/){: new_window}.
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
      <tr></tr>
        <td>Tipo de clave</td>
        <td><a href="/docs/services/key-protect/concepts?topic=key-protect-envelope-encryption#key-types">Tipo de clave</a> que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}. En la lista de tipos de claves, seleccione <b>Clave estándar</b>.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe los valores de <b>Crear una clave</b></caption>
    </table>

5. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Crear una clave** para confirmar. 

## Creación de claves estándar con la API
{: #create-standard-key-api}

Cree una clave estándar realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Llame a la API de [{{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window} con el mandato de cURL siguiente.

    ```cURL
    curl -X POST \
      https://<región>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
      -H 'correlation-id: <ID_correlación>' \
      -H 'prefer: <preferencia_retorno>' \
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

    Para trabajar con claves dentro de un espacio y organización de Cloud Foundry especificado en su cuenta, sustituya `Bluemix-Instance` con las cabeceras adecuadas de `Bluemix-org` y `Bluemix-space`. [Para obtener más información, consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
    {: tip}

    Sustituya las variables en la solicitud de ejemplo siguiendo la siguiente tabla.
    <table>
      <tr>
        <th>Variable</th>
        <th>Descripción</th>
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
        <td><varname>preferencia_retorno</varname></td>
        <td><p>Una cabecera que altera el comportamiento del servidor para operaciones de <code>POST</code> y <code>DELETE</code>.</p><p>Cuando establece la variable <em>preferencia_retorno</em> en <code>return=minimal</code>, el servicio solo devuelve los metadatos de la clave como, por ejemplo, el nombre de clave y valor de ID, en el cuerpo de entidad de la respuesta. Cuando establece la variable en <code>return=representation</code>, el servicio devuelve tanto el material de la clave como los metadatos de la clave.</p></td>
      </tr>
      <tr>
        <td><varname>alias_clave</varname></td>
        <td><strong>Obligatorio.</strong> Nombre descriptivo exclusivo para identificar con facilidad su clave. Para proteger su privacidad, no almacene datos personales como metadatos para la clave.</td>
      </tr>
      <tr>
        <td><varname>descripción_clave</varname></td>
        <td>Una descripción ampliada para su clave. Para proteger su privacidad, no almacene datos personales como metadatos para la clave.</td>
      </tr>
      <tr>
        <td><varname>DD-MM-AAA</varname><br><varname>HH:MM:SS.SS</varname></td>
        <td>Fecha y hora de caducidad de la clave en el sistema, en formato RFC 3339. Si el atributo <code>expirationDate</code> se omite, la clave no caducará.</td>
      </tr>
      <tr>
        <td><varname>tipo_clave</varname></td>
        <td>
          <p>Valor booleano que determina si el material de claves puede dejar el servicio.</p>
          <p>Cuando establece el atributo <code>extractable</code> en <code>true</code>, el servicio crea una clave estándar que puede almacenar en sus apps o servicios.</p>
        </td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 2. Describe las variables necesarias para añadir una clave estándar con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Para proteger la confidencialidad de sus datos personales, evite especificar información de identificación personal (PII), como el nombre o la ubicación, cuando añades claves al servicio. Para obtener más ejemplos sobre la PII, consulte la sección 2.2 de la [NIST Special Publication 800-122 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.nist.gov/publications/guide-protecting-confidentiality-personally-identifiable-information-pii){: new_window}.
    {: important}

    Una respuesta `POST api/v2/keys` satisfactoria devuelve el valor del ID para la clave, junto con otros metadatos. El ID es un identificador exclusivo que se asigna a su clave y que posteriores llamadas lo utilizan para la API de {{site.data.keyword.keymanagementserviceshort}}.

3. Opcional: Verifique que la clave se ha creado ejecutando la siguiente llamada para obtener las claves en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://us-south.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>'
    ```
    {: codeblock}


## Qué hacer a continuación
{: #create-standard-key-next-steps}

- Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

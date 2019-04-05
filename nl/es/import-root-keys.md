---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-20"

keywords: import root key, upload root key, import key-wrapping key, upload key-wrapping key, import CRK, import CMK, upload CRK, upload CMK, import customer key, upload customer key, key-wrapping key, root key API examples

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

# Importación de claves raíz
{: #import-root-keys}

Utilice {{site.data.keyword.keymanagementservicefull}} para proteger claves raíz utilizando la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}} o mediante programación con la API de {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Las claves raíz son claves para envolver claves simétricas que se utilizan para proteger la seguridad de los datos cifrados en la nube. Para obtener más información sobre la importación de claves en {{site.data.keyword.keymanagementserviceshort}}, consulte [Cómo traer las claves de cifrado a la nube](/docs/services/key-protect?topic=key-protect-importing-keys).

## Importación de claves raíz con la interfaz gráfica de usuario
{: #import-root-key-gui}

[Después de crear una instancia del servicio](/docs/services/key-protect?topic=key-protect-provision), siga los siguientes pasos para añadir su clave raíz existente con la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}}.

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/){: new_window}.
2. Vaya a **Menú** &gt; **Lista de recursos** para ver una lista de sus recursos.
3. Desde la lista de recursos de {{site.data.keyword.cloud_notm}} seleccione su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.
4. Para importar una clave, pulse **Añadir clave** y, a continuación, seleccione la ventana **Importar su propia clave**.

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
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Tipo de clave</a> que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}. En la lista de tipos de claves, seleccione <b>Clave raíz</b>.</td>
      </tr>
      <tr>
        <td>Material de la clave</td>
        <td>
          <p>Material de clave codificado en base64 como, por ejemplo, una clave para envolver claves existentes, que desee almacenar y gestionar en el servicio.</p>
          <p>Asegúrese de que el material de la clave cumple los requisitos siguientes:</p>
          <p>
            <ul>
              <li>La clave debe ser de 128, 192 o 256 bits.</li>
              <li>Los bytes de datos, por ejemplo, 32 bytes para 256 bits, se deben codificar en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe los valores de <b>Importar su propia clave</b></caption>
    </table>

5. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Importar clave** para confirmar. 

## Importación de claves raíz con la API
{: #import-root-key-api}

Puede importar las claves raíz en el servicio utilizando la API de {{site.data.keyword.keymanagementserviceshort}}.

Planifique la importación de claves [revisando las opciones para crear y cifrar el material de claves](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Para una mayor seguridad, puede habilitar la importación segura del material de claves utilizando una [clave de transporte](/docs/services/key-protect?topic=key-protect-importing-keys#transport-keys) para cifrar el material de claves antes de traerlo a la nube. Si prefiere importar una clave raíz sin utilizar una clave de transporte, vaya al [paso 4](#import-root-key).
{: note}

### Paso 1: Crear una clave de transporte
{: #create-transport-key}

Actualmente, las claves de transporte son una característica beta. Las características beta pueden cambiar en cualquier momento, y las actualizaciones futuras pueden introducir cambios incompatibles con la última versión.
{: important}

Cree una clave de transporte para la instancia de servicio realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/lockers
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Establezca una política para la clave de transporte llamando a la [API de {{site.data.keyword.keymanagementserviceshort}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

    ```cURL
    curl -X POST \
      https://<región>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/json' \
      -d '{
     "expiration": <hora_caducidad>,  \
     "maxAllowedRetrievals": <recuento_uso>  \
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
      <td><varname>hora_caducidad</varname></td>
      <td>
        <p>El tiempo en segundos desde la creación de una clave de transporte que determina el tiempo durante el cual la clave sigue siendo válida.</p>
        <p>El valor mínimo es 300 segundos (5 minutos), y el valor máximo es 86400 (24 horas). El valor predeterminado es 600 (10 minutos).</p>
      </td>
    </tr>
    <tr>
      <td><varname>recuento_uso</varname></td>
      <td>El número de veces que se puede recuperar una clave de transporte dentro de su hora de caducidad hasta que ya no se pueda acceder a ella. El valor predeterminado
es 1.</td>
    </tr>
      <caption style="caption-side:bottom;">Tabla 2. Describe las variables necesarias para crear una te a transport key {{site.data.keyword.keymanagementserviceshort}} API</caption>
  </table>

  Una respuesta satisfactoria de `POST api/v2/lockers` devuelve el valor del ID para la clave de transporte, junto con otros metadatos. El ID es un identificador exclusivo que se asocia a una clave de transporte y que posteriores llamadas lo utilizan para la API de {{site.data.keyword.keymanagementserviceshort}}.

### Paso 2: Recuperar la clave de transporte y la señal de importación
{: #retrieve-transport-key}

Recupere una clave de transporte y una señal de importación realizando una llamada `GET` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/lockers/<ID_clave>
```
{: codeblock}

1. Llame a la API de [{{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window} con el mandato de cURL siguiente.

    ```cURL
    curl -X GET \
      https://<región>.kms.cloud.ibm.com/api/v2/locker/<locker_id> \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key+json'
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
        <td><varname>ID_bloqueador</varname></td>
        <td><strong>Obligatorio.</strong> El identificador de la clave de transporte que ha creado en el <a href="#create-transport-key">paso 1</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 3. Describe las variables necesarias para recuperar una clave de transporte con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Una respuesta satisfactoria de `GET api/v2/lockers/{id}` devuelve una clave de cifrado pública codificada con DER de 4096 bits en formato PKIX que puede utilizar para cifrar el material de clave raíz, junto con una señal de importación que se utiliza para verificar la integridad de la clave de transporte.

### Paso 3: Cifrar el material de clave
{: #encrypt-root-key}

Después de recuperar la clave de transporte, utilice la clave para cifrar el material de clave que desea importar a {{site.data.keyword.keymanagementserviceshort}}.  

<!-- TODO: Add link to tutorial that uses OpenSSL for key generation and encryption (in progress)-->

Para generar material de claves de forma local, [revise las opciones para crear claves de cifrado simétricas](/docs/services/key-protect?topic=key-protect-importing-keys#plan-ahead). Por ejemplo, es posible que desee utilizar el sistema de gestión de claves internas de la organización, respaldado por un módulo de seguridad de hardware (HSM) local, para crear y exportar el material de claves.
{: note}

Para cifrar el material de claves:

1. Exporte el material de claves de 256 bits en formato binario desde el sistema de gestión de claves local.

    Para aprender a crear y exportar material de claves, consulte la documentación del HSM local o del sistema de gestión de claves.

2. Utilice la [clave de transporte recuperada](#retrieve-transport-key) del paso 2 para cifrar el material de claves.

   Al cifrar el material de claves, utilice el esquema de cifrado `RSAES_OAEP_SHA_256`. Este es el esquema predeterminado que {{site.data.keyword.keymanagementserviceshort}} utiliza para crear la clave de transporte. Para evitar problemas de descifrado en {{site.data.keyword.keymanagementserviceshort}}, no incluya el parámetro opcional `label` al ejecutar el cifrado RSAES_OAEP en el material de claves. Para aprender a ejecutar el cifrado RSA en el material de claves, consulte la documentación la documentación del HSM local o del sistema de gestión de claves.

3. Asegúrese de que el material de claves está codificado con base64 antes de ir al paso siguiente.

### Paso 4: Importar el material de claves
{: #import-root-key}

[Después de cifrar y codificar en base64 el material de claves](#encrypt-root-key), importe la clave raíz en {{site.data.keyword.keymanagementserviceshort}} realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys
```
{: codeblock}

1. Llame a la API de [{{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window} con el mandato de cURL siguiente.

    ```cURL
    curl -X POST \
      https://<región>.kms.cloud.ibm.com/api/v2/keys \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key+json' \
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
       "payload": "<material_clave>",
       "extractable": <tipo_clave>,
       "encryptionAlgorithm": "<algoritmo_cifrado>",
       "importToken": "<señal_importación>"
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
        <td><varname>material_clave</varname></td>
        <td>
          <p>Material de clave codificado en base64 como, por ejemplo, una clave para envolver claves existentes, que desee almacenar y gestionar en el servicio.</p>
          <p>Asegúrese de que el material de la clave cumple los requisitos siguientes:</p>
          <p>
            <ul>
              <li>La clave debe ser de 128, 192 o 256 bits.</li>
              <li>Los bytes de datos, por ejemplo, 32 bytes para 256 bits, se deben codificar en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <tr>
        <td><varname>tipo_clave</varname></td>
        <td>
          <p>Valor booleano que determina si el material de claves puede dejar el servicio.</p>
          <p>Cuando establece el atributo <code>extractable</code> en <code>false</code>, el servicio designa la clave como una clave raíz que se puede utilizar para <code>envolver</code> o <code>desenvolver</code> operaciones.</p>
        </td>
      </tr>
      <tr>
        <td><varname>algoritmo_cifrado</varname></td>
        <td>El esquema de cifrado que ha utilizado para <a href="#encrypt-root-key">cifrar el material de claves</a>. Actualmente, se da soporte a <code>RSAES_OAEP_SHA_256</code>. Para importar el material de clave raíz sin utilizar una clave de transporte y una señal de importación, omita el atributo <code>encryption_algorithm</code>.</td>
      </tr>
      <tr>
        <td><varname>señal_importación</varname></td>
        <td>La señal de importación que se utiliza para verificar el estado activo y la integridad de una clave de transporte. Si cifra el material de claves utilizando una clave de transporte, debe suministrar la misma señal de importación que ha recuperado en el <a href="#retrieve-transport-key">paso 2</a>. Para importar el material de clave raíz sin utilizar una clave de transporte y una señal de importación, omita el atributo <code>importToken</code>.</td>
      </tr>
        <caption style="caption-side:bottom;">Tabla 4. Describe las variables necesarias para añadir una clave raíz con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
    </table>

    Para proteger la confidencialidad de sus datos personales, evite especificar información de identificación personal (PII), como el nombre o la ubicación, cuando añades claves al servicio. Para obtener más ejemplos sobre la PII, consulte la sección 2.2 de la [NIST Special Publication 800-122 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
    {: important}

    Una respuesta `POST api/v2/keys` satisfactoria devuelve el valor del ID para la clave, junto con otros metadatos. El ID es un identificador exclusivo que se asigna a su clave y que posteriores llamadas lo utilizan para la API de {{site.data.keyword.keymanagementserviceshort}}.

2. Opcional: Verifique que la clave se ha añadido ejecutando la siguiente llamada para examinar las claves en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
      https://<región>.kms.cloud.ibm.com/api/v2/keys \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>'
    ```
    {: codeblock}

## Qué hacer a continuación
{: #import-root-key-next-steps}

- Para obtener más información sobre la protección de claves con cifrado de sobre, consulte [Claves de envolvimiento](/docs/services/key-protect?topic=key-protect-wrap-keys).
- Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

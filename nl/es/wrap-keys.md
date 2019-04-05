---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: wrap key, encrypt data encryption key, protect data encryption key, envelope encryption API examples

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

# Envolvimiento de claves
{: #wrap-keys}

Puede gestionar y proteger sus claves de cifrado con una clave raíz utilizando la API de {{site.data.keyword.keymanagementservicelong}}, si es un usuario con los privilegios necesarios.
{: shortdesc}

Cuando envuelve una clave de cifrado de datos (DEK) con una clave raíz, {{site.data.keyword.keymanagementserviceshort}} combina la robustez de varios algoritmos para proteger la privacidad y la integridad de sus datos cifrados.  

Para conocer cómo el envolvimiento de claves ayuda a controlar la seguridad de los datos en reposo en la nube, consulte [Protección de datos con cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Envolvimiento de claves utilizando la API
{: #wrap-key-api}

Proteja una clave de cifrado de datos (DEK) específica con una clave raíz que gestionará en {{site.data.keyword.keymanagementserviceshort}}.

Cuando proporcione una clave raíz para el envolvimiento, asegúrese de que la clave raíz es de 128, 192 o 256 bits para que la llamada de envolvimiento sea satisfactoria. Si crea una clave raíz en el servicio, {{site.data.keyword.keymanagementserviceshort}} genera una clave de 256 bits a partir de sus HSM, soportada por el algoritmo AES-CGM.
{: note}

[Después de designar una clave raíz en el servicio](/docs/services/key-protect?topic=key-protect-create-root-keys), puede envolver una DEK con cifrado avanzado realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=wrap
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copie el material de la clave de la DEK que desea gestionar y proteger.

    Si tiene privilegios de gestor o escritor para su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}, [puede recuperar el material de una clave específica realizando una solicitud `GET /v2/keys/<key_ID>`](/docs/services/key-protect?topic=key-protect-view-keys#api).

3. Copie el ID de la clave raíz que desea utilizar para envolver.

4. Ejecute el siguiente mandato cURL para proteger la clave con una operación de envolvimiento.

    ```cURL
    curl -X POST \
      'https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=wrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <ID_correlación>' \
      -d '{
      "plaintext": "<clave_datos>",
      "aad": ["<datos_adicionales>", "<datos_adicionales>"]
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
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> Identificador exclusivo para la clave raíz que desea utilizar para envolver.</td>
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
        <td><varname>clave_datos</varname></td>
        <td>El material de la clave de la DEK que desea gestionar y proteger. El valor del <code>texto sin formato</code> debe estar codificado en base64. Para generar una nueva DEK, omita el atributo <code>plaintext</code>. El servicio genera un texto sin formato aleatorio (32 bytes), envuelve dicho valor y devuelve tanto el valor generado como el envuelto en la respuesta.</td>
      </tr>
      <tr>
        <td><varname>datos_adicionales</varname></td>
        <td>Datos de autenticación adicionales (AAD) que se utilizan para proteger aún más la clave. Cada serie puede contener hasta 255 caracteres. Si proporcionó los AAD cuando realizó al servicio la llamada de envolvimiento, debe especificar los mismos AAD durante la llamada de desenvolvimiento subsiguiente.<br></br>Importante: El servicio {{site.data.keyword.keymanagementserviceshort}} no guarda datos de autenticación adicionales. Si proporciona AAD, guarde los datos en una ubicación segura para asegurarse de que pueda acceder y proporcionar los mismos AAD durante las llamadas de desenvolvimiento subsiguientes.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para envolver una clave especificada en {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    La clave de cifrado de datos envuelta, con el material de la clave codificado en base64, se devuelve en el cuerpo de entidad de la respuesta. El siguiente objeto JSON muestra un valor devuelto de ejemplo.

    ```
    {
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}
    
    Si omite el atributo `plaintext` cuando realice la solicitud de envolvimiento, el servicio devolverá tanto la clave de cifrado de datos (DEK) generada como la DEK envuelta en formato con codificación base64.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg==",
      "ciphertext": "eyJjaXBoZXJ0ZXh0Ijoic3VLSDNRcmdEZjdOZUw4Rkc4L2FKYjFPTWcyd3A2eDFvZlA4MEc0Z1B2RmNrV2g3cUlidHphYXU0eHpKWWoxZyIsImhhc2giOiJiMmUyODdkZDBhZTAwZGZlY2Q3OGJmMDUxYmNmZGEyNWJkNGUzMjBkYjBhN2FjNzVhMWYzZmNkMDZlMjAzZWYxNWM5MTY4N2JhODg2ZWRjZGE2YWVlMzFjYzk2MjNkNjA5YTRkZWNkN2E5Y2U3ZDc5ZTRhZGY1MWUyNWFhYWM5MjhhNzg3NmZjYjM2NDFjNTQzMTZjMjMwOGY2MThlZGM2OTE3MjAyYjA5YTdjMjA2YzkxNTBhOTk1NmUxYzcxMTZhYjZmNmQyYTQ4MzZiZTM0NTk0Y2IwNzJmY2RmYTk2ZSJ9"
    }
    ```
    {:screen}

    El valor <code>plaintext</code> representa la DEK desenvuelta y el valor <code>ciphertext</code> representa la DEK envuelta.
    
    Si desea que {{site.data.keyword.keymanagementserviceshort}} genere una nueva clave de cifrado de datos (DEK) por usted, también puede pasar un cuerpo vacío en una solicitud de envolvimiento. La DEK generada, que contiene el material de claves con codificación base64, se devolverá en la respuesta de cuerpo de entidad, junto con la DEK empaquetada.
    {: tip}
    

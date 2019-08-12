---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: unwrap key, decrypt key, decrypt data encryption key, access data encryption key, envelope encryption API examples

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

# Desenvolvimiento de claves
{: #unwrap-keys}

Las claves de cifrado de datos (DEK) se desenvuelven para acceder a sus contenidos mediante la API de {{site.data.keyword.keymanagementservicefull}}. El proceso de desenvolvimiento descifra y comprueba la integridad y el contenido de las DEK, devolviendo el material original de la clave a su servicio de datos de {{site.data.keyword.cloud_notm}}.
{: shortdesc}

Para conocer cómo el envolvimiento de claves ayuda a controlar la seguridad de los datos en reposo en la nube, consulte [Protección de datos con cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## Desenvolvimiento de claves utilizando la API
{: #unwrap-key-api}

[Después de realizar al servicio una llamada de envolvimiento](/docs/services/key-protect?topic=key-protect-wrap-keys), puede desenvolver una clave de cifrado de datos (DEK) específica para acceder a su contenido realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=unwrap
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio](/docs/services/key-protect?topic=key-protect-set-up-api).

2. Copie el ID de la clave raíz que ha utilizado para realizar la solicitud inicial de envolvimiento.

    Puede recuperar el ID de una clave realizando una solicitud `GET /v2/keys` o visualizando sus claves en la interfaz gráfica de usuario de {{site.data.keyword.keymanagementserviceshort}}.

3. Copie el valor de `ciphertext` devuelto durante la solicitud inicial de envolvimiento.

4. Ejecute el siguiente mandato cURL para descifrar y autenticar el material de la clave.

    ```cURL
    curl -X POST \
      'https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=unwrap' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -H 'correlation-id: <ID_correlación>' \
      -d '{
      "ciphertext": "<clave_datos_cifrados>",
      "aad": ["<datos_adicionales>", "<datos_adicionales>"]
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
        <td><strong>Obligatorio.</strong> La abreviatura de región, como <code>us-south</code> o <code>eu-gb</code>, que representa el área geográfica donde reside su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-regions#service-endpoints">Puntos finales de servicio regionales</a>.</td>
      </tr>
      <tr>
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> El identificador único de la clave raíz que ha utilizado para la solicitud inicial de envolvimiento.</td>
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
        <td><varname>datos_adicionales</varname></td>
        <td>Datos de autenticación adicionales (AAD) que se utilizan para proteger aún más la clave. Cada serie puede contener hasta 255 caracteres. Si proporcionó los AAD cuando realizó al servicio la llamada de envolvimiento, debe especificar los mismos AAD durante la llamada de desenvolvimiento.</td>
      </tr>
      <tr>
        <td><varname>claves_datos_cifrados</varname></td>
        <td><strong>Obligatorio.</strong> El valor <code>ciphertext</code> devuelto durante la operación de envolvimiento.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para desenvolver las claves de {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    El material original de la clave se devuelve en el cuerpo de entidad de la respuesta. El siguiente objeto JSON muestra un valor devuelto de ejemplo.

    ```
    {
      "plaintext": "VGhpcyBpcyBhIHNlY3JldCBtZXNzYWdlLg=="
    }
    ```
    {:screen}

    Si {{site.data.keyword.keymanagementserviceshort}} detecta que ha rotado la clave raíz que se utiliza para desenvolver y acceder a los datos, el servicio también devuelve una clave de cifrado de datos recién envuelta (`ciphertext`) en el cuerpo de respuesta desenvuelto. Almacene y utilice el nuevo valor `ciphertext` para futuras operaciones de cifrado de sobre, de modo que los datos queden protegidos por la clave raíz más reciente.
    {: note}

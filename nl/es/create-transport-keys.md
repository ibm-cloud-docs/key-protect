---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: create transport encryption key, secure import, key-wrapping key, transport key API examples

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

# Creación de una clave de transporte
{: #create-transport-keys}

Puede habilitar la importación segura de material de claves raíz a la nube creando primero una clave de cifrado de transporte para su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.
{: shortdesc}

Las claves de transporte se utilizan para cifrar e importar de forma segura material de claves raíz en {{site.data.keyword.keymanagementserviceshort}} en base a las políticas que especifique. Para obtener más información sobre cómo importar de forma segura las claves a la nube, consulte [Cómo traer las claves de cifrado a la nube](/docs/services/key-protect/concepts?topic=key-protect-importing-keys).

Actualmente, las claves de transporte son una característica beta. Las características beta pueden cambiar en cualquier momento, y las actualizaciones futuras pueden introducir cambios incompatibles con la última versión.
{: important}

## Creación de una clave de transporte con la API
{: #create-transport-key-api}

Cree una clave de transporte que esté asociada a la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} realizando una llamada `POST` al siguiente punto final.

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
          <td>El número de veces que se puede recuperar una clave de transporte dentro de su hora de caducidad hasta que ya no se pueda acceder a ella. El valor predeterminado es 1.</td>
        </tr>
          <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para añadir una clave raíz con la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
      </table>

    Una solicitud satisfactoria de `POST api/v2/lockers` crea una clave de transporte para la instancia de servicio y devuelve su valor de ID, junto con otros metadatos. El ID es un identificador exclusivo que se asocia a la clave de transporte y que posteriores llamadas lo utilizan para la API de {{site.data.keyword.keymanagementserviceshort}}.

3. Opcional: Verifique que se ha creado la clave de transporte ejecutando la siguiente llamada para recuperar metadatos sobre la instancia de servicio.

    ```cURL
    curl -X GET \
      https://<región>.kms.cloud.ibm.com/api/v2/lockers \
      -H 'accept: application/vnd.ibm.collection+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>'
    ```
    {: codeblock}

## Qué hacer a continuación
{: #create-transport-key-next-steps}

- Para obtener más información sobre el uso de una clave de transporte para importar claves raíz en el servicio, consulte [Importación de claves raíz](/docs/services/key-protect?topic=key-protect-import-root-keys).
- Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

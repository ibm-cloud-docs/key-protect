---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: rotate encryption key, encryption key rotation, rotate key API examples 

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

# Rotación de claves bajo demanda
{: #rotate-keys}

Puede rotar las claves raíz bajo demanda utilizando {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Cuando se rota la clave raíz, se acorta el tiempo de vida de la clave y se limita la cantidad de información que protege dicha clave.   

Para saber cómo la rotación de claves le ayuda a cumplir los estándares del sector y las mejores prácticas de cifrado, consulte [Rotación de las claves de cifrado](/docs/services/key-protect?topic=key-protect-key-rotation).

La rotación solo está disponible para las claves raíz. Para obtener más información acerca de las opciones de rotación claves en {{site.data.keyword.keymanagementserviceshort}}, consulte [Comparación de las opciones de rotación de claves](/docs/services/key-protect?topic=key-protect-compare-key-rotation-options).
{: note}

## Rotación de claves raíz con la GUI
{: #rotate-key-gui}

Si prefiere rotar sus claves raíz utilizando una interfaz gráfica, puede utilizar la GUI de {{site.data.keyword.keymanagementserviceshort}}.

[Después de crear o importar sus claves raíz existentes en el servicio](/docs/services/key-protect?topic=key-protect-create-root-keys), complete los siguientes pasos para rotar una clave:

1. [Inicie sesión en la consola de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/){: new_window}.
2. Vaya a **Menú** &gt; **Lista de recursos** para ver una lista de sus recursos.
3. Desde la lista de recursos de {{site.data.keyword.cloud_notm}} seleccione su instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.
4. En la página de detalles de la aplicación, utilice la tabla de **Claves** tabla para examinar las claves en el servicio.
5. Pulse el icono ⋯ para abrir una lista de opciones para la clave que desea rotar.
6. En el menú de opciones, pulse **Rotar clave** y confirme la rotación en la pantalla siguiente.

Si inicialmente ha importado la clave raíz, debe proporcionar nuevo material de clave con codificación base64 para rotar la clave. Para obtener más información, consulte [Importación de claves raíz con la interfaz gráfica de usuario](/docs/services/key-protect?topic=key-protect-import-root-keys#gui).
{: note}

## Rotación de raíz claves utilizando la API
{: #rotate-key-api}

[Después de designar una clave raíz en el servicio](/docs/services/key-protect?topic=key-protect-create-root-keys), puede rotar la clave realizando una llamada `POST` al siguiente punto final.

```
https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=rotate
```
{: codeblock}

1. [Recupere sus credenciales de servicio y de autenticación para trabajar con claves en el servicio.](/docs/services/key-protect?topic=key-protect-set-up-api)

2. Copie el ID de la clave raíz que desea rotar.

3. Ejecute el siguiente mandato cURL para sustituir la clave con nuevo material.

    ```cURL
    curl -X POST \
      'https://<región>.kms.cloud.ibm.com/api/v2/keys/<ID_clave>?action=rotate' \
      -H 'accept: application/vnd.ibm.kms.key_action+json' \
      -H 'authorization: Bearer <señal_IAM>' \
      -H 'bluemix-instance: <ID_instancia>' \
      -H 'content-type: application/vnd.ibm.kms.key_action+json' \
      -d '{
        'payload: <material_clave>'
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
        <td><varname>ID_clave</varname></td>
        <td><strong>Obligatorio.</strong> Identificador exclusivo para la clave raíz que desea rotar.</td>
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
        <td><varname>material_clave</varname></td>
        <td>
          <p>Material de clave codificado en base64 que desea almacenar y gestionar en el servicio. Este valor es necesario si inicialmente importó el material de claves cuando añadió la clave al servicio.</p>
          <p>Para rotar una clave que {{site.data.keyword.keymanagementserviceshort}} generó inicialmente, omita el atributo <code>payload</code> y pase un cuerpo de entidad de solicitud vacía. Para rotar una clave importada, proporcione un material de claves que cumpla los siguientes requisitos:</p>
          <p>
            <ul>
              <li>La clave debe ser de 128, 192 o 256 bits.</li>
              <li>Los bytes de datos, por ejemplo, 32 bytes para 256 bits, se deben codificar en base64.</li>
            </ul>
          </p>
        </td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Describe las variables necesarias para rotar una clave especificada en {{site.data.keyword.keymanagementserviceshort}}.</caption>
    </table>

    Una solicitud de rotación correcta devuelve una respuesta HTTP `204 No Content`, que indica que la clave raíz se ha sustituido por material de claves nuevo.

4. Opcional: Verifique que la clave se ha rotado ejecutando la siguiente llamada para examinar las claves en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    ```cURL
    curl -X GET \
    https://<región>.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <señal_IAM>' \
    -H 'bluemix-instance: <ID_instancia>'
    ```
    {: codeblock}
  
    Revise el valor `lastRotateDate` en el cuerpo de entidad de respuesta para comprobar la fecha y hora en que se rotó la clave por última vez.
    

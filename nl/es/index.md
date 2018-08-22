---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Iniciación a {{site.data.keyword.keymanagementserviceshort}}

{{site.data.keyword.keymanagementservicefull}} le ayuda a suministrar claves cifradas para apps en servicios de {{site.data.keyword.cloud_notm}}. Esta guía de aprendizaje muestra cómo crear y añadir claves criptográficas existentes utilizando el panel de control de {{site.data.keyword.keymanagementserviceshort}}, de modo que pueda gestionar el cifrado de datos desde una ubicación central.
{: shortdesc}

## Iniciación a las claves de cifrado
{: #get_started_keys}

Desde el panel de control de {{site.data.keyword.keymanagementserviceshort}}, puede crear nuevas claves para el cifrado o importar sus claves. 

Elija entre dos tipos de clave:

<dl>
  <dt>Claves raíz</dt>
    <dd>Las claves raíz son claves que se utilizan para envolver otras claves y que gestiona en su totalidad con {{site.data.keyword.keymanagementserviceshort}}. Una clave raíz sirve para proteger otras claves criptográficas con cifrado avanzado.</dd>
  <dt>Claves estándar</dt>
    <dd>Las claves estándar son claves simétricas que se utilizan para la criptografía. Puede utilizar una clave estándar para cifrar y descifrar datos directamente.</dd>
</dl>

## Creación de nuevas claves
{: #creating_keys}

[Después de crear una instancia de {{site.data.keyword.keymanagementserviceshort}}](https://console.ng.bluemix.net/catalog/services/key-protect/?taxonomyNavigation=apps), estará preparado para designar claves en el servicio. 

Siga estos pasos para crear su primera clave criptográfica. 

1. En el panel de control de {{site.data.keyword.keymanagementserviceshort}}, pulse **Gestionar** &gt; **Añadir clave**.
2. Para crear una nueva clave, seleccione la ventana **Generar una nueva clave**.

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
        <td>[Tipo de clave](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Descripción de los valores de Generar nueva clave</caption>
    </table>

3. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Generar clave** para confirmar. 

Las claves creadas en el servicio son claves simétricas de 256 bits, soportadas por el algoritmo AES-GCM. Para una mayor seguridad, las claves se generan con módulos de seguridad de hardware (HSM) con certificación FIPS 140-2 Nivel 2 que se ubican en centros de datos seguros de {{site.data.keyword.cloud_notm}}. 

## Adición de claves existentes
{: #adding_keys}

Puede obtener las ventajas que ofrece Bring Your Own Key (BYOK) introduciendo las claves existentes en el servicio. 

Siga estos pasos para añadir una clave existente.

1. En el panel de control de {{site.data.keyword.keymanagementserviceshort}}, pulse **Gestionar** &gt; **Añadir clave**.
2. Para subir una clave existente, seleccione la ventana **Especificar clave existente**.

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
        <td>[Tipo de clave](/docs/services/keymgmt/concepts/keyprotect_envelope.html#key_types) que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <tr>
        <td>Material de la clave</td>
        <td>Solo es necesario si está añadiendo una clave existente. El material de la clave puede ser cualquier tipo de datos, como por ejemplo una clave simétrica, que desee almacenar en el servicio de {{site.data.keyword.keymanagementserviceshort}}. La clave que proporcione debe estar codificada en base64.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 2. Descripción de los valores de Especificar clave existente</caption>
    </table>

3. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Añadir una nueva clave** para confirmar. 

Desde el panel de control {{site.data.keyword.keymanagementserviceshort}}, puede inspeccionar las características generales de sus nuevas claves. 

## Qué hacer a continuación

Ahora puede utilizar sus claves para codificar sus apps y servicios.

- Para saber más sobre de sobre cómo gestionar sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} para ver ejemplos de código ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/apidocs/639){: new_window}.
- Para ver un ejemplo de cómo se almacenan las claves en {{site.data.keyword.keymanagementserviceshort}} para cifrar y descifrar datos [consulte la app de ejemplo en GitHub ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}.
- Para obtener más información sobre la integración del servicio {{site.data.keyword.keymanagementserviceshort}} con otras soluciones de datos en la nube, [consulte el documento de Integraciones](/docs/services/keymgmt/keyprotect_integration.html).

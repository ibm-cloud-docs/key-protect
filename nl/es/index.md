---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: key management service, kms, manage encryption keys, data encryption, data-at-rest, protect data encryption keys

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

# Guía de aprendizaje de iniciación
{: #getting-started-tutorial}

{{site.data.keyword.keymanagementservicefull}} le ayuda a suministrar claves cifradas para apps en servicios de {{site.data.keyword.cloud_notm}}. Esta guía de aprendizaje muestra cómo crear y añadir claves criptográficas existentes utilizando el panel de control de {{site.data.keyword.keymanagementserviceshort}}, de modo que pueda gestionar el cifrado de datos desde una ubicación central.
{: shortdesc}

## Iniciación a las claves de cifrado
{: #get-started-keys}

Desde el panel de control de {{site.data.keyword.keymanagementserviceshort}}, puede crear nuevas claves para el cifrado o importar sus claves. 

Elija entre dos tipos de clave:

<dl>
  <dt>Claves raíz</dt>
    <dd>Las claves raíz son claves que se utilizan para envolver otras claves y que gestiona en su totalidad con {{site.data.keyword.keymanagementserviceshort}}. Una clave raíz sirve para proteger otras claves criptográficas con cifrado avanzado. Para obtener más información, consulte <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption">Protección de datos con cifrado de sobre</a>.</dd>
  <dt>Claves estándar</dt>
    <dd>Las claves estándar son claves simétricas que se utilizan para la criptografía. Puede utilizar una clave estándar para cifrar y descifrar datos directamente.</dd>
</dl>

## Creación de nuevas claves
{: #create-keys}

[Después de crear una instancia de {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect?taxonomyNavigation=apps), estará preparado para designar claves en el servicio. 

Siga estos pasos para crear su primera clave criptográfica. 

1. En la página de detalles de la aplicación, pulse **Gestionar** &gt; **Añadir clave**.
2. Para crear una nueva clave, seleccione la ventana **Crear una clave**.

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
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Tipo de clave</a> que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 1. Descripción de los valores de <b>Crear una clave</b></caption>
    </table>

3. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Crear una clave** para confirmar. 

Las claves creadas en el servicio son claves simétricas de 256 bits, soportadas por el algoritmo AES-CBC-PAD. Para una mayor seguridad, las claves se generan con módulos de seguridad de hardware (HSM) con certificación FIPS 140-2 Nivel 3 que se ubican en centros de datos seguros de {{site.data.keyword.cloud_notm}}. 

## Importación de sus propias claves
{: #import-keys}

Puede obtener las ventajas que ofrece Bring Your Own Key (BYOK) introduciendo las claves existentes en el servicio. 

Siga estos pasos para añadir una clave existente.

1. En la página de detalles de la aplicación, pulse **Gestionar** &gt; **Añadir clave**.
2. Para subir una clave existente, seleccione la ventana **Importar su propia clave**.

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
        <td><a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">Tipo de clave</a> que desea gestionar en {{site.data.keyword.keymanagementserviceshort}}.</td>
      </tr>
      <tr>
        <td>Material de la clave</td>
        <td>El material de la clave, como por ejemplo una clave simétrica, que desea almacenar en el servicio de {{site.data.keyword.keymanagementserviceshort}}. La clave que proporcione debe estar codificada en base64.</td>
      </tr>
      <caption style="caption-side:bottom;">Tabla 2. Descripción de los valores de <b>Importar su propia clave</b></caption>
    </table>

3. Cuando haya terminado de cumplimentar los detalles de la clave, pulse **Importar clave** para confirmar. 

Desde el panel de control {{site.data.keyword.keymanagementserviceshort}}, puede inspeccionar las características generales de sus nuevas claves. 

## Qué hacer a continuación
{: #get-started-next-steps}

Ahora puede utilizar sus claves para codificar las apps y servicios. Si ha añadido una clave raíz al servicio, es posible que desee obtener más información sobre el uso de la clave raíz para proteger las claves que cifran los datos en reposo. Consulte [Envolvimiento de claves](/docs/services/key-protect?topic=key-protect-wrap-keys) para empezar.

- Para encontrar más información sobre la gestión y la protección de las claves de cifrado con una clave raíz, consulte [Protección de datos con cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption).
- Para obtener más información sobre la integración del servicio {{site.data.keyword.keymanagementserviceshort}} con otras soluciones de datos en la nube, [consulte el documento de Integraciones](/docs/services/key-protect?topic=key-protect-integrate-services).
- Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}}](https://cloud.ibm.com/apidocs/key-protect){: external}.

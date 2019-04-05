---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: import encryption key, upload encryption key, Bring Your Own Key, BYOK, secure import, transport encryption key 

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

# Cómo traer las claves de cifrado a la nube
{: #importing-keys}

Las claves de cifrado contienen subconjuntos de información, como por ejemplo los metadatos que le ayudan a identificar la clave, y el _material de la clave_ que se utiliza para cifrar y descifrar datos.
{: shortdesc}

Cuando utiliza {{site.data.keyword.keymanagementserviceshort}} para crear claves, el servicio genera un material de claves de cifrado en su nombre que tiene su raíz en los módulos de seguridad de hardware (HSM) basados en la nube. Pero según los requisitos empresariales, es posible que tenga que generar material clave a partir de la solución interna y, a continuación, ampliar la infraestructura de gestión de claves local a la nube importando claves en {{site.data.keyword.keymanagementserviceshort}}.

<table>
  <th>Ventajas</th>
  <th>Descripción</th>
  <tr>
    <td>Traiga sus propias claves (BYOK) </td>
    <td>Desea reforzar y controlar completamente las prácticas de gestión de claves mediante la generación de claves robustas desde el módulo de seguridad de hardware local (HSM). Si opta por exportar claves simétricas desde la infraestructura de gestión de claves internas, puede utilizar {{site.data.keyword.keymanagementserviceshort}} para llevarlos a la nube de forma segura.</td>
  </tr>
  <tr>
    <td>Importación segura de material de claves raíz</td>
    <td>Cuando exporta las claves a la nube, desea asegurarse de que el material de claves esté protegido mientras está en tránsito. Puede mitigar los ataques de intermediario <a href="#transport-keys">utilizando una clave de transporte</a> para importar de forma segura material de claves en la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</td>
  </tr>
  <caption style="caption-side:bottom;">Tabla 1. Describe los beneficios de importar material de claves</caption>
</table>


## Planificación anticipada para importar material de claves
{: #plan-ahead}

Tenga presentes las siguientes consideraciones cuando esté listo para importar el material de claves raíz al servicio.

<dl>
  <dt>Revise las opciones para crear material de claves</dt>
    <dd>Explore las opciones para crear claves de cifrado simétricas de 256 bits según sus necesidades de seguridad. Por ejemplo, puede utilizar el sistema de gestión de claves internas, respaldado por un módulo de seguridad de hardware (HSM), validado por FIPS, para generar el material de claves antes de traer claves a la nube. Si va a crear una prueba de concepto, también puede utilizar un kit de herramientas de criptografía, como por ejemplo <a href="https://www.openssl.org/" target="_blank">OpenSSL</a> para generar material de claves que pueda importar en {{site.data.keyword.keymanagementserviceshort}} para sus necesidades de realización de pruebas.</dd>
  <dt>Elija una opción para importar material de claves en {{site.data.keyword.keymanagementserviceshort}}</dt>
    <dd>Elija entre dos opciones para importar claves raíz en función del nivel de seguridad necesario para el entorno o para la carga de trabajo. De forma predeterminada, {{site.data.keyword.keymanagementserviceshort}} cifra el material de claves mientras se encuentra en tránsito utilizando el protocolo TLS 1.2 (Transport Layer Security). Si está creando una prueba de concepto o si está probando el servicio por primera vez, puede importar el material de claves raíz en {{site.data.keyword.keymanagementserviceshort}} utilizando esta opción predeterminada. Si la carga de trabajo requiere un mecanismo de seguridad superior a TLS, también puede <a href="#transport-keys">utilizar una clave de transporte</a> para cifrar e importar el material de clave raíz en el servicio.</dd>
  <dt>Planifique de forma anticipada la importación de material de claves</dt>
    <dd>Si opta por cifrar el material de claves utilizando una clave de transporte, determine un método para ejecutar el cifrado RSA en el material de claves. Debe utilizar el esquema de cifrado <code>RSAES_OAEP_SHA_256</code> tal como se especifica en el <a href="https://tools.ietf.org/html/rfc3447" target="_blank">estándar PKCS #1 v2.1 para el cifrado RSA</a>. Revise las posibilidades del sistema de gestión de claves interno o de los HSM locales para determinar las opciones de que dispone.</dd>
  <dt>Gestione el ciclo de vida del material de claves importado</dt>
    <dd>Después de importar material de claves en el servicio, tenga en cuenta es su responsabilidad gestionar el ciclo de vida completo de la clave. Al utilizar la API de {{site.data.keyword.keymanagementserviceshort}}, puede establecer una fecha de caducidad para la clave cuando decida cargarla en el servicio. No obstante, si desea <a href="/docs/services/key-protect?topic=key-protect-rotate-keys">rotar una clave raíz importada</a>, debe generar y proporcionar nuevo material de clave para retirar y sustituir la clave existente. </dd>
</dl>

## Utilización de claves de transporte
{: #transport-keys}

Actualmente, las claves de transporte son una característica beta. Las características beta pueden cambiar en cualquier momento, y las actualizaciones futuras pueden introducir cambios incompatibles con la última versión.
{: important}

Si desea cifrar el material de claves antes de importarlo en {{site.data.keyword.keymanagementserviceshort}}, puede crear una clave de cifrado de transporte para la instancia de servicio utilizando la API de {{site.data.keyword.keymanagementserviceshort}}. 

Las claves de transporte son un tipo de recurso en {{site.data.keyword.keymanagementserviceshort}} que habilita la importación segura de material de claves raíz en la instancia de servicio. Al utilizar una clave de transporte para cifrar el material de claves en local, se protegen las claves raíz mientras están en tránsito hacia {{site.data.keyword.keymanagementserviceshort}} según las políticas que especifique. Por ejemplo, puede establecer una política en la clave de transporte que limite el uso de la clave basándose en la hora y el recuento de uso.

### Cómo funciona
{: #how-transport-keys-work}

Cuando se [crea una clave de transporte](/docs/services/key-protect?topic=key-protect-create-transport-keys) para la instancia de servicio, {{site.data.keyword.keymanagementserviceshort}} genera una clave RSA de 4096 bits. El servicio cifra la clave privada y, a continuación, devuelve la clave pública y una señal de importación que puede utilizar para cifrar e importar el material de claves raíz. 

Cuando esté listo para [importar una clave raíz](/docs/services/key-protect?topic=key-protect-import-root-keys#api) en {{site.data.keyword.keymanagementserviceshort}}, proporcione el material de la clave raíz cifrado y la señal de importación que se utiliza para verificar la integridad de la clave pública. Para completar la solicitud, {{site.data.keyword.keymanagementserviceshort}} utiliza la clave privada asociada a la instancia de servicio para descifrar el material de la clave raíz cifrado. Este proceso garantiza que únicamente la clave de transporte que ha generado en {{site.data.keyword.keymanagementserviceshort}} puede descifrar el material de clave que importa y almacena en el servicio.

Sólo puede crear una clave de transporte por instancia de servicio. Para obtener más información sobre los límites de recuperación de las claves de transporte, [consulte el documento de referencia de API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.
{: note} 

### Métodos de API
{: #secure-import-api-methods}

De forma transparente para el usuario, la API de {{site.data.keyword.keymanagementserviceshort}} controla el proceso de creación de las claves de transporte.  

En la tabla siguiente se listan los métodos API que configuran un bloqueador y crean claves de transporte para la instancia de servicio.

<table>
  <tr>
    <th>Método</th>
    <th>Descripción</th>
  </tr>
  <tr>
    <td><code>POST api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Crear una clave de transporte</a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-create-transport-keys">Recuperar metadatos de la clave de transporte </a></td>
  </tr>
  <tr>
    <td><code>GET api/v2/lockers/{id}</code></td>
    <td><a href="/docs/services/key-protect?topic=key-protect-import-root-keys">Recuperar una clave de transporte y una señal de importación</a></td>
  </tr>
  <caption style="caption-side:bottom;">Tabla 2. Describe los métodos API de {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Para obtener más información sobre la gestión de claves mediante programación en {{site.data.keyword.keymanagementserviceshort}}, consulte la [documentación de consulta de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

## Qué hacer a continuación

- Para obtener información sobre cómo crear una clave de transporte para la instancia de servicio, consulte [Creación de una clave de transporte](/docs/services/key-protect?topic=key-protect-create-transport-keys).
- Para obtener más información sobre cómo importar claves al servicio, consulte [Importación de claves raíz](/docs/services/key-protect?topic=key-protect-import-root-keys). 

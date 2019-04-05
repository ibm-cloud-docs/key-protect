---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: envelope encryption, key name, create key in different region, delete service instance

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
{:faq: data-hd-content-type='faq'}

# Preguntas frecuentes
{: #faqs}

Utilice las siguientes preguntas más frecuentes (FAQ) como ayuda para {{site.data.keyword.keymanagementservicelong}}.

## ¿Cómo funciona la fijación de precios para {{site.data.keyword.keymanagementserviceshort}}?
{: #how-does-pricing-work}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} ofrece un [plan de niveles graduado](https://{DomainName}/catalog/services/key-protect) gratuito para usuarios que necesiten 20 claves o menos. Puede tener 20 claves gratuitamente por cada cuenta de {{site.data.keyword.cloud_notm}}. Si su equipo necesita varias instancias de {{site.data.keyword.keymanagementserviceshort}}, {{site.data.keyword.cloud_notm}} añade las claves activas en todas las instancias dentro de la cuenta y luego aplica los precios. 

## ¿Qué es una clave de cifrado activa?
{: #what-is-active-encryption-key}
{: faq}

Al importar claves de cifrado en {{site.data.keyword.keymanagementserviceshort}}, o cuando se utiliza {{site.data.keyword.keymanagementserviceshort}} para generar claves a partir de sus HSM, estas claves pasan a ser claves _activas_. Las tarifas se basan en todas las claves activas dentro de una cuenta de {{site.data.keyword.cloud_notm}}. 

## ¿cómo debo agrupar y gestionar las claves?
{: #how-to-group-keys}
{: faq}

Desde un punto de vista de precios, la mejor forma de utilizar {{site.data.keyword.keymanagementserviceshort}} es crear un número limitado de claves raíz y utilizarlas para cifrar las claves de cifrado de datos creadas por una app externa o un servicio de datos en la nube. 

Para obtener más información sobre cómo utilizar las claves raíz para proteger las claves de cifrado de datos, consulte [Protección de datos con cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption).

## ¿Qué es una clave raíz?
{: #what-is-root-key}
{: faq}

Las claves raíz son recursos primarios en {{site.data.keyword.keymanagementserviceshort}}. Son claves para envolver claves simétricas que se utilizan como claves raíz de confianza para proteger a otras claves que están almacenadas en un servicio de datos con [cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption). Con {{site.data.keyword.keymanagementserviceshort}}, puede crear, almacenar y gestionar el ciclo de vida de las claves raíz para obtener un control total de otras claves almacenadas en la nube. 

## ¿Qué es el cifrado de sobre?
{: #what-is-envelope-encryption}
{: faq}

El cifrado de sobre es la práctica de cifrar datos con una _clave de cifrado de datos_, y, a continuación, cifrar dicha clave con una _clave para envolver claves_ altamente segura.  Sus datos están protegidos en reposo mediante la aplicación de varias capas de cifrado. Para aprender a habilitar el cifrado de sobres para sus recursos de {{site.data.keyword.cloud_notm}}, consulte [Integración de servicios](/docs/services/key-protect?topic=key-protect-integrate-services).

## ¿Cuál es la longitud máxima de un nombre de clave?
{: #key-names}
{: faq}

Puede utilizar un nombre de clave con una longitud de hasta 90 caracteres.

## ¿Puedo guardar información personal como metadatos de mis claves?
{: #personal-data}
{: faq}

Para proteger la confidencialidad de sus datos personales, no almacene información de identificación personal (PII) como metadatos de sus claves. La información personal incluye nombre, dirección, número de teléfono, dirección de correo electrónico u otra información que permita identificar, ponerse en contacto o localizar a usted, a sus clientes o a cualquier otra persona.

Es su responsabilidad garantizar la seguridad de cualquier información que almacene como metadatos de los recursos y las claves de cifrado de {{site.data.keyword.keymanagementserviceshort}}. Para obtener más ejemplos de datos personales, consulte la sección 2.2 de la publicación [NIST Special Publication 800-122 ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-122.pdf){: new_window}.
{: important}

## ¿Las claves que se crean en una región se pueden utilizar en otra región?
{: #keys-across-regions}
{: faq}

Las claves de cifrado están limitadas a la región donde se han creado. {{site.data.keyword.keymanagementserviceshort}} no copia ni exporta claves de cifrado a otras regiones.

## ¿Cómo puedo controlar quién tiene acceso a las claves?
{: #access-control}
{: faq}

{{site.data.keyword.keymanagementserviceshort}} da soporte a un sistema de control de acceso centralizado, gobernado por {{site.data.keyword.iamlong}}, para ayudarle a gestionar usuarios y acceder a sus claves de cifrado. Si es un administrador de seguridad de su servicio, puede asignar [roles de Cloud IAM que correspondan a los permisos de {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-manage-access#roles) específicos que desea otorgar a los miembros de su equipo.

## ¿Cómo puedo supervisar llamadas de API a {{site.data.keyword.keymanagementserviceshort}}?
{: faq}

Puede utilizar el servicio {{site.data.keyword.cloudaccesstrailfull_notm}} para realizar un seguimiento de cómo los usuarios y las aplicaciones interactúan con la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. Por ejemplo, al crear, importar, suprimir o leer una clave en {{site.data.keyword.keymanagementserviceshort}}, se genera un suceso de {{site.data.keyword.cloudaccesstrailshort}}. Estos sucesos se reenvían automáticamente al servicio de {{site.data.keyword.cloudaccesstrailshort}} en la misma región donde se proporciona el servicio de {{site.data.keyword.keymanagementserviceshort}}.

Para obtener más información, consulte [Sucesos de Activity Tracker](/docs/services/key-protect?topic=key-protect-activity-tracker-events).

## ¿Qué sucede cuando suprimo una clave?
{: #key-destruction}
{: faq}

Cuando se suprime una clave, el servicio marca la clave como suprimida y la clave pasa al estado _Destruida_. Las claves que están en este estado ya no son recuperables, y los servicios de nube que utilizan la clave ya no pueden descifrar los datos asociados a la clave. Los datos siguen estando en estos servicios de forma encriptada. Los metadatos asociados con la clave como, por ejemplo, el nombre y el historial de transiciones de la clave, se mantienen en la base de datos de {{site.data.keyword.keymanagementserviceshort}}. 

Antes de suprimir una clave, asegúrese de que ya no necesita acceder a ninguno de los datos asociados con la clave. Esta acción no se puede deshacer.

## ¿Qué ocurre cuando tengo que anular el suministro a mi instancia de servicio?
{: #deprovision-service}
{: faq}

Si decide pasar a una versión superior de {{site.data.keyword.keymanagementserviceshort}}, debe suprimir las claves restantes de la instancia de servicio antes de poder dejar de suministrar el servicio. Una vez suprimida la instancia de servicio, {{site.data.keyword.keymanagementserviceshort}} utiliza el [cifrado de sobre](/docs/services/key-protect?topic=key-protect-envelope-encryption) para destruir criptográficamente todos los datos asociados a la instancia de servicio. 

---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: set up API, use Key Protect API, use KMS API, access Key Protect API, access KMS API

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

# Configuración de la API
{: #set-up-api}

{{site.data.keyword.keymanagementservicefull}} proporciona una API REST que puede utilizarse con cualquier lenguaje de programación para almacenar, recuperar y generar claves de cifrado.
{: shortdesc}

## Recuperación de las credenciales de {{site.data.keyword.cloud_notm}}
{: #retrieve-credentials}

Para trabajar con la API, debe generar credenciales de servicio y autenticación. 

Para recopilar las credenciales:

1. [Genere una señal de acceso IAM de {{site.data.keyword.cloud_notm}}](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. [Recupere el ID de instancia que identifica de forma exclusiva su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-retrieve-instance-ID).

## Cómo crear una solicitud de API
{: #form-api-request}

Cuando se realiza una llamada de API al servicio, la solicitud de API se estructura de acuerdo a cómo se suministró su instancia de {{site.data.keyword.keymanagementserviceshort}}. 

Para crear su solicitud, hay que emparejar un [punto final de servicio regional](/docs/services/key-protect?topic=key-protect-regions) con las credenciales de autenticación adecuadas. Por ejemplo, si ha creado una instancia de servicio para la región `us-south`, utilice el siguiente punto final y las cabeceras de API para examinar claves en el servicio:

```cURL
curl -X GET \
    https://us-south.kms.cloud.ibm.com/api/v2/keys \
    -H 'accept: application/vnd.ibm.collection+json' \
    -H 'authorization: Bearer <señal_acceso>' \
    -H 'bluemix-instance: <ID_instancia>'
```
{: codeblock} 

Sustituya `<access_token>` e `<instance_ID>` por las credenciales de autenticación y servicio que ha recuperado.

¿Desea realizar un seguimiento de las solicitudes de API por si algo va mal? Cuando se incluye el distintivo `-v` como parte de la solicitud cURL, se obtiene un valor de `correlation-id` en las cabeceras de respuesta. Puede utilizar este valor para correlacionar y realizar un seguimiento de la solicitud con fines de depuración.
{: tip} 

## Qué hacer a continuación
{: #set-up-api-next-steps}

Ya está listo para empezar a gestionar las claves de cifrado en Key Protect. Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/key-protect){: new_window}.

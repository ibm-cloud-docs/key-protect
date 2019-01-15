---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Sucesos de {{site.data.keyword.cloudaccesstrailshort}}
{: #at-events}

Utilice el servicio de {{site.data.keyword.cloudaccesstrailfull}} para realizar el seguimiento de cómo interactúan los usuarios y las aplicaciones con {{site.data.keyword.keymanagementservicefull}}. 
{: shortdesc}

El servicio de {{site.data.keyword.cloudaccesstrailfull_notm}} registra actividades iniciadas por los usuarios que cambian el estado de un servicio en {{site.data.keyword.cloud_notm}}. 

Para obtener más información, consulte la [documentación de {{site.data.keyword.cloudaccesstrailshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/cloud-activity-tracker/index.html#getting-started-with-cla){: new_window}.

## Lista de sucesos
{: #events}

La tabla siguiente lista las acciones que generan un suceso:

<table>
    <tr>
        <th>Acción</th>
        <th>Descripción</th>
    </tr>
    <tr>
        <td>kms.secrets.create</td>
        <td>Crear una clave</td>
    </tr>
    <tr>
        <td>kms.secrets.read</td>
        <td>Recuperar una clave por ID</td>
    </tr>
   <tr>
        <td>kms.secrets.delete</td>
        <td>Suprimir una clave por ID</td>
    </tr>
    <tr>
        <td>kms.secrets.list</td>
        <td>Recuperar una lista de claves</td>
    </tr>
    <tr>
        <td>kms.secrets.head</td>
        <td>Recuperar el número de claves</td>
    </tr>
     <tr>
        <td>kms.secrets.wrap</td>
        <td>Envolver una clave</td>
    </tr>
     <tr>
        <td>kms.secrets.unwrap</td>
        <td>Desenvolver una clave</td>
    </tr>
     <tr>
        <td>kms.secrets.rotate</td>
        <td>Rotar una clave</td>
    </tr>
    <caption style="caption-side:bottom;">Tabla 1. Acciones que generan sucesos de {{site.data.keyword.cloudaccesstrailfull_notm}}</caption>
</table>

## Dónde ver los sucesos
{: #gui}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

Los sucesos de {{site.data.keyword.cloudaccesstrailshort}} están disponibles en el **dominio de la cuenta** de {{site.data.keyword.cloudaccesstrailshort}} disponible en la región de {{site.data.keyword.cloud_notm}} donde se han generado los sucesos.

Por ejemplo, al crear, importar, suprimir o leer una clave en {{site.data.keyword.keymanagementserviceshort}}, se genera un suceso de {{site.data.keyword.cloudaccesstrailshort}}. Estos sucesos se reenvían automáticamente al servicio de {{site.data.keyword.cloudaccesstrailshort}} en la misma región donde se proporciona el servicio de {{site.data.keyword.keymanagementserviceshort}}.

Para supervisar la actividad de la API, debe suministrar el servicio de {{site.data.keyword.cloudaccesstrailshort}} en un espacio que esté disponible en la misma región donde se suministra el servicio de {{site.data.keyword.keymanagementserviceshort}}. A continuación, puede ver sucesos mediante la vista de la cuenta en la IU de {{site.data.keyword.cloudaccesstrailshort}} si tiene un plan Lite, y mediante Kibana si tiene un plan Premium.

## Información adicional
{: #info}

Debido a la confidencialidad de la información para una clave de cifrado, cuando se genera un suceso como resultado de una llamada de API al servicio de {{site.data.keyword.keymanagementserviceshort}}, el suceso generado no incluye información detallada sobre la clave. El suceso incluye un ID de correlación que puede utilizar para identificar la clave internamente en el entorno de nube. El ID de correlación es un campo que se devuelve como parte del campo `responseHeader.content`. Puede utilizar esta información para correlacionar la clave de {{site.data.keyword.keymanagementserviceshort}} con la información de la acción de la que se ha informado a través del suceso de {{site.data.keyword.cloudaccesstrailshort}}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: instance ID, instance GUID, get instance ID, get instance GUID, instance ID API, instance ID CLI

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

# Recuperación del ID de instancia
{: #retrieve-instance-ID}

Puede elegir como destino una instancia de servicio de {{site.data.keyword.keymanagementservicelong}} para operaciones incluyendo su identificador exclusivo o ID de instancia en las solicitudes de API al servicio.
{: shortdesc}

## Visualización del ID de instancia en la consola de {{site.data.keyword.cloud_notm}}
{: #view-instance-ID}

Puede ver el ID de instancia que está asociado a la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} yendo a la lista de recursos de {{site.data.keyword.cloud_notm}}.

1. [Inicie la sesión en la consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}){: external}.
2. Vaya a **Menú** &gt; **Lista de recursos** y, a continuación, pulse **Servicios** para examinar una lista de los servicios de nube.
3. Pulse en la fila de la tabla que describe la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.
4. En la vista de detalles de servicio, copie el valor de **GUID**.

    Este valor de **GUID** representa el ID de instancia que identifica de forma exclusiva su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

## Recuperación de un ID de instancia con la CLI
{: #retrieve-instance-ID-cli}

También puede recuperar el ID de instancia de la instancia de servicio utilizando la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.
    {: note}

2. Seleccione la cuenta, la región y el grupo de recursos que contienen la instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.

3. Recupere el nombre de recurso de nube (CRN) que identifica de forma exclusiva su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance <nombre_instancia> --id
    ```
    {: pre}

    Sustituya `<instance_name>` por el alias exclusivo que ha asignado a la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. El siguiente ejemplo esquematizado muestra la salida de la CLI.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    El valor _42454b3b-5b06-407b-a4b3-34d9ef323901_ es un ID de instancia de ejemplo.


## Recuperación de un ID de instancia con la API
{: #retrieve-instance-ID-api}

Puede que desee recuperar el ID de instancia mediante programación para ayudarle a crear y conectar la aplicación. Puede llamar a la
[API del controlador de recursos de {{site.data.keyword.cloud_notm}}](https://{DomainName}/apidocs/resource-controller) y después canalizar la salida de JSON a `jq` para extraer este valor.

1. [Recupere una señal de acceso de {{site.data.keyword.cloud_notm}} IAM](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. Llame a la [API del controlador de recursos](https://{DomainName}/apidocs/resource-controller) para recuperar el ID de instancia.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <señal_acceso>' | jq -r '.resources[] | select(.name | contains("<nombre_instancia>")) | .guid'
    ```
    {: codeblock}

    Sustituya `<instance_name>` por el alias exclusivo que ha asignado a la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}. En la salida siguiente se muestra un ID de instancia de ejemplo:

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

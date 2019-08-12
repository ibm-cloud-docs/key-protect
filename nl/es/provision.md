---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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

# Suministro del servicio
{: #provision}

Puede crear una instancia de {{site.data.keyword.keymanagementservicefull}} utilizando la consola de {{site.data.keyword.cloud_notm}} o la CLI de {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Suministro desde la consola de {{site.data.keyword.cloud_notm}}
{: #provision-gui}

Para suministrar una instancia de {{site.data.keyword.keymanagementserviceshort}} desde la consola de {{site.data.keyword.cloud_notm}}, complete los siguientes pasos.

1. [Inicie una sesión en su cuenta de {{site.data.keyword.cloud_notm}}](https://{DomainName}){: external}.
2. Pulse **Catálogo** para ver la lista de servicios que están disponibles en {{site.data.keyword.cloud_notm}}.
3. En el panel de navegación Todas las categorías, pulse la categoría **Seguridad e identidad**.
4. Desde la lista de servicios, pulse el mosaico **{{site.data.keyword.keymanagementserviceshort}}**.
5. Seleccione un plan de servicio y pulse **Crear** para suministrar una instancia de {{site.data.keyword.keymanagementserviceshort}} en la cuenta, región y grupo de recursos donde ha iniciado una sesión.

## Suministro desde la CLI de {{site.data.keyword.cloud_notm}}
{: #provision-cli}

También puede suministrar una instancia de {{site.data.keyword.keymanagementserviceshort}} utilizando la CLI de {{site.data.keyword.cloud_notm}}. 

1. Inicie sesión en {{site.data.keyword.cloud_notm}} mediante la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **Nota:** Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.

2. Seleccione la cuenta, la región y el grupo de recursos donde desea crear una instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

    Utilice el siguiente mandato para establecer su grupo de recursos y región de destino.

    ```sh
    ibmcloud target -r <nombre_región> -g <nombre_grupo_recursos>
    ```
    {: pre}

3. Suministre una instancia de {{site.data.keyword.keymanagementserviceshort}} dentro de dicha cuenta y grupo de recursos.

    ```sh
    ibmcloud resource service-instance-create <nombre_instancia> kms tiered-pricing
    ```
    {: pre}

    Sustituya `<instance_name>` por un alias exclusivo para su instancia de servicio.

4. Opcional: Verifique que la instancia de servicio se ha creado correctamente.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## Qué hacer a continuación
{: #provision-service-next-steps}

Para obtener más información sobre la gestión de sus claves mediante programación, [consulte la documentación de referencia de la API de {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/apidocs/key-protect){: external}.

---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuración de la CLI
{: #set-up-cli}

Puede utilizar el plugin de CLI de {{site.data.keyword.keymanagementservicelong_notm}} para crear, importar y gestionar claves de cifrado.

Para obtener más información sobre el uso de la CLI, consulte el [documento de consulta de CLI de {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect/cli-reference.html).
{: tip}

## Instalación del plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}
{: #install-cli}

Antes de configurar el plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}, instale la CLI de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/index.html#overview){: new_window}. 

Para instalar las CLI:

1. Instale la CLI de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/index.html#overview){: new_window}.

    Después de instalar la CLI, puede ejecutar mandatos `ibmcloud` para interactuar con sus servicios de nube.

2. Inicie sesión en {{site.data.keyword.cloud_notm}}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.

3. Para empezar a gestionar claves de cifrado, instale el plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}.

    ```sh
    ibmcloud plugin install key-protect -r 'IBM Cloud'
    ```
    {: pre}

4. Opcional: Verifique que el plugin se ha instalado correctamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Actualización del plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}
{: #update-cli}

Es posible que desee actualizar la CLI periódicamente para utilizar características nuevas.

Para actualizar la CLI:

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la CLI de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.

2. Instale la actualización desde el repositorio de plugins.

    ```sh
    ibmcloud plugin update key-protect -r 'IBM Cloud'
    ```
    {: pre}

3. Opcional: Verifique que el plugin se ha actualizado correctamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

## Desinstalación del plugin de CLI de {{site.data.keyword.keymanagementserviceshort}}
{: #uninstall-cli}

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la CLI de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/index.html#overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    **Nota:** Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.

2. Instale la actualización desde el repositorio de plugins.

    ```sh
    ibmcloud plugin uninstall key-protect
    ```
    {: pre}

3. Opcional: Verifique que el plugin se ha desinstalado correctamente.

    ```sh
    ibmcloud plugin list
    ```
    {: pre}

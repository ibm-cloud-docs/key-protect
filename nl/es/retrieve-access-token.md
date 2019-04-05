---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: access token, IAM token, generate access token, generate IAM token, get access token, get IAM token, IAM token API, IAM token CLI

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

# Recuperación de una señal de acceso
{: #retrieve-access-token}

Iniciación a las API de {{site.data.keyword.keymanagementservicelong}} autenticando las solicitudes al servicio con una señal de acceso de {{site.data.keyword.iamlong}} (IAM).
{: shortdesc}

## Recuperación de una señal de acceso con la CLI
{: #retrieve-token-cli}

Puede utilizar la [CLI de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-overview){: new_window} para generar rápidamente su señal de acceso a Cloud IAM.

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la [CLI de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.
    {: note}

2. Seleccione la cuenta, la región y el grupo de recursos que contienen la instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.

3. Ejecute el mandato siguiente para recuperar la señal de acceso de Cloud IAM.

    ```sh
    ibmcloud iam oauth-tokens
    ```
    {: codeblock}

    En el siguiente ejemplo truncado se muestra una señal IAM recuperada.

    ```sh
    IAM token:  Bearer eyJraWQiOiIyM...
    ```
    {: screen}

## Recuperación de una señal de acceso con la API
{: #retrieve-token-api}

También puede recuperar la señal de acceso mediante programación creando primero una [clave de API de ID de servicio](/docs/iam?topic=iam-serviceidapikeys) de la aplicación e intercambiando la clave de API por una señal IAM de {{site.data.keyword.cloud_notm}}.

1. Inicie sesión en {{site.data.keyword.cloud_notm}} con la [CLI de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Si el inicio de sesión falla, ejecute el mandato `ibmcloud login --sso` para intentarlo de nuevo. El parámetro `--sso` es obligatorio al iniciar sesión con un ID federado. Si se utiliza esta opción, vaya al enlace mostrado en la salida de la CLI para generar un código de acceso puntual.
    {: note}

2. Seleccione la cuenta, la región y el grupo de recursos que contienen la instancia suministrada de {{site.data.keyword.keymanagementserviceshort}}.

3. Cree un [ID de servicio](/docs/iam?topic=iam-serviceids#creating-a-service-id) de la aplicación.

  ```sh
  ibmcloud iam service-id-create SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
  ```
  {: pre}

4. [Asigne una política de acceso](/docs/iam?topic=iam-serviceidpolicy) para el ID de servicio.

    Puede asignar permisos de acceso para el ID de servicio [utilizando la consola de {{site.data.keyword.cloud_notm}}](/docs/iam?topic=iam-serviceidpolicy#access_new). Para saber cómo se correlacionan los roles de acceso de _Gestor_, _Escritor_ y _Lector_ con acciones de servicio específicas de {{site.data.keyword.keymanagementserviceshort}}, consulte [Roles y permisos](/docs/services/key-protect?topic=key-protect-manage-access#roles).
    {: tip}

5. Cree una [clave de API de ID de servicio](/docs/iam?topic=iam-serviceidapikeys).

  ```sh
  ibmcloud iam service-api-key-create API_KEY_NAME SERVICE_ID_NAME
                     [-d, --description DESCRIPTION]
                     [--file FILE_NAME]
  ```
  {: pre}

  Sustituya `<service_ID_name>` por el alias exclusivo que ha asignado al ID de servicio en el paso anterior. Guarde la clave de API descargándola en una ubicación segura. 

6. Llame a la [API de IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api) para recuperar la señal de acceso.

    ```cURL
    curl -X POST \
      "https://iam.bluemix.net/identity/token" \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -H "Accept: application/json" \
      -d "grant_type=urn%3Aibm%3Aparams%3Aoauth%3Agrant-type%3Aapikey&apikey=<API_KEY>"
    ```
    {: codeblock}

    En esta solicitud, sustituya `<API_KEY>` por la clave de API que ha creado en el paso anterior. En el siguiente ejemplo truncado se muestra la salida de la señal:

    ```
    {
    "access_token": "eyJraWQiOiIyM...",
    "expiration": 1512161390,
    "expires_in": 3600,
    "refresh_token": "...",
    "token_type": "Bearer"
    }
    ```
    {: screen}

    Utilice el valor completo de `access_token`, con el tipo de señal _Bearer_ como prefijo, para gestionar claves mediante programación utilizando la API de {{site.data.keyword.keymanagementserviceshort}} para su servicio. Para ver un ejemplo de solicitud de API de {{site.data.keyword.keymanagementserviceshort}}, consulte [Formar una solicitud de API](/docs/services/key-protect?topic=key-protect-set-up-api#form-api-request).

    Las señales de acceso son válidas durante 1 hora, pero puede volver a generarlas cuando las necesite. Para mantener el acceso al servicio, vuelva a generar la señal de acceso de su clave de API de forma periódica llamando a la [API de IAM Identity Services](https://{DomainName}/apidocs/iam-identity-token-api).   
    {: note }

    

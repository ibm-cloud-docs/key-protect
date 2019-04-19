---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

keywords: Key Protect CLI plug-in, CLI reference

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Consulta de la CLI de {{site.data.keyword.keymanagementserviceshort}}
{: #cli-reference}

Puede utilizar el plugin de la CLI de {{site.data.keyword.keymanagementserviceshort}} para gestionar claves en su instancia de {{site.data.keyword.keymanagementserviceshort}}.
{:shortdesc}

Para instalar el plugin de la CLI, consulte [Configuración de la CLI](/docs/services/key-protect?topic=key-protect-set-up-cli). 

Cuando inicie sesión en la [CLI de {{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window}, se le notificará cuando haya actualizaciones disponibles. Asegúrese de mantener la CLI al día para que pueda utilizar los mandatos y las señales disponibles para el plugin de la CLI de {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

## Mandatos ibmcloud kp
{: #ibmcloud-kp-commands}

Puede especificar uno de los siguientes mandatos:

<table summary="Mandatos para gestionar claves"> 
    <thead>
        <th colspan="5">Mandatos para gestionar claves</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-create">kp create</a></td>
            <td><a href="#kp-delete">kp delete</a></td>
            <td><a href="#kp-list">kp list</a></td>
            <td><a href="#kp-get">kp get</a></td>
            <td><a href="#kp-rotate">kp rotate</a></td>
        </tr>
        <tr>
            <td><a href="#kp-unwrap">kp unwrap</a></td>
            <td><a href="#kp-wrap">kp wrap</a></td>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabla 1. Mandatos para gestionar claves</caption> 
 </table>

 <table summary="Mandatos para gestionar políticas de claves">
    <thead>
        <th colspan="5">Mandatos para gestionar políticas de claves</th>
    </thead>
    <tbody>
        <tr>
            <td><a href="#kp-policy-list">kp policy list</a></td>
            <td><a href="#kp-policy-get">kp policy get</a></td>
            <td><a href="#kp-policy-set">kp policy set</a></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
    <caption style="caption-side:bottom;">Tabla 2. Mandatos para gestionar políticas de claves</caption> 
 </table>

## kp create
{: #kp-create}

[Crea una clave raíz](/docs/services/key-protect?topic=key-protect-create-root-keys) en la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} que especifique. 

```
ibmcloud kp create KEY_NAME -i INSTANCE_ID | $INSTANCE_ID
                   [-k, --key-material KEY_MATERIAL] 
                   [-s, --standard-key]
                   [--output FORMAT]
```
{:pre}

### Parámetros necesarios
{: #create-req-params}

<dl>
    <dt><code>KEY_NAME</code></dt>
        <dd>Alias descriptivo único para asignarlo a la clave.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #create-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Material de clave codificado en base64 que desea almacenar y gestionar en el servicio. Para importar una clave existente, proporcione una clave de 256 bits. Para generar una nueva clave, omita el parámetro <code>--key-material</code>.</dd>
    <dt><code>-s, --standard-key</code></dt>
        <dd>Establezca el parámetro sólo si desea crear una <a href="/docs/services/key-protect?topic=key-protect-envelope-encryption#key-types">clave estándar</a>. Para crear una clave raíz, omita el parámetro <code>--standard-key</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp delete
{: #kp-delete}

[Suprime una clave](/docs/services/key-protect?topic=key-protect-delete-keys) que esté almacenada en el servicio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp delete KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parámetros necesarios
{: #delete-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
   <dd>ID de la clave que desea suprimir. Para recuperar una lista de las claves disponibles, ejecute el mandato <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

## kp list
{: #kp-list}

Lista las últimas 200 claves que están disponibles en su instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp list -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parámetros necesarios
{: #list-req-params}

<dl>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp get
{: #kp-get}

Recupera detalles sobre una clave, como por ejemplo los metadatos y el material de la clave.

Si la clave se ha designado como una clave raíz, el sistema no puede devolver el material de clave de dicha clave.

```
ibmcloud kp get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parámetros necesarios
{: #get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>El ID de la clave que desea recuperar. Para recuperar una lista de las claves disponibles, ejecute el mandato <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp rotate
{: #kp-rotate}

[Rota una clave](/docs/services/key-protect?topic=key-protect-wrap-keys) almacenada en el servicio {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp rotate KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-k, --key-material KEY_MATERIAL] 
```
{: pre}

### Parámetros necesarios
{: #rotate-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clave raíz que desea rotar.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #rotate-opt-params}

<dl>
    <dt><code>-k, --key-material</code></dt>
        <dd>Material de clave codificado en base64 que desea utilizar para rotar una clave raíz existente. Para rotar una clave que inicialmente se importó al servicio, proporcione una clave nueva de 256 bits. Para rotar una clave que inicialmente se generó en {{site.data.keyword.keymanagementserviceshort}}, omita el parámetro <code>--key-material</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp wrap
{: #kp-wrap}

[Envuelve una clave de cifrado de datos](/docs/services/key-protect?topic=key-protect-wrap-keys) utilizando una clave raíz que está almacenada en la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}} que especifique.

```
ibmcloud kp wrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 [-p, --plaintext DATA_KEY] 
                 [-a, --aad ADDITIONAL_DATA]
```
{: pre}


### Parámetros necesarios
{: #wrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clave raíz que desea utilizar para envolver.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #wrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd>Datos de autenticación adicionales (AAD) que se utilizan para proteger aún más una clave. Si se proporcionan para envolver también se deben proporcionar para desenvolver.</dd>
    <dt><code>-p, --plaintext</code></dt>
        <dd>Clave de cifrado de datos (DEK) con codificación base64 que desea gestionar y proteger. Para importar una clave existente, proporcione una clave de 256 bits. Para generar y envolver una nueva DEK, omita el parámetro <code>--plaintext</code>.</dd>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp unwrap
{: #kp-unwrap}

[Desenvuelve una clave de cifrado de datos](/docs/services/key-protect?topic=key-protect-unwrap-keys) utilizando una clave raíz que está almacenada en la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.

```
ibmcloud kp unwrap KEY_ID -i INSTANCE_ID | $INSTANCE_ID 
                   CIPHERTEXT_FROM_WRAP
                   [-a, --aad ADDITIONAL_DATA, ..]
```
{: pre}

### Parámetros necesarios
{: #unwrap-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>ID de la clave raíz que ha utilizado para la solicitud inicial de envolvimiento.</dd>
    <dt><code>CIPHERTEXT_FROM_WRAP</code></dt>
        <dd>Clave de datos cifrados que se ha devuelto durante la solicitud inicial de envolvimiento.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #unwrap-opt-params}

<dl>
    <dt><code>-a, --aad</code></dt>
        <dd><p>Datos de autenticación adicionales (AAD) que se han utilizado para proteger aún más una clave. Puede proporcionar hasta 255 series, delimitadas mediante comas. Si ha proporcionado AAD durante el envolvimiento, debe especificar los mismos AAD durante el desenvolvimiento.</p><p><b>Importante:</b> El servicio {{site.data.keyword.keymanagementserviceshort}} no guarda datos de autenticación adicionales. Si proporciona AAD, guarde los datos en una ubicación segura para asegurarse de que pueda acceder y proporcionar los mismos AAD durante las llamadas de desenvolvimiento subsiguientes.</p></dd>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp policy list
{: #kp-policy-list}

Lista las políticas que están asociadas con la clave raíz que especifique.

```
ibmcloud kp policy list KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parámetros necesarios
{: #policy-list-req-params}

<dl>
    <dt><code>KEY_ID</code></dt>
        <dd>El ID de la clave raíz que desea consultar. Para recuperar una lista de las claves disponibles, ejecute el mandato <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #policy-list-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp policy get
{: #kp-policy-get}

Recupera detalles sobre una política de claves, como por ejemplo el intervalo de rotación automático de la clave.

```
ibmcloud kp policy get KEY_ID -i INSTANCE_ID | $INSTANCE_ID
```
{: pre}

### Parámetros necesarios
{: #policy-get-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>El ID de la clave que desea consultar. Para recuperar una lista de las claves disponibles, ejecute el mandato <a href="#kp-list">kp list</a>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #policy-get-opt-params}

<dl>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>

## kp policy set
{: #kp-policy-set}

Crea o sustituye la política que está asociada con la clave raíz que especifique.

```
ibmcloud kp policy set KEY_ID -i INSTANCE_ID | $INSTANCE_ID
                 --set-type POLICY_TYPE 
                 [--policy INTERVAL]
```
{: pre}

### Parámetros necesarios
{: #policy-set-req-params}

<dl>
   <dt><code>KEY_ID</code></dt>
        <dd>El ID de la clave que desea consultar. Para recuperar una lista de las claves disponibles, ejecute el mandato <a href="#kp-list">kp list</a>.</dd>
   <dt><code>--set-type</code></dt>
        <dd>Especifica el tipo de política que desea establecer. Para establecer una política de rotación, utilice <code>--set-type rotation</code>.</dd>
    <dt><code>-i, --instance-ID | $INSTANCE_ID</code></dt>
        <dd>ID de instancia de {{site.data.keyword.cloud_notm}} que identifica la instancia de servicio de {{site.data.keyword.keymanagementserviceshort}}.</dd>
</dl>

### Parámetros opcionales
{: #policy-set-opt-params}

<dl>
   <dt><code>-p, --policy</code></dt>
        <dd>Especifica el intervalo de tiempo de rotación (en meses) para una clave. El valor predeterminado es 1.</dd>
    <dt><code>--output</code></dt>
        <dd>Establece el formato de salida de la CLI. De forma predeterminada, todos los mandatos se imprimen en formato de tabla. Para cambiar el formato de salida a JSON, utilice <code>--output json</code>.</dd>
</dl>


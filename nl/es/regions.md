---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-24"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# Regiones y ubicaciones
{: #regions}

Conéctese a sus aplicaciones con el servicio de {{site.data.keyword.keymanagementservicelong_notm}} especificando un punto final de servicio regional.
{: shortdesc}

## Regiones disponibles
{: #regions}

{{site.data.keyword.keymanagementserviceshort}} está disponible en las siguientes regiones y ubicaciones: ![La imagen muestra las regiones en las que el servicio de protección de claves está disponible](images/world-map_min.svg)

## Puntos finales de servicio
{: #endpoints}

Si está gestionando sus recursos de {{site.data.keyword.keymanagementserviceshort}} mediante programación, consulte la siguiente tabla para determinar los puntos finales de API que hay que utilizar para conectarse a la [API de {{site.data.keyword.keymanagementserviceshort}}](https://console.bluemix.net/apidocs/kms): 

<table>
    <tr>
        <th>Nombre de región</th>
        <th>Ubicación geográfica</th>
        <th>Punto final de API de servicio</th>
    </tr>
    <tr>
        <td>Alemania</td>
        <td>Frankfurt, Alemania</td>
        <td>
            <code>keyprotect.eu-de.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Sídney</td>
        <td>Sídney, Australia</td>
        <td>
            <code>keyprotect.au-syd.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>Reino Unido</td>
        <td>Londres, Inglaterra</td>
        <td>
            <code>keyprotect.eu-gb.bluemix.net</code>
        </td>
    </tr>
    <tr>
        <td>EE.UU. sur</td>
        <td>Dallas, EE.UU.</td>
        <td>
            <code>keyprotect.us-south.bluemix.net</code>
        </td>
    </tr>
    <caption style="caption-side:bottom;">Tabla 1. Muestra los puntos finales disponibles para la API de {{site.data.keyword.keymanagementserviceshort}}</caption>
</table>

Para las instancias de servicio de {{site.data.keyword.keymanagementserviceshort}} que existen dentro de una región o un espacio de Cloud Foundry, utilice el punto final heredado `https://ibm-key-protect.edge.bluemix.net` para interactuar con la API de {{site.data.keyword.keymanagementserviceshort}}.
{: tip}

Para obtener más información sobre la autenticación con {{site.data.keyword.keymanagementserviceshort}}, consulte [Acceso a la API](/docs/services/key-protect/access-api.html).

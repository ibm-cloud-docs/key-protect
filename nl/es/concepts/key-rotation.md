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

# Rotación de claves
{: #key-rotation}

La rotación de claves tiene lugar cuando retira una clave de cifrado y sustituye la clave generando nuevo material de clave de cifrado.

La rotación de claves de forma regular le ayuda a cumplir los estándares del sector y las mejores prácticas de cifrado. La siguiente tabla describe los mayores beneficios de la rotación de claves:

<table>
  <th>Ventajas</th>
  <th>Descripción</th>
  <tr>
    <td>Gestión de período criptográfico de claves</td>
    <td>La rotación de claves limita la cantidad de información que protege una única clave. Rotando las claves raíz a intervalos regulares, también se reduce el período criptográfico de las claves. Cuanto más largo es el tiempo de vida de una clave de cifrado, más alta es la probabilidad de que se produzca una brecha de seguridad.</td>
  </tr>
  <tr>
    <td>Mitigación de incidencias</td>
    <td>Si su organización detecta un problema de seguridad, puede rotar inmediatamente la clave para mitigar o reducir los costes asociados con un riesgo de las claves.</td>
  </tr>

  <caption style="caption-side:bottom;">Tabla 1. Describe los beneficios de la rotación de claves</caption>
</table>

La rotación de claves se trata en la NIST Special Publication 800-57, Recommendation for Key Management. Para obtener más información, consulte [NIST SP 800-57 Pt. 1 Rev. 4. ![Icono de enlace externo](../../../icons/launch-glyph.svg "Icono de enlace externo")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}
{: tip}

## Cómo funciona la rotación de claves
{: #how-rotation-works}

Las claves criptográficas, durante su ciclo de vida, pasan por varios [estados de clave](/docs/services/key-protect/concepts/key-states.html). En el estado _Activa_, las claves cifran y descifran datos. En el estado _Desactivada_, las claves no pueden cifrar datos, pero permanecen disponibles para el descifrado. En el estado _Destruida_, las claves ya no se pueden utilizar para el cifrado ni para el descifrado.

La rotación de claves funciona cambiando de forma segura material de clave de un estado _Activo_ a _Desactivado_. Para sustituir el material de clave retirado, el nuevo material clave cambia a estado _Activo_ y estará disponible para operaciones de cifrado.

En {{site.data.keyword.keymanagementserviceshort}}, puede rotar las claves raíz bajo demanda, sin que sea necesario realizar un seguimiento del material de clave raíz retirado. En el diagrama siguiente se muestra una vista contextual de la funcionalidad de rotación de claves.
![El diagrama muestra una vista contextual de la rotación de claves.](../images/key-rotation_min.svg)

La rotación solo está disponible para las claves raíz. 
{: note}

### Rotación de claves raíz
{: #rotating-key}

Con cada solicitud de rotación, {{site.data.keyword.keymanagementserviceshort}} asocia el nuevo material de clave con la clave raíz. 

![El diagrama muestra una micro vista de la pila de claves raíz.](../images/root-key-stack_min.svg)

Cuando se completa una rotación, el nuevo material de clave raíz pasa a estar disponible para proteger claves de cifrado de datos (DEK) futuras con [cifrado de sobre](/docs/services/key-protect/concepts/envelope-encryption.html). El material de clave retirado pasa al estado _Desactivado_, donde solo se puede utilizar para desenvolver y acceder a DEK más antiguas que todavía no están protegidas por el último material de clave raíz. Si {{site.data.keyword.keymanagementserviceshort}} detecta que utiliza material de clave raíz retirado para desenvolver una DEK antigua, el servicio vuelve a cifrar automáticamente la DEK y devuelve una clave de cifrado de datos envuelta (WDEK) que se basa en el último material de clave raíz. Almacene y utilice la nueva WDEK para futuras operaciones de desenvolvimiento para proteger a sus DEK con el último material de clave raíz.

Para saber cómo utilizar la API de {{site.data.keyword.keymanagementserviceshort}} para rotar sus claves raíz, consulte [Rotación de claves](/docs/services/key-protect/rotate-keys.html).

Cuando rote una clave en {{site.data.keyword.keymanagementserviceshort}}, no se le añadirán cargos adicionales. Puede seguir desenvolviendo las claves de cifrado de datos envueltas (WDEK) con material de clave retirado sin ningún coste adicional. Para obtener más información sobre nuestras opciones de precios, consulte la [página del catálogo de {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect).
{: tip}

## Frecuencia de la rotación de claves
{: #rotation-frequency}

Después de generar una clave raíz en {{site.data.keyword.keymanagementserviceshort}}, debe decidir la frecuencia de su rotación. Es posible que desee rotar sus claves debido a cambios de personal, funcionamiento incorrecto del proceso o de acuerdo con la política de caducidad de claves internas de su organización. 

Rote sus claves de forma regular, por ejemplo, cada 30 días, para seguir las mejores prácticas de cifrado. {{site.data.keyword.keymanagementserviceshort}} permite una rotación por hora por cada clave raíz.

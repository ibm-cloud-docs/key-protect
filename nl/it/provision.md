---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-18"

keywords: service instance, create service instance, KMS service instance, Key Protect service instance

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

# Provisioning del servizio
{: #provision}

Puoi creare un'istanza di {{site.data.keyword.keymanagementservicefull}} utilizzando la console {{site.data.keyword.cloud_notm}} o la CLI {{site.data.keyword.cloud_notm}}.
{: shortdesc}

## Provisioning dalla console {{site.data.keyword.cloud_notm}}
{: #provision-gui}

Per eseguire il provisioning di un'istanza di {{site.data.keyword.keymanagementserviceshort}} dalla console
{{site.data.keyword.cloud_notm}}, completa la seguente procedura.

1. [Accedi al tuo account {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}.
2. Fai clic su **Catalogo** per visualizzare l'elenco dei sevizi disponibili in {{site.data.keyword.cloud_notm}}.
3. Dal riquadro di navigazione Tutte le categorie, fai clic sulla categoria **Sicurezza e identità**.
4. Dall'elenco di servizi, fai clic sul tile **{{site.data.keyword.keymanagementserviceshort}}**.
5. Seleziona un piano dei sevizi e fai clic su **Crea** per eseguire il provisioning di un'istanza di
{{site.data.keyword.keymanagementserviceshort}} nell'account, regione e gruppo di risorse in cui hai eseguito l'accesso.

## Provisioning dalla CLI {{site.data.keyword.cloud_notm}}
{: #provision-cli}

Puoi anche eseguire il provisioning di un'istanza di {{site.data.keyword.keymanagementserviceshort}} utilizzando la CLI {{site.data.keyword.cloud_notm}}. 

1. Accedi a {{site.data.keyword.cloud_notm}} attraverso la [CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-overview){: new_window}.

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **Nota:** se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.

2. Seleziona l'account, la regione e il gruppo di risorse in cui vuoi creare un'istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

    Puoi utilizzare il seguente comando per impostare la regione e il gruppo di risorse.

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. Esegui il provisioning di un'istanza di {{site.data.keyword.keymanagementserviceshort}} in tali account e gruppo di risorse.

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    Sostituisci `<instance_name>` con un alias univoco per la tua istanza del servizio.

4. Facoltativo: verifica che tale istanza del servizio sia stata creata correttamente.

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## Operazioni successive
{: #provision-service-next-steps}

Per ulteriori informazioni sulla gestione a livello programmatico delle tue chiavi, [consulta la documentazione di riferimento API {{site.data.keyword.keymanagementserviceshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/key-protect){: new_window}.

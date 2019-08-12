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

# Richiamo del tuo ID dell'istanza
{: #retrieve-instance-ID}

Puoi selezionare una sola istanza del servizio {{site.data.keyword.keymanagementservicelong}} per le operazioni includendo il suo identificativo univoco o l'ID istanza, nelle richieste API al servizio.
{: shortdesc}

## Visualizzazione del tuo ID istanza nella console {{site.data.keyword.cloud_notm}}
{: #view-instance-ID}

Puoi visualizzare l'ID istanza associato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}} passando al tuo elenco di risorse {{site.data.keyword.cloud_notm}}.

1. [Accedi alla console {{site.data.keyword.cloud_notm}}](https://{DomainName}){: external}.
2. Vai a **Menu** &gt; **Resource List** e fai poi clic su **Services** per selezionare un elenco di tuoi servizi cloud.
3. Fai clic sulla tabella che descrive la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.
4. Dalla vista dei dettagli del servizio, copia il valore **GUID**.

    Questo valore **GUID** rappresenta l'ID istanza che identifica in modo univoco la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}.

## Richiamo di un ID istanza con la CLI
{: #retrieve-instance-ID-cli}

Puoi anche richiamare l'ID istanza per la tua istanza del servizio utilizzando la [CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

1. Accedi a {{site.data.keyword.cloud_notm}} con la [CLI {{site.data.keyword.cloud_notm}}](/docs/cli?topic=cloud-cli-getting-started){: external}.

    ```sh
    ibmcloud login 
    ```
    {: pre}

    Se l'accesso non riesce, esegui il comando `ibmcloud login --sso` e prova di nuovo. Il parametro `--sso` è obbligatorio quando
accedi con un ID federato. Se viene utilizzata questa opzione, vai al link elencato nell'output della CLI
per generare una passcode monouso.
    {: note}

2. Seleziona l'account, la regione e il gruppo di risorse che contengono la tua istanza di cui è stato eseguito il provisioning di {{site.data.keyword.keymanagementserviceshort}}.

3. Richiama il CRN (Cloud Resource Name) che identifica in modo univoco la tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. 

    ```sh
    ibmcloud resource service-instance <instance_name> --id
    ```
    {: pre}

    Sostituisci `<instance_name>` con l'alias univoco che hai assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Il seguente esempio troncato mostra l'output della CLI.

    ```
    crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:42454b3b-5b06-407b-a4b3-34d9ef323901:: 42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

    Il valore _42454b3b-5b06-407b-a4b3-34d9ef323901_ è un ID dell'istanza di esempio.


## Richiamo di un ID istanza con l'API
{: #retrieve-instance-ID-api}

Potresti voler richiamare l'ID istanza in modo programmatico per aiutarti a creare e connettere la tua applicazione. Puoi richiamare l'[API {{site.data.keyword.cloud_notm}} Resource Controller](https://{DomainName}/apidocs/resource-controller) e poi inviare l'output JSON a `jq` per estrarre questo valore.

1. [Richiama un token di accesso {{site.data.keyword.cloud_notm}} IAM](/docs/services/key-protect?topic=key-protect-retrieve-access-token).
2. Richiama l'[API Resource Controller](https://{DomainName}/apidocs/resource-controller) per recuperare il tuo ID istanza.

    ```sh
    curl -X GET \
    https://resource-controller.cloud.ibm.com/v2/resource_instances \
    -H 'Authorization: Bearer <access_token>' | jq -r '.resources[] | select(.name | contains("<instance_name>")) | .guid'
    ```
    {: codeblock}

    Sostituisci `<instance_name>` con l'alias univoco che hai assegnato alla tua istanza del servizio {{site.data.keyword.keymanagementserviceshort}}. Il seguente output mostra un ID istanza di esempio:

    ```
    42454b3b-5b06-407b-a4b3-34d9ef323901
    ```
    {: screen}

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

# Rotazione delle chiavi
{: #key-rotation}

La rotazione delle chiavi ha luogo quando ritiri una chiave di crittografia e sostituisci la chiave generando del nuovo materiale della chiave crittografico.

Eseguire la rotazione delle chiavi su base regolare aiuta a soddisfare gli standard del settore e le prassi ottimali crittografiche. La seguente tabella descrive i vantaggi principali della rotazione delle chiavi:

<table>
  <th>Vantaggio</th>
  <th>Descrizione</th>
  <tr>
    <td>Gestione del periodo di crittografia per le chiavi</td>
    <td>La rotazione delle chiavi limita la quantità di informazioni protetta da una singola chiave. Eseguendo la rotazione delle tue chiavi root a intervalli regolari, puoi anche abbreviare il periodo di crittografia delle chiavi. Più lunga è la durata di una chiave di crittografia e più elevata è la probabilità di una violazione della sicurezza.</td>
  </tr>
  <tr>
    <td>Mitigazione degli incidenti</td>
    <td>Se la tua organizzazione rileva un problema di sicurezza, puoi eseguire immediatamente la rotazione della chiave per attenuare o ridurre i costi associati alla compromissione di una chiave.</td>
  </tr>

  <caption style="caption-side:bottom;">Tabella 1. Descrive i vantaggi della rotazione delle chiavi</caption>
</table>

La rotazione delle chiavi è trattata in NIST Special Publication 800-57, Recommendation for Key Management. Per ulteriori informazioni, consulta [NIST SP 800-57 Pt. 1 Rev. 4. ![Icona link esterno](../../../icons/launch-glyph.svg "Icona link esterno")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}
{: tip}

## Come funziona la rotazione delle chiavi
{: #how-rotation-works}

Le chiavi crittografiche, nel corso della loro durata, passano attraverso diversi [stati della chiave](/docs/services/key-protect/concepts/key-states.html). Nello stato _Attiva_, le chiavi crittografano e decrittografano i dati. Nello stato _Disattivata_, le chiavi non possono crittografare i dati ma rimangono disponibili per la decrittografia. Nello stato _Distrutta_, le chiavi non sono più utilizzate per la crittografia o la decrittografia.

La rotazione delle chiavi funziona mediante il passaggio, in modo protetto, del materiale della chiave da uno stato di _Attiva_ a uno stato di _Disattivata_. Per sostituire il materiale della chiave ritirato, il nuovo materiale della chiave passa allo stato di _Attiva_ e diventa disponibile per le operazioni crittografiche.

In {{site.data.keyword.keymanagementserviceshort}}, puoi eseguire la rotazione delle tue chiavi root su richiesta, senza che occorra tenere traccia del tuo materiale della chiave root ritirato. Il seguente diagramma mostra una vista contestuale della funzionalità di rotazione delle chiavi.
![Il diagramma mostra una vista contestuale di una rotazione di chiavi.](../images/key-rotation_min.svg)

La rotazione è disponibile solo per le chiavi root.
{: note}

### Rotazione delle chiavi root
{: #rotating-key}

Con ciascuna richiesta di rotazione, {{site.data.keyword.keymanagementserviceshort}} associa il nuovo materiale della chiave alla tua chiave root. 

![Il diagramma mostra una vista in scala ridotta dello stack di chiavi root.](../images/root-key-stack_min.svg)

Dopo il completamento di una rotazione, il nuovo materiale della chiave root diventa disponibile per proteggere le future DEK (data encryption key) con la [crittografia envelope](/docs/services/key-protect/concepts/envelope-encryption.html). Il materiale della chiave ritirato passa allo stato _Disattivato_, dove può essere utilizzato solo per spacchettare e accedere alle DEK meno recenti che non sono ancora protette dal materiale della chiave root più recente. Se {{site.data.keyword.keymanagementserviceshort}} rileva che stai utilizzando del materiale della chiave root ritirato per spacchettare una DEK meno recente, il servizio automaticamente crittografa nuovamente la DEK e restituisce una WDEK (wrapped data encryption key) basata sul materiale della chiave root più recente. Archivia e usa la nuova WDEK per future operazioni di spacchettamento, quindi proteggi le tue DEK con il materiale della chiave root più recente.

Per ulteriori informazioni su come utilizzare l'API {{site.data.keyword.keymanagementserviceshort}} per eseguire la rotazione delle tue chiavi root, vedi [Rotazione delle chiavi](/docs/services/key-protect/rotate-keys.html).

Quando esegui la rotazione di una chiave in {{site.data.keyword.keymanagementserviceshort}}, non ci sono ulteriori addebiti a tuo carico. Puoi continuare a spacchettare le tue WDEK (wrapped data encryption key) con il materiale della chiave ritirato senza alcun costo supplementare. Per ulteriori informazioni sulle nostre opzioni di prezzo, consulta la [pagina del catalogo {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect).
{: tip}

## Frequenza di rotazione delle chiavi
{: #rotation-frequency}

Dopo che hai generato una chiave root in {{site.data.keyword.keymanagementserviceshort}}, decidi la frequenza della sua rotazione. Potesti voler eseguire la rotazione delle tue chiavi per un avvicendamento del personale, un malfunzionamento del processo oppure in base alla politica interna di scadenza delle chiavi della tua organizzazione. 

Esegui la rotazione delle tue chiavi regolarmente, ad esempio ogni 30 giorni, per soddisfare le prassi ottimali crittografiche. {{site.data.keyword.keymanagementserviceshort}} consente una rotazione all'ora per ciascuna chiave root.

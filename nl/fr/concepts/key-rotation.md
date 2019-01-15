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

# Rotation des clés
{: #key-rotation}

La rotation des clés s'effectue lorsque vous retirez une clé de chiffrement et la remplacez en générant un nouveau le matériel relatif à la clé cryptographique.

Procéder régulièrement à une rotation des clés vous aide à respecter les normes en vigueur de l'industrie et les meilleures pratiques en matière de cryptographie. Le tableau suivant décrit les principaux avantages de la rotation des clés :

<table>
  <th>Avantage</th>
  <th>Description</th>
  <tr>
    <td>Gestion de la cryptopériode des clés</td>
    <td>La rotation des clés limite la quantité d'informations que protège une unique clé. En procédant à une rotation des clés à intervalles réguliers, vous raccourcissez également la cryptopériode des clés. Plus la durée de vie d'une clé de chiffrement est longue, plus le risque de violation de la sécurité est élevé.</td>
  </tr>
  <tr>
    <td>Atténuation d'incidents</td>
    <td>Si votre organisation détecte un problème de sécurité, vous pouvez immédiatement effectuer une rotation de la clé afin d'atténuer ou de réduire les coûts associés à une clé non fiable.</td>
  </tr>

  <caption style="caption-side:bottom;">Tableau 1. Description des avantages de la rotation des clés</caption>
</table>

La rotation des clés est abordée dans le document NIST Special Publication 800-57, Recommendation for Key Management. Pour en savoir plus, voir [NIST SP 800-57 Pt. 1 Rev. 4. ![Icône de lien externe](../../../icons/launch-glyph.svg "Icône de lien externe")](http://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r4.pdf){: new_window}
{: tip}

## Fonctionnement de la rotation des clés
{: #how-rotation-works}

Les clés cryptographiques, pendant leur durée de vie, passent par différents [états de clé](/docs/services/key-protect/concepts/key-states.html). A l'état _Actif_, les clés chiffrent et déchiffrent des données. A l'état _Désactivé_, les clés ne peuvent pas chiffrer de données, mais elles restent disponibles pour le déchiffrement. A l'état _Détruit_, les clés ne sont plus utilisables ni pour le chiffrement ni pour le déchiffrement.

La rotation des clés opère une transition sécurisée du matériel relatif à la clé de l'état _Actif_ à l'état _Désactivé_. Pour remplacer le matériel relatif à la clé retiré, un nouveau le matériel relatif à la clé est passé à l'état _Actif_ et devient disponible pour des opérations de chiffrement.

Dans {{site.data.keyword.keymanagementserviceshort}}, vous pouvez effectuer une rotation de vos clés racine à la demande, sans avoir à garder une trace du matériel relatif aux clés racine retiré. Le diagramme suivant présente une vue contextuelle de la fonction de rotation des clés.
![Diagramme présentant une vue contextuelle de la rotation des clés.](../images/key-rotation_min.svg)

La rotation est disponible uniquement pour les clés racine.
{: note}

### Rotation de clés racine
{: #rotating-key}

A chaque demande de rotation, {{site.data.keyword.keymanagementserviceshort}} associe un nouveau matériel relatif à la clé à votre clé racine. 

![Diagramme illustrant une vue microscopique de la pile de clés racine.](../images/root-key-stack_min.svg)

Une fois la rotation terminée, le nouveau matériel relatif à la clé racine est disponible pour protéger les futures clés de chiffrement de données (DEK) avec [chiffrement d'enveloppe](/docs/services/key-protect/concepts/envelope-encryption.html). Le matériel retiré est placé à l'état _Désactivé_, dans lequel il ne peut être utilisé que pour désencapsuler et accéder aux anciennes clés de chiffrement de données qui ne sont pas encore protégées par le matériel le plus récent. Si {{site.data.keyword.keymanagementserviceshort}} détecte que vous utilisez du matériel retiré pour désencapsuler une ancienne clé de chiffrement de données, le service chiffre de nouveau automatiquement la clé de chiffrement de données et renvoie une clé de chiffrement de données encapsulée (WDEK) selon le dernier matériel relatif à la clé racine. Stockez et utilisez la nouvelle clé de chiffrement de données encapsulée (WDEK) pour vos futures opérations de désencapsulage de manière à protéger vos clés de chiffrement de données (DEK) avec le matériel relatif aux clés racine le plus récent.

Pour savoir comment utiliser l'API {{site.data.keyword.keymanagementserviceshort}} pour effectuer une rotation de vos clés racine, voir [Rotation des clés](/docs/services/key-protect/rotate-keys.html).

Lorsque vous effectuez une rotation de clé dans {{site.data.keyword.keymanagementserviceshort}}, aucuns frais supplémentaires ne vous sont facturés. Vous pouvez continuer à désencapsuler vos clés de chiffrement de données encapsulées (WDEK) avec du matériel de clé retiré sans coût supplémentaire. Pour plus d'informations sur nos options de tarification, voir la [page du catalogue {{site.data.keyword.keymanagementserviceshort}}](https://{DomainName}/catalog/services/key-protect).
{: tip}

## Fréquence de rotation des clés
{: #rotation-frequency}

Après avoir généré une clé racine dans {{site.data.keyword.keymanagementserviceshort}}, vous décidez de la fréquence de sa rotation. Vous pouvez effectuer une rotation des clés en raison d'une rotation de personnel, d'un dysfonctionnement de processus ou selon les règles d'expiration des clés internes de votre organisation. 

Effectuez régulièrement une rotation des clés, par exemple tous les 30 jours pour respecter les meilleures pratiques en matière de cryptographie. {{site.data.keyword.keymanagementserviceshort}} autorise une rotation par heure pour chaque clé racine.

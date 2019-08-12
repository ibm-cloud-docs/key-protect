---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Key Protect availability, Key Protect disaster recovery

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

# Alta disponibilidade e recuperação de desastre
{: #ha-dr}

O {{site.data.keyword.keymanagementservicefull}} é um serviço regional altamente disponível com recursos automáticos que ajudam a manter seus aplicativos seguros e operacionais.
{: shortdesc}

Use essa página para saber mais sobre as estratégias de alta disponibilidade e recuperação de desastres do {{site.data.keyword.keymanagementserviceshort}}.

## Locais, arrendamento e disponibilidade
{: #availability}

O {{site.data.keyword.keymanagementserviceshort}}  é um serviço regional de diversos locatários. 

É possível criar recursos do {{site.data.keyword.keymanagementserviceshort}} em uma das [{{site.data.keyword.cloud_notm}}regiões](/docs/services/key-protect?topic=key-protect-regions#regions) suportadas, que representam a área geográfica na qual as solicitações do {{site.data.keyword.keymanagementserviceshort}} são manipuladas e processadas. Cada região do {{site.data.keyword.cloud_notm}} contém [múltiplas zonas de disponibilidade](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external} para atender aos requisitos de acesso local, baixa latência e segurança para a região.

Ao planejar sua estratégia de criptografia em repouso com o {{site.data.keyword.cloud_notm}}, lembre-se de que o fornecimento do {{site.data.keyword.keymanagementserviceshort}} em uma região mais próxima a você mais provavelmente resultará em conexões mais rápidas e mais confiáveis ao interagir com as APIs do {{site.data.keyword.keymanagementserviceshort}}. Escolha uma região específica se os usuários, os apps ou os serviços que dependem de um recurso do {{site.data.keyword.keymanagementserviceshort}} estiverem geograficamente concentrados. Lembre-se de que os usuários e os serviços muito distantes da região podem ter latência mais alta. 

As suas chaves de criptografia estão limitadas à região na qual você as cria. O {{site.data.keyword.keymanagementserviceshort}} não copia ou exporta chaves de criptografia para outras regiões.
{: note}

## Recuperação de desastre
{: #disaster-recovery}

O {{site.data.keyword.keymanagementserviceshort}} tem recuperação de desastre regional em vigor com um Objetivo de Tempo de Recuperação (RTO) de uma hora. O serviço segue os requisitos do {{site.data.keyword.cloud_notm}} para planejamento e recuperação de eventos de desastre. Para obter mais informações, consulte  [ Recuperação de Desastre ](/docs/overview?topic=overview-zero-downtime#disaster-recovery).



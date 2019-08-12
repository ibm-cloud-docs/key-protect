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

# 高可用性和災難回復
{: #ha-dr}

{{site.data.keyword.keymanagementservicefull}} 是一項高度可用的地區服務，其具有可協助您保護應用程式安全和作業的自動特性。
{: shortdesc}

請利用這個頁面來進一步瞭解 {{site.data.keyword.keymanagementserviceshort}} 的可用性和災難回復策略。

## 位置、承租戶及可用性
{: #availability}

{{site.data.keyword.keymanagementserviceshort}} 是一項多方承租戶地區服務。 

您可以在其中一個支援的 [{{site.data.keyword.cloud_notm}} 地區](/docs/services/key-protect?topic=key-protect-regions#regions)中建立 {{site.data.keyword.keymanagementserviceshort}} 資源，這些區域代表管理及處理 {{site.data.keyword.keymanagementserviceshort}} 要求的地理區域。每個 {{site.data.keyword.cloud_notm}} 地區都包含[多個可用性區域](https://www.ibm.com/blogs/bluemix/2018/06/expansion-availability-zones-global-regions/){: external}，以符合地區的本端存取、低延遲及安全需求。

在您使用 {{site.data.keyword.cloud_notm}} 規劃靜態策略的加密時，請牢記，當您與 {{site.data.keyword.keymanagementserviceshort}} API 互動時，在離您最近的地區中佈建 {{site.data.keyword.keymanagementserviceshort}} 可能會產生更快速、更可靠的連線。如果取決於 {{site.data.keyword.keymanagementserviceshort}} 資源的使用者、應用程式或服務依地理方式集中，請選擇特定的地區。請記住，遠離該地區的使用者及服務可能會遇到更高的延遲。 

您的加密金鑰僅限於您在其中建立它們的地區。{{site.data.keyword.keymanagementserviceshort}} 不會將加密金鑰複製或匯出到其他地區。
{: note}

## 災難回復
{: #disaster-recovery}

{{site.data.keyword.keymanagementserviceshort}} 已制定一小時的「回復時間目標 (RTO) 」來進行地區災難回復。服務會遵循 {{site.data.keyword.cloud_notm}} 需求，以規劃及回復災難事件。如需相關資訊，請參閱[災難回復](/docs/overview?topic=overview-zero-downtime#disaster-recovery)。



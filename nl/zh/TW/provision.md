---

copyright:
  years: 2017, 2019
lastupdated: "2019-04-03"

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

# 佈建服務
{: #provision}

您可以使用 {{site.data.keyword.cloud_notm}} 主控台或 {{site.data.keyword.cloud_notm}} CLI 建立 {{site.data.keyword.keymanagementservicefull}} 實例。
{: shortdesc}

## 從 {{site.data.keyword.cloud_notm}} 主控台佈建
{: #provision-gui}

若要從 {{site.data.keyword.cloud_notm}} 主控台佈建 {{site.data.keyword.keymanagementserviceshort}} 的實例，請完成下列步驟。

1. [登入 {{site.data.keyword.cloud_notm}} 帳戶 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}){: new_window}。
2. 按一下**型錄**以檢視 {{site.data.keyword.cloud_notm}} 上可用的服務清單。
3. 從「所有種類」導覽窗格，按一下**安全及身分**種類。
4. 從服務清單按一下 **{{site.data.keyword.keymanagementserviceshort}}** 磚。
5. 選取服務方案，然後按一下**建立**以在您登入的帳戶、地區及資源群組中佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。

## 從 {{site.data.keyword.cloud_notm}} CLI 佈建
{: #provision-cli}

您也可以使用 {{site.data.keyword.cloud_notm}} CLI 來佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。 

1. 透過 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-ibmcloud-cli){: new_window} 登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **附註：**如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。

2. 選取要在其中建立 {{site.data.keyword.keymanagementserviceshort}} 服務實例的帳戶、地區及資源群組。

    您可以使用下列指令來設定目標地區及資源群組。

    ```sh
    ibmcloud target -r <region_name> -g <resource_group_name>
    ```
    {: pre}

3. 在該帳戶及資源群組內佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。

    ```sh
    ibmcloud resource service-instance-create <instance_name> kms tiered-pricing
    ```
    {: pre}

    將 `<instance_name>` 取代為服務實例的唯一別名。

4. 選用項目：確認已順利建立服務實例。

    ```sh
    ibmcloud resource service-instances
    ```
    {: pre}

## 下一步為何？
{: #provision-service-next-steps}

若要進一步瞭解如何以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/key-protect){: new_window}。

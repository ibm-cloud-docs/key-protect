---

copyright:
  years: 2017, 2018
lastupdated: "2018-06-07"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}

# 佈建服務
{: #provision}

您可以使用 {{site.data.keyword.cloud_notm}} 主控台或 {{site.data.keyword.cloud_notm}} CLI 建立 {{site.data.keyword.keymanagementservicefull}} 實例。
{: shortdesc}

## 從 {{site.data.keyword.cloud_notm}} 主控台佈建
{: #gui}

若要從 {{site.data.keyword.cloud_notm}} 主控台佈建 {{site.data.keyword.keymanagementserviceshort}} 的實例，請完成下列步驟。

1. [登入 {{site.data.keyword.cloud_notm}} 帳戶 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/){: new_window}。
2. 按一下**型錄**以檢視 {{site.data.keyword.cloud_notm}} 上可用的服務清單。
3. 從「所有種類」導覽窗格中，捲動至**平台**，然後按一下**安全**種類。
4. 從服務清單中，按一下 **{{site.data.keyword.keymanagementserviceshort}}** 磚。
5. 選取服務方案，然後按一下**建立**以在您登入的帳戶、地區及資源群組中佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。

## 從 {{site.data.keyword.cloud_notm}} CLI 佈建
{: #cli}

您可以使用 {{site.data.keyword.cloud_notm}} CLI 來佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。 

### 在帳戶內建立服務實例
{: #provision_acct_lvl}

若要使用 [{{site.data.keyword.iamlong}} 角色](/docs/iam/users_roles.html#iamusermanrol)來簡化對加密金鑰的存取，您可以在帳戶內建立一個以上的 {{site.data.keyword.keymanagementserviceshort}} 服務實例，而不需要指定組織及空間。 

1. 透過 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html){: new_window} 登入 {{site.data.keyword.cloud_notm}}。

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

### 在組織及空間內建立服務實例
{: #provision_space_lvl}

若要使用 [Cloud Foundry 角色](/docs/iam/cfaccess.html)來管理對加密金鑰的存取，您可以在指定的組織及空間內建立 {{site.data.keyword.keymanagementserviceshort}} 服務實例。  

1. 透過 [{{site.data.keyword.cloud_notm}} CLI](/docs/cli/reference/bluemix_cli/get_started.html){: new_window} 登入 {{site.data.keyword.cloud_notm}}。

    ```sh
    ibmcloud login 
    ```
    {: pre}
    **附註：**如果登入失敗，請執行 `ibmcloud login --sso` 指令再試一次。當您使用聯合 ID 登入時，需要 `--sso` 參數。如果使用這個選項，請前往 CLI 輸出中所列的鏈結，以產生一次性的通行碼。

2. 選取要在其中建立 {{site.data.keyword.keymanagementserviceshort}} 服務實例的帳戶、地區、組織及空間。

    您可以使用下列指令來設定目標地區、組織及空間。

    ```sh
    ibmcloud target -r <region> -o <organization_name> -s <space_name>
    ```
    {: pre}

3. 在該帳戶、地區、組織及空間內佈建 {{site.data.keyword.keymanagementserviceshort}} 實例。

    ```sh
    ibmcloud service create kms tiered-pricing <instance_name>
    ```
    {: pre}

    將 `<instance_name>` 取代為服務實例的唯一別名。

4. 選用項目：確認已順利建立服務實例。

    ```sh
    ibmcloud service list
    ```
    {: pre}


### 下一步為何？

- 若要查看 {{site.data.keyword.keymanagementserviceshort}} 中所儲存的金鑰如何運作來加密及解密資料的範例，請[試用 GitHub 中的範例應用程式 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/IBM-Bluemix/key-protect-helloworld-python){: new_window}。
- 若要進一步瞭解以程式設計方式管理您的金鑰，[請參閱 {{site.data.keyword.keymanagementserviceshort}} API 參考資料文件以取得程式碼範例 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/apidocs/639){: new_window}。

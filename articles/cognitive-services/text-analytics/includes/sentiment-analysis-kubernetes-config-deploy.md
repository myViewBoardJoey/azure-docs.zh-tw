---
title: 情感分析 Kubernetes config and 部署步驟
titleSuffix: Azure Cognitive Services
description: 情感分析 Kubernetes config and 部署步驟
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: ca8d4d725ff25687d1005ddab1964316a147c730
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779127"
---
### <a name="deploy-the-sentiment-analysis-container-to-an-aks-cluster"></a>將情感分析容器部署到 AKS 叢集

1. 開啟 Azure CLI，然後登入 Azure。

    ```azurecli
    az login
    ```

1. 登入 AKS 叢集。 `your-cluster-name` `your-resource-group` 以適當的值取代和。

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    執行此命令之後，它會報告類似以下的訊息：

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > 如果您的 Azure 帳戶有多個可用的訂用帳戶，而且 `az aks get-credentials` 命令傳回錯誤，表示您使用了錯誤的訂用帳戶。 將 Azure CLI 會話的內容設定為使用您用來建立資源的相同訂用帳戶，然後再試一次。
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. 開啟選擇的文字編輯器。 這個範例會使用 Visual Studio Code。

    ```console
    code .
    ```

1. 在文字編輯器中，建立名為 *情感*的新檔案，並在其中貼上下列 yaml。 請務必將和取代為 `billing/value` `apikey/value` 您自己的資訊。

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            resources:
              requests:
                memory: 2Gi
                cpu: 1
              limits:
                memory: 4Gi
                cpu: 1
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # {ENDPOINT_URI}
            - name: apikey
              value: # {API_KEY}
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. 儲存檔案，然後關閉文字編輯器。
1. `apply`使用*情感 yaml*檔案作為其目標來執行 Kubernetes 命令：

    ```console
    kubectl apply -f sentiment.yaml
    ```

    命令成功套用部署設定之後，會出現類似下列輸出的訊息：

    ```output
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. 確認已部署 pod：

    ```console
    kubectl get pods
    ```

    Pod 執行狀態的輸出：

    ```output
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. 確認服務可供使用，並取得 IP 位址。

    ```console
    kubectl get services
    ```

    Pod 中 *情感* 服務的執行狀態輸出：

    ```output
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

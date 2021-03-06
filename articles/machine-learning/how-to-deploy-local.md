---
title: 如何在本機執行和部署
titleSuffix: Azure Machine Learning
description: 瞭解如何在本機電腦上執行定型的模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: laobri
author: lobrien
ms.date: 11/20/2020
ms.topic: conceptual
ms.custom: how-to, deploy
ms.openlocfilehash: fcea7c162e5c978a7c8ff1b42e09ffe0afb98f3c
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/24/2020
ms.locfileid: "95549971"
---
# <a name="deploy-on-your-local-machines-models-trained-with-azure-machine-learning"></a>在以 Azure Machine Learning 定型的本機電腦模型上部署

本文會教您如何使用本機電腦作為定型或部署在 Azure Machine Learning 中建立之模型的目標。 Azure Machine Learning 的彈性可讓 it 使用大部分的 Python 機器學習架構。 機器學習解決方案通常有複雜的相依性，可能難以複製。 本文將告訴您如何以整體控制和易用性的方式進行取捨。

本機部署的某些案例包括：

* 在專案初期快速逐一查看資料、腳本和模型
* 後續階段中的調試和疑難排解
* 使用者管理的硬體上的最終部署

## <a name="prerequisites"></a>Prerequisites

- Azure Machine Learning 工作區。 如需詳細資訊，請參閱 [建立 Azure Machine Learning 工作區](how-to-manage-workspace.md)
- 模型和環境。 如果您沒有定型的模型，您可以使用[本教學](tutorial-train-models-with-aml.md)課程中提供的模型和相依性檔案。
- [適用于 Python 的 Azure Machine Learning 軟體發展工具組 (SDK) ](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py)
- Conda 管理員，例如 Anaconda 或 miniconda，如果您想要鏡像 Azure Machine Learning 的套件相依性
- 如果您想要使用容器化版本的 Azure Machine Learning 環境，請使用 Docker

## <a name="prepare-your-local-machine"></a>準備本機電腦

在本機執行 Azure Machine Learning 模型最可靠的方式就是使用 Docker 映射。 Docker 映射除了硬體問題之外，還提供一項隔離的容器化體驗。 如需有關針對開發案例安裝和設定 Docker 的詳細資訊，請參閱 [Windows 上的 docker 遠端開發總覽](/windows/dev-environment/docker/overview)。

雖然可以將偵錯工具附加至在 Docker 中執行的進程 (請參閱 [附加至執行中的容器](https://code.visualstudio.com/docs/remote/attach-container)) ，您可能會想要在不涉及 Docker 的情況下，對 Python 程式碼進行偵錯工具和反復查看。 在此案例中，您的本機電腦必須使用在 Azure Machine Learning 中執行實驗時使用的相同程式庫。 為了管理 Python 相依性，Azure 會使用 [conda](https://docs.conda.io/)。 雖然您可以使用其他套件管理員重新建立環境，但在本機電腦上安裝和設定 conda 是同步處理的最簡單方式。 

## <a name="prepare-your-entry-script"></a>準備您的輸入腳本

即使您使用 Docker 來管理模型和相依性，Python 評分腳本必須是本機的。 腳本必須有兩種方法：

- `init()`不採用任何引數且不傳回任何專案的方法 
- `run()`採用 json 格式字串並傳回 json 可序列化物件的方法。

方法的引數 `run()` 會是下列格式： 

```json
{
    "data": <model-specific-data-structure>
}
```

您從方法傳回的物件 `run()` 必須執行 `toJSON() -> string` 。

下列範例示範如何載入已註冊的 scikit-learn 學習模型，並使用 numpy 資料對其進行評分。 此範例是以 [本教學](tutorial-train-models-with-aml.md)課程的模型和相依性為基礎：

```python
import json
import numpy as np
import os
import pickle
import joblib

def init():
    global model
    # AZUREML_MODEL_DIR is an environment variable created during deployment.
    # It is the path to the model folder (./azureml-models/$MODEL_NAME/$VERSION)
    # For multiple models, it points to the folder containing all deployed models (./azureml-models)
    model_path = os.path.join(os.getenv('AZUREML_MODEL_DIR'), 'sklearn_mnist_model.pkl')
    model = joblib.load(model_path)

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    y_hat = model.predict(data)
    # you can return any data type as long as it is JSON-serializable
    return y_hat.tolist()
```

如需更高階的範例，包括自動 Swagger 架構產生，以及如何對二進位資料進行評分 (例如，影像) 資料、讀取 [advanced entry 腳本撰寫](how-to-deploy-advanced-entry-script.md)。 

## <a name="deploy-as-a-local-web-service-using-docker"></a>使用 Docker 部署為本機 web 服務

複寫 Azure Machine Learning 使用的環境最簡單的方式，就是使用 Docker 部署 web 服務。 當 Docker 在您的本機電腦上執行時，您將會：

1. 連接到您的模型註冊 Azure Machine Learning 工作區
1. 建立 `Model` 代表模型的物件
1. 建立包含相依性的 `Environment` 物件，並定義您的程式碼將在其中執行的軟體環境
1. 建立 `InferenceConfig` 物件，以將輸入腳本和 `Environment`
1. 建立子 `DeploymentConfiguration` 類別的物件 `LocalWebserviceDeploymentConfiguration`
1. 用 `Model.deploy()` 來建立 `Webservice` 物件。 此方法會下載 Docker 映射，並將其與 `Model` 、和相關聯。 `InferenceConfig``DeploymentConfiguration`
1. 啟動 `Webservice` 方式 `Webservice.wait_for_deployment()`

下列程式碼顯示這些步驟：

```python
from azureml.core.webservice import Webservice
from azure.core.model import InferenceConfig
from azureml.core.environment import Environment
from azureml.core import Workspace
from azureml.core.model import Model

ws = Workspace.from_config()
model = Model(ws, 'sklearn_mnist')


myenv = Environment.get(workspace=ws, name="tutorial-env", version="1")
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

deployment_config = LocalWebservice.deploy_configuration(port=6789)

local_service = Model.deploy(workspace=ws, 
                       name='sklearn-mnist-local', 
                       models=[model], 
                       inference_config=inference_config, 
                       deployment_config = deployment_config)

local_service.wait_for_deployment(show_output=True)
print(f"Scoring URI is : {local_service.scoring_uri}")
```

呼叫 `Model.deploy()` 可能需要幾分鐘的時間。 在您一開始部署之後，使用 `update()` 方法，而不是從頭開始是更有效率的作法。 請參閱 [更新已部署的 web 服務](how-to-deploy-update-web-service.md)。

### <a name="test-your-local-deployment"></a>測試您的本機部署

當您執行先前的部署腳本時，它會輸出您可以張貼資料以進行評分 (例如) 的 URI `http://localhost:6789/score` 。 下列範例顯示的腳本會將範例資料評分為 `"sklearn-mnist-local"` 本機部署的模型。 如果經過適當定型，則會推斷出 `normalized_pixel_values` 應解釋為 "2" 的模型。 

```python
import requests

normalized_pixel_values = "[\
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.5, 0.5, 0.7, 1.0, 1.0, 0.6, 0.4, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.7, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.9, 0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.7, 1.0, 1.0, 1.0, 0.8, 0.6, 0.7, 1.0, 1.0, 0.5, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.2, 1.0, 1.0, 0.8, 0.1, 0.0, 0.0, 0.0, 0.8, 1.0, 0.5, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.3, 1.0, 0.8, 0.1, 0.0, 0.0, 0.0, 0.5, 1.0, 1.0, 0.3, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.1, 0.1, 0.0, 0.0, 0.0, 0.0, 0.8, 1.0, 1.0, 0.3, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.5, 1.0, 1.0, 0.8, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.3, 1.0, 1.0, 0.9, 0.2, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.5, 1.0, 1.0, 0.6, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.7, 1.0, 1.0, 0.6, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.1, 0.9, 1.0, 0.9, 0.1, \
0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.8, 1.0, 1.0, 0.6, \
0.6, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.3, 1.0, 1.0, 0.7, \
0.7, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.1, 0.8, 1.0, 1.0, \
1.0, 0.6, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.5, 1.0, 1.0, \
1.0, 0.7, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, 1.0, \
1.0, 1.0, 0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 1.0, \
1.0, 1.0, 1.0, 0.2, 0.1, 0.1, 0.1, 0.1, 0.0, 0.0, 0.0, 0.1, 0.1, 0.1, 0.6, 0.6, 0.6, 0.6, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.7, 0.6, 0.7, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.5, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.7, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 0.7, 0.5, 0.5, 0.2, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.5, 0.5, 0.5, 0.5, 0.7, 1.0, 1.0, 1.0, 0.6, 0.5, 0.5, 0.2, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, \
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]"

input_data = "{\"data\": [" + normalized_pixel_values + "]}"

headers = {'Content-Type': 'application/json'}

scoring_uri = "http://localhost:6789/score"
resp = requests.post(scoring_uri, input_data, headers=headers)

print("Should be predicted as '2'")
print("prediction:", resp.text)
```

## <a name="download-and-run-your-model-directly"></a>直接下載並執行您的模型

使用 Docker 將您的模型部署為 web 服務是最常見的選項時，您可能也會想要使用本機 Python 腳本直接執行程式碼。 您將需要兩個重要的元素： 

- 模型本身
- 模型所依賴的相依性 

您可以完成下載模型：  

- 從入口網站選擇 [**模型**] 索引標籤，選取想要的模型，然後從 [**詳細資料**] 頁面選擇 [**下載**]
- 從命令列使用 `az ml model download` (參閱 [模型下載參考](/cli/azure/ext/azure-cli-ml/ml/model?view=azure-cli-latest#ext_azure_cli_ml_az_ml_model_download&preserve-view=false)) 
- 使用 Python SDK，藉由使用 `Model.download()` 方法 (查看 [模型 API 參考](/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#download-target-dir------exist-ok-false--exists-ok-none-&preserve-view=false)) 

Azure 模型是一或多個已序列化的 Python 物件，封裝為 Python pickle 檔案， (**>model.pkl** 延伸模組) 。 Pickle 檔案的內容取決於用來定型模型的 ML 程式庫或技術。 例如，透過教學課程中的模型，您可以使用下列內容載入模型：

```python
import pickle

with open('sklearn_mnist_model.pkl', 'rb') as f : 
    logistic_model = pickle.load(f, encoding='latin1')
```

相依性一律很難使用，尤其是在機器學習中，通常會有特定版本需求的 dizzying web。 您可以使用類別的方法，在本機電腦上重新建立 Azure Machine Learning 環境，做為完整的 conda 環境或 Docker 映射 `Environment` `build_local()` 。 

```python
ws = Workspace.from_config()
myenv = Environment.get(workspace=ws, name="tutorial-env", version="1")
myenv.build_local(workspace=ws, useDocker=False) #Creates conda env
```

如果您將 `build_local()` 的引數設定 `useDocker` 為 `True` ，此函式會建立 Docker 映射，而不是 conda 環境。 如果您想要更多控制，可以使用 `Environment` 的 `save_to_directory()` 方法，它會將 **conda_dependencies. yml** 和 **azureml_environment.js** 寫入定義檔，讓您可以微調並用作擴充的基礎。 

`Environment`類別有一些其他方法可在您的計算硬體、Azure 工作區和 Docker 映射之間同步處理環境。 如需詳細資訊，請參閱[ `Environment` API 參考](/python/api/azureml-core/azureml.core.environment(class))。

一旦您下載模型並解決其相依性之後，就不會有任何 Azure 定義的限制來執行評分、微調模型、使用轉移學習等等。 

## <a name="upload-a-retrained-model-to-azure-machine-learning"></a>將重新定型模型上傳至 Azure Machine Learning

如果您有本機定型或重新定型的模型，您可以向 Azure 註冊。 註冊之後，您可以繼續使用 Azure 計算進行調整，或使用 Azure 功能（例如 [Azure Kubernetes Service](how-to-deploy-azure-kubernetes-service.md) 或 [Triton 推斷伺服器 (Preview) ](how-to-deploy-with-triton.md)）來部署它。

若要搭配 Azure Machine Learning 的 Python SDK 使用，模型必須以 **>model.pkl** 檔形式儲存為 pickle (格式的序列化 Python 物件) 且必須執行傳回 `predict(data)` JSON 可序列化物件的方法。 例如，您可以使用下列程式儲存本機定型的 scikit-learn 學習糖尿病模型： 

```python
import joblib

from sklearn.datasets import load_diabetes
from sklearn.linear_model import Ridge

dataset_x, dataset_y = load_diabetes(return_X_y=True)

sk_model = Ridge().fit(dataset_x, dataset_y)

joblib.dump(sk_model, "sklearn_regression_model.pkl")
```

若要讓模型可在 Azure 中使用，您可以接著使用 `register()` 類別的方法 `Model` ：

```python
from azureml.core.model import Model

model = Model.register(model_path="sklearn_regression_model.pkl",
                       model_name="sklearn_regression_model",
                       tags={'area': "diabetes", 'type': "regression"},
                       description="Ridge regression model to predict diabetes",
                       workspace=ws)
```

然後，您可以在 Azure Machine Learning 的 [ **模型** ] 索引標籤中找到新註冊的模型：

:::image type="content" source="media/how-to-deploy-local/registered-model.png" alt-text="顯示已上傳模型的 Azure Machine Learning 模型] 索引標籤的螢幕擷取畫面。":::

如需上傳和更新模型和環境的詳細資訊，請參閱在 [本機註冊模型和使用 advanced 使用方式部署](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-local/register-model-deploy-local-advanced.ipynb)。

## <a name="next-steps"></a>後續步驟

- 如需管理環境的詳細資訊，請參閱 [建立 & 在 Azure Machine Learning 中使用軟體環境](how-to-use-environments.md)
- 若要瞭解如何從資料存放區存取資料，請參閱 [連接到 Azure 上的儲存體服務](how-to-access-data.md)
---
title: 範例 Jupyter Notebook
titleSuffix: Azure Machine Learning
description: 了解如何找到及使用 Juypter Notebook，其設計旨在協助您探索 SDK，並且可供您自己的機器學習服務專案作為模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 3f5cb2831a58ce9d0d30039b80815e281401e883
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2020
ms.locfileid: "94542553"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>使用 Jupyter Notebook 探索 Azure Machine Learning

> [!NOTE] 
> 如需範例的社群導向存放庫，請參閱 https://github.com/Azure/azureml-examples 。

[範例 Azure Machine Learning Notebook 存放庫](https://github.com/azure/machinelearningnotebooks)包含最新的 Azure Machine Learning Python SDK 範例。 這些 Juypter Notebook 可協助您探索 SDK，並且可供您自己的機器學習服務專案作為模型。

本文說明如何從下列環境存取存放庫：

- [Azure Machine Learning 計算執行個體](#notebookvm)
- [自攜 Notebook 伺服器](#byo)
- [資料科學虛擬機器](#dsvm)

> [!NOTE]
> 在複製存放庫之後，您可在 **tutorials** 資料夾中找到教學課程 Notebook，並且可在 **how-to-use-azureml** 資料夾中找到特定功能的 Notebook。

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>取得 Azure Machine Learning 計算執行個體的範例

要開始使用範例，最簡單的方式是完成[教學課程：設定環境和工作區](tutorial-1st-experiment-sdk-setup.md)。 完成後，您將會有已預先載入 SDK 與範例存放庫的專用 Notebook 伺服器。 您不需要下載或安裝。

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>在 Notebook 伺服器上取得範例

如果您想要使用攜帶的 Notebook 伺服器進行本機開發，請遵循下列步驟：

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

這些指示會安裝快速入門和教學課程 Notebook 所需的基底 SDK 套件。 若要使用其他範例 Notebook，可能需要安裝額外的元件。 如需詳細資訊，請參閱[安裝適用於 Python 的 Azure Machine Learning SDK](/python/api/overview/azure/ml/install)。

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>在 DSVM 上取得範例

資料科學虛擬機器 (DSVM) 是專為進行資料科學而建置的自訂 VM 映像。 如果您[建立 DSVM](how-to-configure-environment.md#dsvm)，系統將為您安裝及設定 SDK 和 Notebook 伺服器。 不過，您仍須建立工作區並複製範例存放庫。

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>後續步驟

瀏覽[範例 Notebook](https://github.com/Azure/MachineLearningNotebooks) 以探索 Azure Machine Learning 的功用。

如需更多 GitHub 範例專案和範例，請參閱下列存放庫：
+ [Azure/azureml-examples](https://github.com/Azure/azureml-examples)
+ [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
+ [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

試試以下教學課程︰

- [使用 MNIST 定型和部署影像分類模型](tutorial-train-models-with-aml.md)

- [準備資料並使用自動化機器學習服務以 NYC 計程車資料集定型迴歸模型](tutorial-auto-train-models.md)
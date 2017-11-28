---
title: az Azure Blob Storage aaaMove adatok tooand |} Microsoft Docs
description: "Helyezze át az adatokat tooand Azure Blob Storage-ból"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;sachouks
ms.openlocfilehash: e12b8c157955195e826f8b108075afaf25ca7bd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage"></a><span data-ttu-id="27231-103">Helyezze át az adatokat tooand Azure Blob Storage-ból</span><span class="sxs-lookup"><span data-stu-id="27231-103">Move data tooand from Azure Blob Storage</span></span>
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="27231-104">Az ideális módszer attól függ, hogy a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="27231-104">Which method is best for you depends on your scenario.</span></span> <span data-ttu-id="27231-105">Hello [forgatókönyvek az Azure Machine Learning speciális elemzésekre](machine-learning-data-science-plan-sample-scenarios.md) cikk segít meghatározni, hello erőforrások különböző adatok tudományos munkafolyamatokat advanced analytics folyamat hello használatban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="27231-105">hello [Scenarios for advanced analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) article helps you determine hello resources you need for a variety of data science workflows used in hello advanced analytics process.</span></span>

> [!NOTE]
> <span data-ttu-id="27231-106">A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="27231-106">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

<span data-ttu-id="27231-107">Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) számára:</span><span class="sxs-lookup"><span data-stu-id="27231-107">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to:</span></span> 

* <span data-ttu-id="27231-108">Hozzon létre, és egy folyamatot, amely letölti az adatokat az Azure blob storage ütemezése</span><span class="sxs-lookup"><span data-stu-id="27231-108">create and schedule a pipeline that downloads data from Azure blob storage,</span></span> 
* <span data-ttu-id="27231-109">Adja át tooa közzé az Azure Machine Learning webszolgáltatáshoz,</span><span class="sxs-lookup"><span data-stu-id="27231-109">pass it tooa published Azure Machine Learning web service,</span></span> 
* <span data-ttu-id="27231-110">hello prediktív elemzési eredményeket, és</span><span class="sxs-lookup"><span data-stu-id="27231-110">receive hello predictive analytics results, and</span></span> 
* <span data-ttu-id="27231-111">hello eredmények toostorage feltöltése.</span><span class="sxs-lookup"><span data-stu-id="27231-111">upload hello results toostorage.</span></span> 

<span data-ttu-id="27231-112">További információkért lásd: [létrehozása az Azure Data Factory és az Azure Machine Learning a prediktív folyamatok](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="27231-112">For more information, see [Create predictive pipelines using Azure Data Factory and Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27231-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="27231-113">Prerequisites</span></span>
<span data-ttu-id="27231-114">Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és hello megfelelő kulcs fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="27231-114">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="27231-115">Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="27231-115">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="27231-116">tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="27231-116">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="27231-117">A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="27231-117">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>


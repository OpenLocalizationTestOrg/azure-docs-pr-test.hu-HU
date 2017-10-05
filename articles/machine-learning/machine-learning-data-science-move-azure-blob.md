---
title: "Adatok áthelyezése az és az Azure Blob Storage |} Microsoft Docs"
description: "Adatok áthelyezése Azure Blob Storage-tárolóba vagy onnan máshová"
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
ms.openlocfilehash: d9a626cccd3cdfcdc85f000bd3192aef2881e9a6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-and-from-azure-blob-storage"></a><span data-ttu-id="d04c2-103">Adatok áthelyezése, és az Azure-Blobtárolóba</span><span class="sxs-lookup"><span data-stu-id="d04c2-103">Move data to and from Azure Blob Storage</span></span>
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="d04c2-104">Az ideális módszer attól függ, hogy a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="d04c2-104">Which method is best for you depends on your scenario.</span></span> <span data-ttu-id="d04c2-105">A [forgatókönyvek az Azure Machine Learning speciális elemzésekre](machine-learning-data-science-plan-sample-scenarios.md) cikk segít meghatározni, a különböző adatok tudományos munkafolyamatokat a speciális elemzés folyamat használja a szükséges erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d04c2-105">The [Scenarios for advanced analytics in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md) article helps you determine the resources you need for a variety of data science workflows used in the advanced analytics process.</span></span>

> [!NOTE]
> <span data-ttu-id="d04c2-106">Az Azure blob storage teljes bevezetéséhez hivatkozik [Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és [Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="d04c2-106">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

<span data-ttu-id="d04c2-107">Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) számára:</span><span class="sxs-lookup"><span data-stu-id="d04c2-107">As an alternative, you can use [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) to:</span></span> 

* <span data-ttu-id="d04c2-108">Hozzon létre, és egy folyamatot, amely letölti az adatokat az Azure blob storage ütemezése</span><span class="sxs-lookup"><span data-stu-id="d04c2-108">create and schedule a pipeline that downloads data from Azure blob storage,</span></span> 
* <span data-ttu-id="d04c2-109">Adja át azt egy közzétett Azure Machine Learning webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d04c2-109">pass it to a published Azure Machine Learning web service,</span></span> 
* <span data-ttu-id="d04c2-110">a prediktív elemzési eredményeket, és</span><span class="sxs-lookup"><span data-stu-id="d04c2-110">receive the predictive analytics results, and</span></span> 
* <span data-ttu-id="d04c2-111">Töltse fel az eredményeket a tároló.</span><span class="sxs-lookup"><span data-stu-id="d04c2-111">upload the results to storage.</span></span> 

<span data-ttu-id="d04c2-112">További információkért lásd: [létrehozása az Azure Data Factory és az Azure Machine Learning a prediktív folyamatok](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="d04c2-112">For more information, see [Create predictive pipelines using Azure Data Factory and Azure Machine Learning](../data-factory/data-factory-azure-ml-batch-execution-activity.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d04c2-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d04c2-113">Prerequisites</span></span>
<span data-ttu-id="d04c2-114">Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és a megfelelő kulcsot adott fiók rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d04c2-114">This document assumes that you have an Azure subscription, a storage account, and the corresponding storage key for that account.</span></span> <span data-ttu-id="d04c2-115">Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.</span><span class="sxs-lookup"><span data-stu-id="d04c2-115">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="d04c2-116">Állítsa be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d04c2-116">To set up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d04c2-117">A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="d04c2-117">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>


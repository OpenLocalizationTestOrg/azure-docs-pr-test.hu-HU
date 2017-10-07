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
# <a name="move-data-tooand-from-azure-blob-storage"></a>Helyezze át az adatokat tooand Azure Blob Storage-ból
[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this tooseparate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Az ideális módszer attól függ, hogy a forgatókönyv. Hello [forgatókönyvek az Azure Machine Learning speciális elemzésekre](machine-learning-data-science-plan-sample-scenarios.md) cikk segít meghatározni, hello erőforrások különböző adatok tudományos munkafolyamatokat advanced analytics folyamat hello használatban van szüksége.

> [!NOTE]
> A teljes bemutatása tooAzure blob-tároló, tekintse meg túl[Azure Blob alapjai](../storage/blobs/storage-dotnet-how-to-use-blobs.md) és túl[Azure Blob szolgáltatás](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Alternatív megoldásként használható [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) számára: 

* Hozzon létre, és egy folyamatot, amely letölti az adatokat az Azure blob storage ütemezése 
* Adja át tooa közzé az Azure Machine Learning webszolgáltatáshoz, 
* hello prediktív elemzési eredményeket, és 
* hello eredmények toostorage feltöltése. 

További információkért lásd: [létrehozása az Azure Data Factory és az Azure Machine Learning a prediktív folyamatok](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Előfeltételek
Jelen dokumentum céljából feltételezzük, hogy az Azure-előfizetéssel, a tárfiók és hello megfelelő kulcs fiók rendelkezik. Adatok feltöltése/letöltése, előtt ismernie kell az Azure storage-fiók nevét és a fiók kulcs.

* tooset be Azure-előfizetéssel, lásd: [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* A storage-fiók létrehozásával és az első fiók és a fontos információkat lásd: [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md).


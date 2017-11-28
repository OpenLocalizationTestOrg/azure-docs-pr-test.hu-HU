---
title: "Listázza az összes, az Azure importálási/exportálási feladatok |} MicrosoftDocs"
description: "Megtudhatja, hogyan listázza az összes előfizetés az Azure Import/Export szolgáltatás feladatok."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f2e619be-1bbd-4a54-9472-9e2f70a83b64
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 1977bfc0e516088310f45ecdd960287eeed2c2d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a><span data-ttu-id="3e175-103">Az Azure Import/Export szolgáltatás feladatok számbavétele</span><span class="sxs-lookup"><span data-stu-id="3e175-103">Enumerating jobs in the Azure Import/Export service</span></span>
<span data-ttu-id="3e175-104">Felsorolni az előfizetés levő összes feladatnak, hívja meg a [lista feladatok](/rest/api/storageimportexport/jobs#Jobs_List) műveletet.</span><span class="sxs-lookup"><span data-stu-id="3e175-104">To enumerate all jobs in a subscription, call the [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) operation.</span></span> <span data-ttu-id="3e175-105">`List Jobs`feladatok, valamint a következő attribútumok listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="3e175-105">`List Jobs` returns a list of jobs as well as the following attributes:</span></span>

-   <span data-ttu-id="3e175-106">A feladat (importálása vagy exportálása) típusa</span><span class="sxs-lookup"><span data-stu-id="3e175-106">The type of job (Import or Export)</span></span>

-   <span data-ttu-id="3e175-107">A feladat jelenlegi állapota</span><span class="sxs-lookup"><span data-stu-id="3e175-107">The current job state</span></span>

-   <span data-ttu-id="3e175-108">A feladat társított storage-fiók</span><span class="sxs-lookup"><span data-stu-id="3e175-108">The job's associated storage account</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e175-109">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e175-109">Next steps</span></span>

* [<span data-ttu-id="3e175-110">Az Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="3e175-110">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)

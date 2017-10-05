---
title: "Szakítsa meg, és az Azure importálási/exportálási feladatok törlése |} Microsoft Docs"
description: "Megtudhatja, hogyan megszakításához és a Microsoft Azure Import/Export szolgáltatás feladatok törlése."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: e0a7ff391e5a03ed563912dea54c7cfe73111bcf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="4d583-103">A megszakítás és az Azure importálási/exportálási feladatok törlése</span><span class="sxs-lookup"><span data-stu-id="4d583-103">Canceling and deleting Azure Import/Export jobs</span></span>

<span data-ttu-id="4d583-104">Kérheti, hogy a feladatok megszakadnak ahhoz, hogy az a `Packaging` állapot meghívásával a [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet és a beállítás a `CancelRequested` elem `true`.</span><span class="sxs-lookup"><span data-stu-id="4d583-104">You can request that a job be cancelled before it is in the `Packaging` state by calling the [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and setting the `CancelRequested` element to `true`.</span></span> <span data-ttu-id="4d583-105">A feladat meg lesz szakítva legjobb alapon.</span><span class="sxs-lookup"><span data-stu-id="4d583-105">The job will be cancelled on a best-effort basis.</span></span> <span data-ttu-id="4d583-106">Ha meghajtók jelenleg másolja át az adatokat, adatok továbbra is helyezhető át, akkor is megszakítását kérték.</span><span class="sxs-lookup"><span data-stu-id="4d583-106">If drives are in the process of transferring data, data may continue to be transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="4d583-107">A megszakított feladatok helyezi át a `Completed` állapot és 90 nap, amikor a rendszer törli azt meg kell őrizni.</span><span class="sxs-lookup"><span data-stu-id="4d583-107">A cancelled job will move to the `Completed` state and be kept for 90 days, at which point it will be deleted.</span></span>

 <span data-ttu-id="4d583-108">Törli a feladatot, hívja meg a [törlése feladat](/rest/api/storageimportexport/jobs#Jobs_Delete) művelet előtt a feladat rendelkezik-e kiadva (*azaz*, amíg a feladat a `Creating` állapot).</span><span class="sxs-lookup"><span data-stu-id="4d583-108">To delete a job, call the [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before the job has shipped (*i.e.*, while the job is in the `Creating` state).</span></span> <span data-ttu-id="4d583-109">Törölheti is a feladat a esetén a `Completed` állapotát.</span><span class="sxs-lookup"><span data-stu-id="4d583-109">You can also delete a job when it is in the `Completed` state.</span></span> <span data-ttu-id="4d583-110">Egy feladat törlését követően annak adatait és állapotát már nincs a REST API-t vagy az Azure-portálon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="4d583-110">After a job has been deleted, its information and status are no longer accessible via the REST API or the Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d583-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d583-111">Next steps</span></span>

* [<span data-ttu-id="4d583-112">Az Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="4d583-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)

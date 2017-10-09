---
title: "aaaCancel és az Azure importálási/exportálási feladatok törlése |} Microsoft Docs"
description: "Ismerje meg, hogyan toocancel és törlési feladatok a Microsoft Azure Import/Export szolgáltatás hello."
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
ms.openlocfilehash: 5d2aba510dafd0ca9a10f5643f721e7059a6a8f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="d035d-103">A megszakítás és az Azure importálási/exportálási feladatok törlése</span><span class="sxs-lookup"><span data-stu-id="d035d-103">Canceling and deleting Azure Import/Export jobs</span></span>

<span data-ttu-id="d035d-104">Kérheti, hogy a feladat szakítható meg, mielőtt hello `Packaging` állapot szerint hívó hello [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet és a beállítás hello `CancelRequested` elem túl`true`.</span><span class="sxs-lookup"><span data-stu-id="d035d-104">You can request that a job be cancelled before it is in hello `Packaging` state by calling hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and setting hello `CancelRequested` element too`true`.</span></span> <span data-ttu-id="d035d-105">hello feladat megszakad legjobb alapon.</span><span class="sxs-lookup"><span data-stu-id="d035d-105">hello job will be cancelled on a best-effort basis.</span></span> <span data-ttu-id="d035d-106">Ha meghajtók adatátvitel hello folyamatán, adatok megszakítását kérték után is át toobe továbbra is.</span><span class="sxs-lookup"><span data-stu-id="d035d-106">If drives are in hello process of transferring data, data may continue toobe transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="d035d-107">A megszakított feladatok áthelyezi toohello `Completed` állapot és 90 nap, amikor a rendszer törli azt meg kell őrizni.</span><span class="sxs-lookup"><span data-stu-id="d035d-107">A cancelled job will move toohello `Completed` state and be kept for 90 days, at which point it will be deleted.</span></span>

 <span data-ttu-id="d035d-108">egy feladat toodelete hívás hello [törlése feladat](/rest/api/storageimportexport/jobs#Jobs_Delete) művelet előtt hello feladat van kiadva (*azaz*, hello feladat hello közben `Creating` állapot).</span><span class="sxs-lookup"><span data-stu-id="d035d-108">toodelete a job, call hello [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before hello job has shipped (*i.e.*, while hello job is in hello `Creating` state).</span></span> <span data-ttu-id="d035d-109">Törölheti a feladatok is ha hello `Completed` állapotát.</span><span class="sxs-lookup"><span data-stu-id="d035d-109">You can also delete a job when it is in hello `Completed` state.</span></span> <span data-ttu-id="d035d-110">Egy feladat törlését követően annak adatait és állapotát már nem érhetők el hello REST API-n keresztül, vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d035d-110">After a job has been deleted, its information and status are no longer accessible via hello REST API or hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d035d-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d035d-111">Next steps</span></span>

* [<span data-ttu-id="d035d-112">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="d035d-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)

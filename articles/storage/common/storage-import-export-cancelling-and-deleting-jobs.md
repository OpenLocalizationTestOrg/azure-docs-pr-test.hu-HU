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
ms.openlocfilehash: 13456a8e7652850baacb53730cc7bb1520b0a4c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a><span data-ttu-id="87e2b-103">A megszakítás és az Azure importálási/exportálási feladatok törlése</span><span class="sxs-lookup"><span data-stu-id="87e2b-103">Canceling and deleting Azure Import/Export jobs</span></span>

 <span data-ttu-id="87e2b-104">hello van, hogy egy feladat azt megelőzően lehet megszakítani toorequest `Packaging` állapot, a hívás hello [frissítés Feladattulajdonság](/rest/api/storageimportexport/jobs#Jobs_Update) művelet és a set hello `CancelRequested` elem túl`true`.</span><span class="sxs-lookup"><span data-stu-id="87e2b-104">toorequest that a job be canceled before it is in hello `Packaging` state, call hello [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operation and set hello `CancelRequested` element too`true`.</span></span> <span data-ttu-id="87e2b-105">hello feladat megszakítása van, elérhető legjobb alapon.</span><span class="sxs-lookup"><span data-stu-id="87e2b-105">hello job is canceled on a best-effort basis.</span></span> <span data-ttu-id="87e2b-106">Ha meghajtók adatátvitel hello folyamatán, adatok megszakítását kérték után is át toobe továbbra is.</span><span class="sxs-lookup"><span data-stu-id="87e2b-106">If drives are in hello process of transferring data, data may continue toobe transferred even after cancellation has been requested.</span></span>

 <span data-ttu-id="87e2b-107">A megszakított feladatok az áthelyezett toohello `Completed` állapot, és ekkor törölt 90 napig maradnak.</span><span class="sxs-lookup"><span data-stu-id="87e2b-107">A canceled job is moved toohello `Completed` state and is kept for 90 days, at which point it is deleted.</span></span>

 <span data-ttu-id="87e2b-108">egy feladat toodelete hívás hello [törlése feladat](/rest/api/storageimportexport/jobs#Jobs_Delete) művelet előtt hello feladat van kiadva (Ez azt jelenti, hogy közben hello feladat hello `Creating` állapot).</span><span class="sxs-lookup"><span data-stu-id="87e2b-108">toodelete a job, call hello [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) operation before hello job has shipped (that is, while hello job is in hello `Creating` state).</span></span> <span data-ttu-id="87e2b-109">Törölheti a feladatok is ha hello `Completed` állapotát.</span><span class="sxs-lookup"><span data-stu-id="87e2b-109">You can also delete a job when it is in hello `Completed` state.</span></span> <span data-ttu-id="87e2b-110">Egy feladat törlését követően annak adatait és állapotát már nincs hello REST API-n keresztül érhető el, vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="87e2b-110">After a job is deleted, its information and status are no longer accessible via hello REST API or hello Azure portal.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87e2b-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="87e2b-111">Next steps</span></span>

* [<span data-ttu-id="87e2b-112">Hello Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="87e2b-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)

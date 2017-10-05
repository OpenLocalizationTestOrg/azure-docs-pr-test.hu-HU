---
title: "Azure Batch-készlet törlése befejeződésének eseményét. |} Microsoft Docs"
description: "Útmutató a Batch-készlet törlése befejeződésének eseményét."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-complete-event"></a><span data-ttu-id="bc51f-103">Készlet törlés befejeződésének eseményét.</span><span class="sxs-lookup"><span data-stu-id="bc51f-103">Pool delete complete event</span></span>

 <span data-ttu-id="bc51f-104">Ez az esemény is ki lesz adva a készlet törlési művelet befejeződésekor.</span><span class="sxs-lookup"><span data-stu-id="bc51f-104">This event is emitted when a pool delete operation has completed.</span></span>

 <span data-ttu-id="bc51f-105">A következő példa bemutatja a készletben törlés befejeződésének eseményét törzsét.</span><span class="sxs-lookup"><span data-stu-id="bc51f-105">The following example shows the body of a pool delete complete event.</span></span>

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|<span data-ttu-id="bc51f-106">Elem</span><span class="sxs-lookup"><span data-stu-id="bc51f-106">Element</span></span>|<span data-ttu-id="bc51f-107">Típus</span><span class="sxs-lookup"><span data-stu-id="bc51f-107">Type</span></span>|<span data-ttu-id="bc51f-108">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bc51f-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="bc51f-109">id</span><span class="sxs-lookup"><span data-stu-id="bc51f-109">id</span></span>|<span data-ttu-id="bc51f-110">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bc51f-110">String</span></span>|<span data-ttu-id="bc51f-111">A készlet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="bc51f-111">The id of the pool.</span></span>|
|<span data-ttu-id="bc51f-112">startTime</span><span class="sxs-lookup"><span data-stu-id="bc51f-112">startTime</span></span>|<span data-ttu-id="bc51f-113">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="bc51f-113">DateTime</span></span>|<span data-ttu-id="bc51f-114">A készlet törlése indulásakor.</span><span class="sxs-lookup"><span data-stu-id="bc51f-114">The time the pool delete started.</span></span>|
|<span data-ttu-id="bc51f-115">Befejezés időpontja</span><span class="sxs-lookup"><span data-stu-id="bc51f-115">endTime</span></span>|<span data-ttu-id="bc51f-116">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="bc51f-116">DateTime</span></span>|<span data-ttu-id="bc51f-117">Az az idő a készlet törlése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="bc51f-117">The time the pool delete completed.</span></span>|

## <a name="remarks"></a><span data-ttu-id="bc51f-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bc51f-118">Remarks</span></span>
<span data-ttu-id="bc51f-119">Állapotok és hibakódok készlet átméretezés kapcsolatos további információkért lásd: [címkészlet törlése fiókból](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span><span class="sxs-lookup"><span data-stu-id="bc51f-119">For more information about states and error codes for pool resize operation, see [Delete a pool from an account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span></span>
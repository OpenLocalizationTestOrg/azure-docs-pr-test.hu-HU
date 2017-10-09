---
title: "AAA \"Azure Batch-készlet törlése befejeződésének eseményét. |} Microsoft dokumentumok\""
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
ms.openlocfilehash: 494c371e48ebfb1bf3d2973a7401829a939ba141
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-complete-event"></a><span data-ttu-id="1c74d-103">Készlet törlés befejeződésének eseményét.</span><span class="sxs-lookup"><span data-stu-id="1c74d-103">Pool delete complete event</span></span>

 <span data-ttu-id="1c74d-104">Ez az esemény is ki lesz adva a készlet törlési művelet befejeződésekor.</span><span class="sxs-lookup"><span data-stu-id="1c74d-104">This event is emitted when a pool delete operation has completed.</span></span>

 <span data-ttu-id="1c74d-105">hello következő példa bemutatja a készletben törlés befejeződésének eseményét hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="1c74d-105">hello following example shows hello body of a pool delete complete event.</span></span>

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|<span data-ttu-id="1c74d-106">Elem</span><span class="sxs-lookup"><span data-stu-id="1c74d-106">Element</span></span>|<span data-ttu-id="1c74d-107">Típus</span><span class="sxs-lookup"><span data-stu-id="1c74d-107">Type</span></span>|<span data-ttu-id="1c74d-108">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1c74d-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="1c74d-109">id</span><span class="sxs-lookup"><span data-stu-id="1c74d-109">id</span></span>|<span data-ttu-id="1c74d-110">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1c74d-110">String</span></span>|<span data-ttu-id="1c74d-111">hello készlet hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="1c74d-111">hello id of hello pool.</span></span>|
|<span data-ttu-id="1c74d-112">startTime</span><span class="sxs-lookup"><span data-stu-id="1c74d-112">startTime</span></span>|<span data-ttu-id="1c74d-113">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="1c74d-113">DateTime</span></span>|<span data-ttu-id="1c74d-114">hello hello készlet törlése indulásakor.</span><span class="sxs-lookup"><span data-stu-id="1c74d-114">hello time hello pool delete started.</span></span>|
|<span data-ttu-id="1c74d-115">endTime</span><span class="sxs-lookup"><span data-stu-id="1c74d-115">endTime</span></span>|<span data-ttu-id="1c74d-116">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="1c74d-116">DateTime</span></span>|<span data-ttu-id="1c74d-117">hello idő hello készlet törlése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1c74d-117">hello time hello pool delete completed.</span></span>|

## <a name="remarks"></a><span data-ttu-id="1c74d-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1c74d-118">Remarks</span></span>
<span data-ttu-id="1c74d-119">Állapotok és hibakódok készlet átméretezés kapcsolatos további információkért lásd: [címkészlet törlése fiókból](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span><span class="sxs-lookup"><span data-stu-id="1c74d-119">For more information about states and error codes for pool resize operation, see [Delete a pool from an account](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).</span></span>
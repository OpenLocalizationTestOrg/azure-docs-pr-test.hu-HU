---
title: "Azure Batch készlet törlése start esemény |} Microsoft Docs"
description: "Útmutató a Batch-készlet törlése esemény indítása."
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-start-event"></a><span data-ttu-id="f1f0d-103">Készlet törlése start esemény</span><span class="sxs-lookup"><span data-stu-id="f1f0d-103">Pool delete start event</span></span>

 <span data-ttu-id="f1f0d-104">Ez az esemény kibocsátott, amikor egy alkalmazáskészlet törlési művelete megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="f1f0d-104">This event is emitted when a pool delete operation has started.</span></span> <span data-ttu-id="f1f0d-105">Mivel a készlet törlése egy aszinkron esemény, egy készlet törlése teljes esemény a törlési művelet befejeződése után kell kibocsátott számíthat.</span><span class="sxs-lookup"><span data-stu-id="f1f0d-105">Since the pool delete is an asynchronous event, you can expect a pool delete complete event to be emitted once the delete operation completes.</span></span>

 <span data-ttu-id="f1f0d-106">A következő példa bemutatja a készlet törlése start esemény törzsét.</span><span class="sxs-lookup"><span data-stu-id="f1f0d-106">The following example shows the body of a pool delete start event.</span></span>

```
{
    "id": "myPool1"
}
```

|<span data-ttu-id="f1f0d-107">Elem</span><span class="sxs-lookup"><span data-stu-id="f1f0d-107">Element</span></span>|<span data-ttu-id="f1f0d-108">Típus</span><span class="sxs-lookup"><span data-stu-id="f1f0d-108">Type</span></span>|<span data-ttu-id="f1f0d-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f1f0d-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="f1f0d-110">id</span><span class="sxs-lookup"><span data-stu-id="f1f0d-110">id</span></span>|<span data-ttu-id="f1f0d-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="f1f0d-111">String</span></span>|<span data-ttu-id="f1f0d-112">A készlet azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f1f0d-112">The id of the pool.</span></span>|
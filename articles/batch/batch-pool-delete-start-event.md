---
title: "AAA \"Azure Batch készlet törlése start esemény |} Microsoft dokumentumok\""
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
ms.openlocfilehash: 79bb28bffc760a49cc0a95062f5086dc96c6a795
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="pool-delete-start-event"></a><span data-ttu-id="878f3-103">Készlet törlése start esemény</span><span class="sxs-lookup"><span data-stu-id="878f3-103">Pool delete start event</span></span>

 <span data-ttu-id="878f3-104">Ez az esemény kibocsátott, amikor egy alkalmazáskészlet törlési művelete megkezdődött.</span><span class="sxs-lookup"><span data-stu-id="878f3-104">This event is emitted when a pool delete operation has started.</span></span> <span data-ttu-id="878f3-105">Mivel hello készlet törlése egy aszinkron esemény, számíthat egy alkalmazáskészlet törlés befejeződésének eseményét. toobe kibocsátott után hello törlésének művelete befejeződött.</span><span class="sxs-lookup"><span data-stu-id="878f3-105">Since hello pool delete is an asynchronous event, you can expect a pool delete complete event toobe emitted once hello delete operation completes.</span></span>

 <span data-ttu-id="878f3-106">hello következő példa bemutatja egy készlet törlése start esemény hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="878f3-106">hello following example shows hello body of a pool delete start event.</span></span>

```
{
    "id": "myPool1"
}
```

|<span data-ttu-id="878f3-107">Elem</span><span class="sxs-lookup"><span data-stu-id="878f3-107">Element</span></span>|<span data-ttu-id="878f3-108">Típus</span><span class="sxs-lookup"><span data-stu-id="878f3-108">Type</span></span>|<span data-ttu-id="878f3-109">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="878f3-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="878f3-110">id</span><span class="sxs-lookup"><span data-stu-id="878f3-110">id</span></span>|<span data-ttu-id="878f3-111">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="878f3-111">String</span></span>|<span data-ttu-id="878f3-112">hello készlet hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="878f3-112">hello id of hello pool.</span></span>|

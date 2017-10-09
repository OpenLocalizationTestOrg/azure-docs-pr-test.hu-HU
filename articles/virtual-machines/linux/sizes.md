---
title: "az Azure-ban méretének aaaLinux VM |} Microsoft Docs"
description: "Linux virtuális gépek Azure-ban elérhető különböző méretű hello sorolja fel."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 56cbe0a0d7d34def07636edba74c4c699e336012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a><span data-ttu-id="7b944-103">Az Azure Linux virtuális gépek méretei</span><span class="sxs-lookup"><span data-stu-id="7b944-103">Sizes for Linux virtual machines in Azure</span></span>
<span data-ttu-id="7b944-104">Ez a cikk ismerteti hello elérhető méretek, és a Linux-alkalmazások és munkafolyamatok toorun használhatja az Azure virtuális gépek hello beállítások.</span><span class="sxs-lookup"><span data-stu-id="7b944-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Linux apps and workloads.</span></span> <span data-ttu-id="7b944-105">Amikor arra készül, toouse ezeket az erőforrásokat is biztosít telepítési szempontok toobe tudomást.</span><span class="sxs-lookup"><span data-stu-id="7b944-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span> <span data-ttu-id="7b944-106">Ez a cikk érhető el is [Windows virtuális gépek](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7b944-106">This article is also available for [Windows virtual machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


| <span data-ttu-id="7b944-107">Típus</span><span class="sxs-lookup"><span data-stu-id="7b944-107">Type</span></span>                     | <span data-ttu-id="7b944-108">Méretek</span><span class="sxs-lookup"><span data-stu-id="7b944-108">Sizes</span></span>           |    <span data-ttu-id="7b944-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="7b944-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="7b944-110">Általános célú</span><span class="sxs-lookup"><span data-stu-id="7b944-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="7b944-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span><span class="sxs-lookup"><span data-stu-id="7b944-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span></span>  | <span data-ttu-id="7b944-112">Kiegyensúlyozott processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="7b944-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="7b944-113">Tesztelés és fejlesztési kis toomedium adatbázisok és alacsony toomedium forgalom webkiszolgálók ideális.</span><span class="sxs-lookup"><span data-stu-id="7b944-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="7b944-114">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="7b944-115">FS, F</span><span class="sxs-lookup"><span data-stu-id="7b944-115">Fs, F</span></span>             | <span data-ttu-id="7b944-116">Magas processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="7b944-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="7b944-117">Közepes forgalom webkiszolgálók, hálózati berendezésekbe, fürdőbe folyamatok és alkalmazáskiszolgálók jó.</span><span class="sxs-lookup"><span data-stu-id="7b944-117">Good for medium traffic web servers, network appliances, bath processes, and application servers.</span></span>        |
| [<span data-ttu-id="7b944-118">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-118">Memory optimized</span></span>](sizes-memory.md)         | <span data-ttu-id="7b944-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="7b944-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="7b944-120">Magas memória-CPU aránya.</span><span class="sxs-lookup"><span data-stu-id="7b944-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="7b944-121">A relációs adatbázis-kiszolgálók, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="7b944-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="7b944-122">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-122">Storage optimized</span></span>](sizes-storage.md)        | <span data-ttu-id="7b944-123">Ls</span><span class="sxs-lookup"><span data-stu-id="7b944-123">Ls</span></span>                | <span data-ttu-id="7b944-124">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="7b944-124">High disk throughput and IO.</span></span> <span data-ttu-id="7b944-125">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="7b944-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="7b944-126">GPU</span><span class="sxs-lookup"><span data-stu-id="7b944-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="7b944-127">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="7b944-127">NV, NC</span></span>            | <span data-ttu-id="7b944-128">Speciális virtuális gépekre nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt.</span><span class="sxs-lookup"><span data-stu-id="7b944-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="7b944-129">Egy vagy több Feldolgozóegységekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="7b944-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="7b944-130">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="7b944-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="7b944-131">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="7b944-131">H, A8-11</span></span>          | <span data-ttu-id="7b944-132">A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel.</span><span class="sxs-lookup"><span data-stu-id="7b944-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br>

- <span data-ttu-id="7b944-133">Különböző méretű hello az árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="7b944-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span> 
- <span data-ttu-id="7b944-134">Az Azure-régiók Virtuálisgép-méretek rendelkezésre állását, lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="7b944-134">For availability of VM sizes in Azure regions, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
- <span data-ttu-id="7b944-135">általános korlátozások toosee Azure virtuális gépeken, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="7b944-135">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="7b944-136">További tudnivalók [Azure számítási egység (ACU)](../windows/acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="7b944-136">Learn more about how [Azure compute units (ACU)](../windows/acu.md) can help you compare compute performance across Azure SKUs.</span></span>


## <a name="rest-api"></a><span data-ttu-id="7b944-137">REST API-n</span><span class="sxs-lookup"><span data-stu-id="7b944-137">Rest API</span></span>

<span data-ttu-id="7b944-138">A Virtuálisgép-méretek hello REST API tooquery használatáról információkért lásd: hello következő:</span><span class="sxs-lookup"><span data-stu-id="7b944-138">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="7b944-139">Átméretezéséhez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="7b944-139">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="7b944-140">Az előfizetéshez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="7b944-140">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="7b944-141">A rendelkezésre állási csoportok elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="7b944-141">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="7b944-142">ACU</span><span class="sxs-lookup"><span data-stu-id="7b944-142">ACU</span></span>

<span data-ttu-id="7b944-143">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="7b944-143">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b944-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7b944-144">Next steps</span></span>

<span data-ttu-id="7b944-145">További tudnivalók hello másik Virtuálisgép-méretek elérhető:</span><span class="sxs-lookup"><span data-stu-id="7b944-145">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="7b944-146">Általános célú</span><span class="sxs-lookup"><span data-stu-id="7b944-146">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="7b944-147">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-147">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="7b944-148">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-148">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="7b944-149">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="7b944-149">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="7b944-150">GPU</span><span class="sxs-lookup"><span data-stu-id="7b944-150">GPU</span></span>](sizes-gpu.md)
- [<span data-ttu-id="7b944-151">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="7b944-151">High performance compute</span></span>](sizes-hpc.md)




---
title: "az Azure-ban méretének aaaWindows VM |} Microsoft Docs"
description: "Windows virtuális gépek Azure-ban elérhető különböző méretű hello sorolja fel."
services: virtual-machines-windows
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.openlocfilehash: 80bd8241b134031c41b56224e841c7557c4845cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a><span data-ttu-id="c8e53-103">Az Azure-ban a Windows virtuális gépek méretei</span><span class="sxs-lookup"><span data-stu-id="c8e53-103">Sizes for Windows virtual machines in Azure</span></span>

<span data-ttu-id="c8e53-104">Ez a cikk ismerteti a hello elérhető méretek, és beállítások hello toorun használhatja a Windows-alkalmazások és számítási feladatok Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="c8e53-104">This article describes hello available sizes and options for hello Azure virtual machines you can use toorun your Windows apps and workloads.</span></span> <span data-ttu-id="c8e53-105">Amikor arra készül, toouse ezeket az erőforrásokat is biztosít telepítési szempontok toobe tudomást.</span><span class="sxs-lookup"><span data-stu-id="c8e53-105">It also provides deployment considerations toobe aware of when you're planning toouse these resources.</span></span>  <span data-ttu-id="c8e53-106">Ez a cikk érhető el is [Linux virtuális gépek](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c8e53-106">This article is also available for [Linux virtual machines](../linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


| <span data-ttu-id="c8e53-107">Típus</span><span class="sxs-lookup"><span data-stu-id="c8e53-107">Type</span></span>                     | <span data-ttu-id="c8e53-108">Méretek</span><span class="sxs-lookup"><span data-stu-id="c8e53-108">Sizes</span></span>           |    <span data-ttu-id="c8e53-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="c8e53-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="c8e53-110">Általános célú</span><span class="sxs-lookup"><span data-stu-id="c8e53-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="c8e53-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span><span class="sxs-lookup"><span data-stu-id="c8e53-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span> | <span data-ttu-id="c8e53-112">Kiegyensúlyozott processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="c8e53-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="c8e53-113">Tesztelés és fejlesztési kis toomedium adatbázisok és alacsony toomedium forgalom webkiszolgálók ideális.</span><span class="sxs-lookup"><span data-stu-id="c8e53-113">Ideal for testing and development, small toomedium databases, and low toomedium traffic web servers.</span></span> |
| [<span data-ttu-id="c8e53-114">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="c8e53-115">FS, F</span><span class="sxs-lookup"><span data-stu-id="c8e53-115">Fs, F</span></span>             | <span data-ttu-id="c8e53-116">Magas processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="c8e53-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="c8e53-117">Alkalmas közepes adatforgalmú webkiszolgálók, hálózati berendezések, kötegfolyamatok és alkalmazáskiszolgálók számára.</span><span class="sxs-lookup"><span data-stu-id="c8e53-117">Good for medium traffic web servers, network appliances, batch processes, and application servers.</span></span>        |
| [<span data-ttu-id="c8e53-118">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-118">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)         | <span data-ttu-id="c8e53-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="c8e53-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="c8e53-120">Magas memória-CPU aránya.</span><span class="sxs-lookup"><span data-stu-id="c8e53-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="c8e53-121">A relációs adatbázis-kiszolgálók, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.</span><span class="sxs-lookup"><span data-stu-id="c8e53-121">Great for relational database servers, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="c8e53-122">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-122">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)        | <span data-ttu-id="c8e53-123">Ls</span><span class="sxs-lookup"><span data-stu-id="c8e53-123">Ls</span></span>                | <span data-ttu-id="c8e53-124">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="c8e53-124">High disk throughput and IO.</span></span> <span data-ttu-id="c8e53-125">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="c8e53-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="c8e53-126">GPU</span><span class="sxs-lookup"><span data-stu-id="c8e53-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="c8e53-127">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="c8e53-127">NV, NC</span></span>            | <span data-ttu-id="c8e53-128">Speciális virtuális gépekre nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt.</span><span class="sxs-lookup"><span data-stu-id="c8e53-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="c8e53-129">Egy vagy több Feldolgozóegységekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="c8e53-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="c8e53-130">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="c8e53-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="c8e53-131">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="c8e53-131">H, A8-11</span></span>          | <span data-ttu-id="c8e53-132">A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel.</span><span class="sxs-lookup"><span data-stu-id="c8e53-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br> 

- <span data-ttu-id="c8e53-133">Különböző méretű hello az árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span><span class="sxs-lookup"><span data-stu-id="c8e53-133">For information about pricing of hello various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).</span></span> 
- <span data-ttu-id="c8e53-134">általános korlátozások toosee Azure virtuális gépeken, lásd: [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="c8e53-134">toosee general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="c8e53-135">Tárolási költségek hello tárfiókban használt lapok számítása külön alapul.</span><span class="sxs-lookup"><span data-stu-id="c8e53-135">Storage costs are calculated separately based on used pages in hello storage account.</span></span> <span data-ttu-id="c8e53-136">További információkért [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/storage/).</span><span class="sxs-lookup"><span data-stu-id="c8e53-136">For details, [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/).</span></span>
- <span data-ttu-id="c8e53-137">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="c8e53-137">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>



## <a name="rest-api"></a><span data-ttu-id="c8e53-138">REST API-n</span><span class="sxs-lookup"><span data-stu-id="c8e53-138">Rest API</span></span>

<span data-ttu-id="c8e53-139">A Virtuálisgép-méretek hello REST API tooquery használatáról információkért lásd: hello következő:</span><span class="sxs-lookup"><span data-stu-id="c8e53-139">For information on using hello REST API tooquery for VM sizes, see hello following:</span></span>

- [<span data-ttu-id="c8e53-140">Átméretezéséhez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="c8e53-140">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="c8e53-141">Az előfizetéshez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="c8e53-141">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="c8e53-142">A rendelkezésre állási csoportok elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="c8e53-142">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="c8e53-143">ACU</span><span class="sxs-lookup"><span data-stu-id="c8e53-143">ACU</span></span>

<span data-ttu-id="c8e53-144">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="c8e53-144">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8e53-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8e53-145">Next steps</span></span>

<span data-ttu-id="c8e53-146">További tudnivalók hello másik Virtuálisgép-méretek elérhető:</span><span class="sxs-lookup"><span data-stu-id="c8e53-146">Learn more about hello different VM sizes that are available:</span></span>
- [<span data-ttu-id="c8e53-147">Általános célú</span><span class="sxs-lookup"><span data-stu-id="c8e53-147">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="c8e53-148">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-148">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="c8e53-149">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-149">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)
- [<span data-ttu-id="c8e53-150">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-150">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)
- [<span data-ttu-id="c8e53-151">GPU-optimalizált</span><span class="sxs-lookup"><span data-stu-id="c8e53-151">GPU optimized</span></span>](sizes-gpu.md)
- [<span data-ttu-id="c8e53-152">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="c8e53-152">High performance compute</span></span>](sizes-hpc.md)




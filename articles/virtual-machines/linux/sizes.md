---
title: "Linux virtuális gép méretének az Azure-ban |} Microsoft Docs"
description: "A Linux virtuális gépek Azure-ban elérhető különböző méretű sorolja fel."
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
ms.openlocfilehash: fe7a92901ae25aa99ef71f09c416e6c6ad30d39b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a><span data-ttu-id="cf5cf-103">Az Azure Linux virtuális gépek méretei</span><span class="sxs-lookup"><span data-stu-id="cf5cf-103">Sizes for Linux virtual machines in Azure</span></span>
<span data-ttu-id="cf5cf-104">Ez a cikk ismerteti az elérhető méretek és a beállítások a Linux-alkalmazások és munkafolyamatok futtatásához használhatja az Azure virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-104">This article describes the available sizes and options for the Azure virtual machines you can use to run your Linux apps and workloads.</span></span> <span data-ttu-id="cf5cf-105">Telepítési szempontok érdemes figyelembe vennie, amikor arra készül használni ezeket az erőforrásokat is biztosít.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-105">It also provides deployment considerations to be aware of when you're planning to use these resources.</span></span> <span data-ttu-id="cf5cf-106">Ez a cikk érhető el is [Windows virtuális gépek](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf5cf-106">This article is also available for [Windows virtual machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


| <span data-ttu-id="cf5cf-107">Típus</span><span class="sxs-lookup"><span data-stu-id="cf5cf-107">Type</span></span>                     | <span data-ttu-id="cf5cf-108">Méretek</span><span class="sxs-lookup"><span data-stu-id="cf5cf-108">Sizes</span></span>           |    <span data-ttu-id="cf5cf-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="cf5cf-109">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="cf5cf-110">Általános célú</span><span class="sxs-lookup"><span data-stu-id="cf5cf-110">General purpose</span></span>](sizes-general.md)          | <span data-ttu-id="cf5cf-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span><span class="sxs-lookup"><span data-stu-id="cf5cf-111">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7,</span></span>  | <span data-ttu-id="cf5cf-112">Kiegyensúlyozott processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-112">Balanced CPU-to-memory ratio.</span></span> <span data-ttu-id="cf5cf-113">Ideális választás tesztelési-fejlesztési feladatokhoz, kis és közepes méretű adatbázisokhoz, valamint kis és közepes adatforgalmú webkiszolgálókhoz.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-113">Ideal for testing and development, small to medium databases, and low to medium traffic web servers.</span></span> |
| [<span data-ttu-id="cf5cf-114">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-114">Compute optimized</span></span>](sizes-compute.md)        | <span data-ttu-id="cf5cf-115">FS, F</span><span class="sxs-lookup"><span data-stu-id="cf5cf-115">Fs, F</span></span>             | <span data-ttu-id="cf5cf-116">Magas processzor-memória arány.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-116">High CPU-to-memory ratio.</span></span> <span data-ttu-id="cf5cf-117">Közepes forgalom webkiszolgálók, hálózati berendezésekbe, fürdőbe folyamatok és alkalmazáskiszolgálók jó.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-117">Good for medium traffic web servers, network appliances, bath processes, and application servers.</span></span>        |
| [<span data-ttu-id="cf5cf-118">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-118">Memory optimized</span></span>](sizes-memory.md)         | <span data-ttu-id="cf5cf-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="cf5cf-119">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="cf5cf-120">Magas memória-CPU aránya.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-120">High memory-to-CPU ratio.</span></span> <span data-ttu-id="cf5cf-121">Ideális választás relációs adatbázis-kiszolgálókhoz, közepes és nagy gyorsítótárakhoz és memóriabeli elemzésekhez.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-121">Great for relational database servers, medium to large caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="cf5cf-122">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-122">Storage optimized</span></span>](sizes-storage.md)        | <span data-ttu-id="cf5cf-123">Ls</span><span class="sxs-lookup"><span data-stu-id="cf5cf-123">Ls</span></span>                | <span data-ttu-id="cf5cf-124">Magas lemez-adatátviteli és I/O-műveleti jellemzők.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-124">High disk throughput and IO.</span></span> <span data-ttu-id="cf5cf-125">Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-125">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="cf5cf-126">GPU</span><span class="sxs-lookup"><span data-stu-id="cf5cf-126">GPU</span></span>](sizes-gpu.md)            | <span data-ttu-id="cf5cf-127">PORTOK HV, NC</span><span class="sxs-lookup"><span data-stu-id="cf5cf-127">NV, NC</span></span>            | <span data-ttu-id="cf5cf-128">Speciális virtuális gépekre nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-128">Specialized virtual machines targeted for heavy graphic rendering and video editing.</span></span> <span data-ttu-id="cf5cf-129">Egy vagy több Feldolgozóegységekkel érhető el.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-129">Available with single or multiple GPUs.</span></span>       |
| [<span data-ttu-id="cf5cf-130">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="cf5cf-130">High performance compute</span></span>](sizes-hpc.md) | <span data-ttu-id="cf5cf-131">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="cf5cf-131">H, A8-11</span></span>          | <span data-ttu-id="cf5cf-132">A leggyorsabb és leghatékonyabb processzorral rendelkező virtuális gépeink, választható nagy átviteli sebességű (távoli közvetlen memória-hozzáférést lehetővé tevő) hálózati adapterrel.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-132">Our fastest and most powerful CPU virtual machines with optional high-throughput network interfaces (RDMA).</span></span> 

<br>

- <span data-ttu-id="cf5cf-133">A különböző méretű árazással kapcsolatos információkért lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span><span class="sxs-lookup"><span data-stu-id="cf5cf-133">For information about pricing of the various sizes, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).</span></span> 
- <span data-ttu-id="cf5cf-134">Az Azure-régiók Virtuálisgép-méretek rendelkezésre állását, lásd: [régiónként rendelkezésre álló termékek](https://azure.microsoft.com/regions/services/).</span><span class="sxs-lookup"><span data-stu-id="cf5cf-134">For availability of VM sizes in Azure regions, see [Products available by region](https://azure.microsoft.com/regions/services/).</span></span>
- <span data-ttu-id="cf5cf-135">Általános korlátozások az Azure virtuális gépeken futó, olvassa el [Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések](../../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="cf5cf-135">To see general limits on Azure VMs, see [Azure subscription and service limits, quotas, and constraints](../../azure-subscription-service-limits.md).</span></span>
- <span data-ttu-id="cf5cf-136">További tudnivalók [Azure számítási egység (ACU)](../windows/acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-136">Learn more about how [Azure compute units (ACU)](../windows/acu.md) can help you compare compute performance across Azure SKUs.</span></span>


## <a name="rest-api"></a><span data-ttu-id="cf5cf-137">REST API-n</span><span class="sxs-lookup"><span data-stu-id="cf5cf-137">Rest API</span></span>

<span data-ttu-id="cf5cf-138">A lekérdezés REST API-t használ a Virtuálisgép-méretek információkért tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="cf5cf-138">For information on using the REST API to query for VM sizes, see the following:</span></span>

- [<span data-ttu-id="cf5cf-139">Átméretezéséhez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="cf5cf-139">List available virtual machine sizes for resizing</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-for-resizing)
- [<span data-ttu-id="cf5cf-140">Az előfizetéshez elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="cf5cf-140">List available virtual machine sizes for a subscription</span></span>](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region)
- [<span data-ttu-id="cf5cf-141">A rendelkezésre állási csoportok elérhető virtuálisgép-méretek listázása</span><span class="sxs-lookup"><span data-stu-id="cf5cf-141">List available virtual machine sizes in an availability set</span></span>](
https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-list-sizes-availability-set)

## <a name="acu"></a><span data-ttu-id="cf5cf-142">ACU</span><span class="sxs-lookup"><span data-stu-id="cf5cf-142">ACU</span></span>

<span data-ttu-id="cf5cf-143">További tudnivalók [Azure számítási egység (ACU)](acu.md) segíthetnek a számítási teljesítmény összehasonlítása Azure termékváltozatok mentén.</span><span class="sxs-lookup"><span data-stu-id="cf5cf-143">Learn more about how [Azure compute units (ACU)](acu.md) can help you compare compute performance across Azure SKUs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf5cf-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf5cf-144">Next steps</span></span>

<span data-ttu-id="cf5cf-145">További információ a különböző Virtuálisgép-méretek érhetők el:</span><span class="sxs-lookup"><span data-stu-id="cf5cf-145">Learn more about the different VM sizes that are available:</span></span>
- [<span data-ttu-id="cf5cf-146">Általános célú</span><span class="sxs-lookup"><span data-stu-id="cf5cf-146">General purpose</span></span>](sizes-general.md)
- [<span data-ttu-id="cf5cf-147">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-147">Compute optimized</span></span>](sizes-compute.md)
- [<span data-ttu-id="cf5cf-148">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-148">Memory optimized</span></span>](sizes-memory.md)
- [<span data-ttu-id="cf5cf-149">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="cf5cf-149">Storage optimized</span></span>](sizes-storage.md)
- [<span data-ttu-id="cf5cf-150">GPU</span><span class="sxs-lookup"><span data-stu-id="cf5cf-150">GPU</span></span>](sizes-gpu.md)
- [<span data-ttu-id="cf5cf-151">Nagy teljesítményű számítás</span><span class="sxs-lookup"><span data-stu-id="cf5cf-151">High performance compute</span></span>](sizes-hpc.md)




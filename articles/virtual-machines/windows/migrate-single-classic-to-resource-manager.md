---
title: "A klasszikus virtuális gépek áttelepítése egy ARM felügyelt lemezes virtuális gép |} Microsoft Docs"
description: "Telepítse át egy Azure virtuális a klasszikus telepítési modellből felügyelt lemezeket a Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 82389834d85981c0ed71bdcc891fbfdbe1377654
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="manually-migrate-a-classic-vm-to-a-new-arm-managed-disk-vm-from-the-vhd"></a><span data-ttu-id="626de-103">Manuális áttelepítésével egy klasszikus virtuális Gépet egy új ARM kezelt lemez virtuális géphez a virtuális merevlemezről</span><span class="sxs-lookup"><span data-stu-id="626de-103">Manually migrate a Classic VM to a new ARM Managed Disk VM from the VHD</span></span> 


<span data-ttu-id="626de-104">Ez a szakasz segítséget nyújt a meglévő Azure virtuális gépek áttelepítéséhez a klasszikus telepítési modellből [kezelt lemezek](managed-disks-overview.md) a Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="626de-104">This section helps you to migrate your existing Azure VMs from the classic deployment model to [Managed Disks](managed-disks-overview.md) in the Resource Manager deployment model.</span></span>


## <a name="plan-for-the-migration-to-managed-disks"></a><span data-ttu-id="626de-105">Felügyelt lemezek az áttelepítés megtervezése</span><span class="sxs-lookup"><span data-stu-id="626de-105">Plan for the migration to Managed Disks</span></span>

<span data-ttu-id="626de-106">Ez a szakasz segítséget nyújt a legjobb döntést a virtuális gép és a lemez típusok.</span><span class="sxs-lookup"><span data-stu-id="626de-106">This section helps you to make the best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="626de-107">Hely</span><span class="sxs-lookup"><span data-stu-id="626de-107">Location</span></span>

<span data-ttu-id="626de-108">Jelölje ki a helyet, ahol Azure felügyelt lemezek érhetőek el.</span><span class="sxs-lookup"><span data-stu-id="626de-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="626de-109">Ha az áttelepítés prémium felügyelt lemezekre, is ellenőrizze, hogy prémium szintű storage elérhető a régióban, ahol szeretne áttelepíteni.</span><span class="sxs-lookup"><span data-stu-id="626de-109">If you are migrating to Premium Managed Disks, also ensure that Premium storage is available in the region where you are planning to migrate to.</span></span> <span data-ttu-id="626de-110">Lásd: [Azure Services byRegion](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="626de-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="626de-111">A virtuális gépek mérete</span><span class="sxs-lookup"><span data-stu-id="626de-111">VM sizes</span></span>

<span data-ttu-id="626de-112">Ha áttelepítés prémium szintű felügyelt lemez, hogy frissítse a virtuális gép méretét prémium szintű Storage képes a rendelkezésre álló terület a régióban, ahol a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="626de-112">If you are migrating to Premium Managed Disks, you have to update the size of the VM to Premium Storage capable size available in the region where VM is located.</span></span> <span data-ttu-id="626de-113">Tekintse át a Virtuálisgép-méretek, amelyek a prémium szintű Storage-kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="626de-113">Review the VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="626de-114">Az Azure virtuális gép mérete paramétereknek szereplő [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="626de-114">The Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="626de-115">Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok teljesítményétől.</span><span class="sxs-lookup"><span data-stu-id="626de-115">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="626de-116">Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális Gépet, a lemez forgalom alapjául.</span><span class="sxs-lookup"><span data-stu-id="626de-116">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="626de-117">Lemezméretek</span><span class="sxs-lookup"><span data-stu-id="626de-117">Disk sizes</span></span>

<span data-ttu-id="626de-118">**Prémium szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="626de-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="626de-119">Hét különböző típusú premium felügyelt lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok.</span><span class="sxs-lookup"><span data-stu-id="626de-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="626de-120">Vegye figyelembe a működés felső korlátjának kiválasztása a prémium szintű lemez a virtuális gép a kapacitás, a teljesítmény, méretezhetőség az alkalmazás igényeinek megfelelően, és maximális tölti be.</span><span class="sxs-lookup"><span data-stu-id="626de-120">Consider these limits when choosing the Premium disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="626de-121">Prémium szintű lemezek típusa</span><span class="sxs-lookup"><span data-stu-id="626de-121">Premium Disks Type</span></span>  | <span data-ttu-id="626de-122">P4</span><span class="sxs-lookup"><span data-stu-id="626de-122">P4</span></span>    | <span data-ttu-id="626de-123">P6</span><span class="sxs-lookup"><span data-stu-id="626de-123">P6</span></span>    | <span data-ttu-id="626de-124">P10</span><span class="sxs-lookup"><span data-stu-id="626de-124">P10</span></span>   | <span data-ttu-id="626de-125">P20</span><span class="sxs-lookup"><span data-stu-id="626de-125">P20</span></span>   | <span data-ttu-id="626de-126">P30</span><span class="sxs-lookup"><span data-stu-id="626de-126">P30</span></span>   | <span data-ttu-id="626de-127">P40</span><span class="sxs-lookup"><span data-stu-id="626de-127">P40</span></span>   | <span data-ttu-id="626de-128">P50</span><span class="sxs-lookup"><span data-stu-id="626de-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="626de-129">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="626de-129">Disk size</span></span>           | <span data-ttu-id="626de-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="626de-130">128 GB</span></span>| <span data-ttu-id="626de-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="626de-131">512 GB</span></span>| <span data-ttu-id="626de-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="626de-132">128 GB</span></span>| <span data-ttu-id="626de-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="626de-133">512 GB</span></span>            | <span data-ttu-id="626de-134">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="626de-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="626de-135">2048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="626de-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="626de-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="626de-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="626de-137">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="626de-137">IOPS per disk</span></span>       | <span data-ttu-id="626de-138">120</span><span class="sxs-lookup"><span data-stu-id="626de-138">120</span></span>   | <span data-ttu-id="626de-139">240</span><span class="sxs-lookup"><span data-stu-id="626de-139">240</span></span>   | <span data-ttu-id="626de-140">500</span><span class="sxs-lookup"><span data-stu-id="626de-140">500</span></span>   | <span data-ttu-id="626de-141">2300</span><span class="sxs-lookup"><span data-stu-id="626de-141">2300</span></span>              | <span data-ttu-id="626de-142">5000</span><span class="sxs-lookup"><span data-stu-id="626de-142">5000</span></span>              | <span data-ttu-id="626de-143">7500</span><span class="sxs-lookup"><span data-stu-id="626de-143">7500</span></span>              | <span data-ttu-id="626de-144">7500</span><span class="sxs-lookup"><span data-stu-id="626de-144">7500</span></span>              | 
| <span data-ttu-id="626de-145">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="626de-145">Throughput per disk</span></span> | <span data-ttu-id="626de-146">25 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-146">25 MB per second</span></span>  | <span data-ttu-id="626de-147">50 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-147">50 MB per second</span></span>  | <span data-ttu-id="626de-148">100 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-148">100 MB per second</span></span> | <span data-ttu-id="626de-149">150 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-149">150 MB per second</span></span> | <span data-ttu-id="626de-150">200 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-150">200 MB per second</span></span> | <span data-ttu-id="626de-151">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-151">250 MB per second</span></span> | <span data-ttu-id="626de-152">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-152">250 MB per second</span></span> | 

<span data-ttu-id="626de-153">**Standard szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="626de-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="626de-154">Standard szintű felügyelt lemez, amely a virtuális gép használható hét típusa van.</span><span class="sxs-lookup"><span data-stu-id="626de-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="626de-155">Azok a különböző kapacitással rendelkeznek, de azonos IOPS és átviteli sebességének korlátai.</span><span class="sxs-lookup"><span data-stu-id="626de-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="626de-156">Válassza ki a standard szintű felügyelt lemez az alkalmazás a kapacitásigények alapján.</span><span class="sxs-lookup"><span data-stu-id="626de-156">Choose the type of Standard Managed disks based on the capacity needs of your application.</span></span>

| <span data-ttu-id="626de-157">Standard lemez típusa</span><span class="sxs-lookup"><span data-stu-id="626de-157">Standard Disk Type</span></span>  | <span data-ttu-id="626de-158">S4</span><span class="sxs-lookup"><span data-stu-id="626de-158">S4</span></span>               | <span data-ttu-id="626de-159">S6</span><span class="sxs-lookup"><span data-stu-id="626de-159">S6</span></span>               | <span data-ttu-id="626de-160">S10</span><span class="sxs-lookup"><span data-stu-id="626de-160">S10</span></span>              | <span data-ttu-id="626de-161">S20</span><span class="sxs-lookup"><span data-stu-id="626de-161">S20</span></span>              | <span data-ttu-id="626de-162">S30</span><span class="sxs-lookup"><span data-stu-id="626de-162">S30</span></span>              | <span data-ttu-id="626de-163">S40</span><span class="sxs-lookup"><span data-stu-id="626de-163">S40</span></span>              | <span data-ttu-id="626de-164">S50</span><span class="sxs-lookup"><span data-stu-id="626de-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="626de-165">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="626de-165">Disk size</span></span>           | <span data-ttu-id="626de-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="626de-166">30 GB</span></span>            | <span data-ttu-id="626de-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="626de-167">64 GB</span></span>            | <span data-ttu-id="626de-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="626de-168">128 GB</span></span>           | <span data-ttu-id="626de-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="626de-169">512 GB</span></span>           | <span data-ttu-id="626de-170">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="626de-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="626de-171">2048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="626de-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="626de-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="626de-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="626de-173">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="626de-173">IOPS per disk</span></span>       | <span data-ttu-id="626de-174">500</span><span class="sxs-lookup"><span data-stu-id="626de-174">500</span></span>              | <span data-ttu-id="626de-175">500</span><span class="sxs-lookup"><span data-stu-id="626de-175">500</span></span>              | <span data-ttu-id="626de-176">500</span><span class="sxs-lookup"><span data-stu-id="626de-176">500</span></span>              | <span data-ttu-id="626de-177">500</span><span class="sxs-lookup"><span data-stu-id="626de-177">500</span></span>              | <span data-ttu-id="626de-178">500</span><span class="sxs-lookup"><span data-stu-id="626de-178">500</span></span>              | <span data-ttu-id="626de-179">500</span><span class="sxs-lookup"><span data-stu-id="626de-179">500</span></span>             | <span data-ttu-id="626de-180">500</span><span class="sxs-lookup"><span data-stu-id="626de-180">500</span></span>              | 
| <span data-ttu-id="626de-181">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="626de-181">Throughput per disk</span></span> | <span data-ttu-id="626de-182">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-182">60 MB per second</span></span> | <span data-ttu-id="626de-183">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-183">60 MB per second</span></span> | <span data-ttu-id="626de-184">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-184">60 MB per second</span></span> | <span data-ttu-id="626de-185">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-185">60 MB per second</span></span> | <span data-ttu-id="626de-186">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-186">60 MB per second</span></span> | <span data-ttu-id="626de-187">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-187">60 MB per second</span></span> | <span data-ttu-id="626de-188">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="626de-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="626de-189">Lemez gyorsítótárazási házirend</span><span class="sxs-lookup"><span data-stu-id="626de-189">Disk caching policy</span></span> 

<span data-ttu-id="626de-190">**Prémium szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="626de-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="626de-191">Alapértelmezés szerint a gyorsítótárazási házirend lemez van *csak olvasható* prémium adatok lemezein, és *írható-olvasható* az a prémium szintű operációsrendszer-lemez csatolva a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="626de-191">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="626de-192">A konfigurációs beállítás ajánlott az alkalmazás IOs rendszerhez az optimális teljesítmény eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="626de-192">This configuration setting is recommended to achieve the optimal performance for your application’s IOs.</span></span> <span data-ttu-id="626de-193">Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.</span><span class="sxs-lookup"><span data-stu-id="626de-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="626de-194">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="626de-194">Pricing</span></span>

<span data-ttu-id="626de-195">Tekintse át a [kezelt lemezek árképzési](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="626de-195">Review the [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="626de-196">Prémium szintű felügyelt lemez árképzési legyen, mint a nem felügyelt Premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="626de-196">Pricing of Premium Managed Disks is same as the Premium Unmanaged Disks.</span></span> <span data-ttu-id="626de-197">Azonban a standard szintű felügyelt lemez árképzési más nem felügyelt Standard lemezeknél.</span><span class="sxs-lookup"><span data-stu-id="626de-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="626de-198">Feladatlista</span><span class="sxs-lookup"><span data-stu-id="626de-198">Checklist</span></span>

1.  <span data-ttu-id="626de-199">Ha áttelepítés prémium szintű felügyelt lemez, feltétlenül érhető el a régió telepít át.</span><span class="sxs-lookup"><span data-stu-id="626de-199">If you are migrating to Premium Managed Disks, make sure it is available in the region you are migrating to.</span></span>

2.  <span data-ttu-id="626de-200">Döntse el, az új Virtuálisgép-sorozat fog használni.</span><span class="sxs-lookup"><span data-stu-id="626de-200">Decide the new VM series you will be using.</span></span> <span data-ttu-id="626de-201">A prémium szintű Storage képes kell, ha az áttelepítés prémium szintű felügyelt lemez.</span><span class="sxs-lookup"><span data-stu-id="626de-201">It should be a Premium Storage capable if you are migrating to Premium Managed Disks.</span></span>

3.  <span data-ttu-id="626de-202">Döntse el, a pontos Virtuálisgép-méretet fogja használni a régió telepít át a rendelkezésre álló.</span><span class="sxs-lookup"><span data-stu-id="626de-202">Decide the exact VM size you will use which are available in the region you are migrating to.</span></span> <span data-ttu-id="626de-203">Virtuálisgép-méretet kell lennie, elég nagy legyen rendelkezik adatlemezek számának támogatásához.</span><span class="sxs-lookup"><span data-stu-id="626de-203">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="626de-204">Például ha négy adatlemezek, a virtuális gép két vagy több maggal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="626de-204">For example, if you have four data disks, the VM must have two or more cores.</span></span> <span data-ttu-id="626de-205">Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="626de-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="626de-206">Az aktuális virtuális gép adatai lesz szüksége, beleértve a megfelelő VHD-blobok és lemezek listáját rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="626de-206">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="626de-207">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="626de-207">Prepare your application for downtime.</span></span> <span data-ttu-id="626de-208">Egy tiszta az áttelepítés végrehajtásához, akkor állítsa le a feldolgozás az aktuális rendszerben.</span><span class="sxs-lookup"><span data-stu-id="626de-208">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="626de-209">Csak ezután beszerezheti a konzisztens állapotú. Ez az új platformon is áttelepíthetők.</span><span class="sxs-lookup"><span data-stu-id="626de-209">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="626de-210">Állásidő időtartama áttelepítéséhez a lemezeken mennyiségétől függ.</span><span class="sxs-lookup"><span data-stu-id="626de-210">Downtime duration depends on the amount of data in the disks to migrate.</span></span>


## <a name="migrate-the-vm"></a><span data-ttu-id="626de-211">A virtuális gép áttelepítése</span><span class="sxs-lookup"><span data-stu-id="626de-211">Migrate the VM</span></span>

<span data-ttu-id="626de-212">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="626de-212">Prepare your application for downtime.</span></span> <span data-ttu-id="626de-213">Egy tiszta az áttelepítés végrehajtásához, akkor állítsa le a feldolgozás az aktuális rendszerben.</span><span class="sxs-lookup"><span data-stu-id="626de-213">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="626de-214">Csak ezután beszerezheti a konzisztens állapotú. Ez az új platformon is áttelepíthetők.</span><span class="sxs-lookup"><span data-stu-id="626de-214">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="626de-215">Állásidő időtartama áttelepítéséhez a lemezeken mennyiségétől függ.</span><span class="sxs-lookup"><span data-stu-id="626de-215">Downtime duration depends the amount of data in the disks to migrate.</span></span>


1.  <span data-ttu-id="626de-216">Első lépésként állítsa be a következő általános paramétereket:</span><span class="sxs-lookup"><span data-stu-id="626de-216">First, set the common parameters:</span></span>

    ```powershell
    $resourceGroupName = 'yourResourceGroupName'
    
    $location = 'your location' 
    
    $virtualNetworkName = 'yourExistingVirtualNetworkName'
    
    $virtualMachineName = 'yourVMName'
    
    $virtualMachineSize = 'Standard_DS3'
    
    $adminUserName = "youradminusername"
    
    $adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force
    
    $imageName = 'yourImageName'
    
    $osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'
    
    $dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'
    
    $dataDiskName = 'dataDisk1'
    ```

2.  <span data-ttu-id="626de-217">Hozzon létre egy felügyelt operációsrendszer-lemez, a klasszikus virtuális gépről a virtuális merevlemez használatával.</span><span class="sxs-lookup"><span data-stu-id="626de-217">Create a managed OS disk using the VHD from the classic VM.</span></span>

    <span data-ttu-id="626de-218">Győződjön meg arról, hogy megadta-e a teljes URI-azonosítója az operációs rendszer VHD-fájlt a $osVhdUri paraméter.</span><span class="sxs-lookup"><span data-stu-id="626de-218">Ensure that you have provided the complete URI of the OS VHD to the $osVhdUri parameter.</span></span> <span data-ttu-id="626de-219">Emellett adja meg **- AccountType** , **PremiumLRS** vagy **StandardLRS** alapú lemezek (prémium és Standard) típusú végzi az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="626de-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="626de-220">Az operációsrendszer-lemezképet csatlakoztatni az új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="626de-220">Attach the OS disk to the new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="626de-221">Felügyelt adatlemezt készíteni a VHD-fájlt, és adja hozzá az új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="626de-221">Create a managed data disk from the data VHD file and add it to the new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="626de-222">Az új virtuális gép létrehozása úgy, hogy nyilvános IP-, a virtuális hálózat és a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="626de-222">Create the new VM by setting public IP, Virtual Network and NIC.</span></span>

    ```powershell
    $publicIp = New-AzureRmPublicIpAddress -Name ($VirtualMachineName.ToLower()+'_ip') '
    -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
    
    $vnet = Get-AzureRmVirtualNetwork -Name $virtualNetworkName -ResourceGroupName $resourceGroupName
    
    $nic = New-AzureRmNetworkInterface -Name ($VirtualMachineName.ToLower()+'_nic') '
    -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id '
    -PublicIpAddressId $publicIp.Id
    
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nic.Id
    
    New-AzureRmVM -VM $VirtualMachine -ResourceGroupName $resourceGroupName -Location $location
    ```

> [!NOTE]
><span data-ttu-id="626de-223">További lépésekre lehet szükség az alkalmazás, amely támogatja az útmutató nem vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="626de-223">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="626de-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="626de-224">Next steps</span></span>

- <span data-ttu-id="626de-225">Csatlakozzon a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="626de-225">Connect to the virtual machine.</span></span> <span data-ttu-id="626de-226">Útmutatásért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="626de-226">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


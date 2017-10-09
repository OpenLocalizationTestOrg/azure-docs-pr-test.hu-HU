---
title: "a klasszikus virtuális gép tooan ARM kezelt lemez VM aaaMigrate |} Microsoft Docs"
description: "Egy Azure virtuális át hello klasszikus telepítési modell tooManaged lemezek hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: d8c4b9431f5dd8a071fcbc2ee36581a33f76ba62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manually-migrate-a-classic-vm-tooa-new-arm-managed-disk-vm-from-hello-vhd"></a><span data-ttu-id="7a311-103">Manuálisan telepítse át a klasszikus virtuális gép tooa új ARM kezelt lemez virtuális gép virtuális merevlemez hello</span><span class="sxs-lookup"><span data-stu-id="7a311-103">Manually migrate a Classic VM tooa new ARM Managed Disk VM from hello VHD</span></span> 


<span data-ttu-id="7a311-104">Ez a szakasz elősegíti, toomigrate a meglévő Azure virtuális gépek hello klasszikus telepítési modellből túl[kezelt lemezek](managed-disks-overview.md) hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="7a311-104">This section helps you toomigrate your existing Azure VMs from hello classic deployment model too[Managed Disks](managed-disks-overview.md) in hello Resource Manager deployment model.</span></span>


## <a name="plan-for-hello-migration-toomanaged-disks"></a><span data-ttu-id="7a311-105">Hello áttelepítés tervezése tooManaged lemezek</span><span class="sxs-lookup"><span data-stu-id="7a311-105">Plan for hello migration tooManaged Disks</span></span>

<span data-ttu-id="7a311-106">Ez a szakasz segítséget nyújt toomake hello legjobb döntést a virtuális gép és a lemez típusok.</span><span class="sxs-lookup"><span data-stu-id="7a311-106">This section helps you toomake hello best decision on VM and disk types.</span></span>


### <a name="location"></a><span data-ttu-id="7a311-107">Hely</span><span class="sxs-lookup"><span data-stu-id="7a311-107">Location</span></span>

<span data-ttu-id="7a311-108">Jelölje ki a helyet, ahol Azure felügyelt lemezek érhetőek el.</span><span class="sxs-lookup"><span data-stu-id="7a311-108">Pick a location where Azure Managed Disks are available.</span></span> <span data-ttu-id="7a311-109">TooPremium kezelt lemezek telepít át, ha is győződjön meg arról, hogy prémium szintű storage elérhető hello régióban, ha azt tervezi, hogy toomigrate.</span><span class="sxs-lookup"><span data-stu-id="7a311-109">If you are migrating tooPremium Managed Disks, also ensure that Premium storage is available in hello region where you are planning toomigrate to.</span></span> <span data-ttu-id="7a311-110">Lásd: [Azure Services byRegion](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7a311-110">See [Azure Services byRegion](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="7a311-111">A virtuális gépek mérete</span><span class="sxs-lookup"><span data-stu-id="7a311-111">VM sizes</span></span>

<span data-ttu-id="7a311-112">TooPremium kezelt lemezek telepít át, ha vannak tooupdate hello mérete hello VM tooPremium képes tárméret elérhető hello régió, ahol a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7a311-112">If you are migrating tooPremium Managed Disks, you have tooupdate hello size of hello VM tooPremium Storage capable size available in hello region where VM is located.</span></span> <span data-ttu-id="7a311-113">Tekintse át, amelyek képesek a prémium szintű Storage hello Virtuálisgép-méretek.</span><span class="sxs-lookup"><span data-stu-id="7a311-113">Review hello VM sizes that are Premium Storage capable.</span></span> <span data-ttu-id="7a311-114">hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="7a311-114">hello Azure VM size specifications are listed in [Sizes for virtual machines](sizes.md).</span></span>
<span data-ttu-id="7a311-115">Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől.</span><span class="sxs-lookup"><span data-stu-id="7a311-115">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="7a311-116">Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.</span><span class="sxs-lookup"><span data-stu-id="7a311-116">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

### <a name="disk-sizes"></a><span data-ttu-id="7a311-117">Lemezméretek</span><span class="sxs-lookup"><span data-stu-id="7a311-117">Disk sizes</span></span>

<span data-ttu-id="7a311-118">**Prémium szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="7a311-118">**Premium Managed Disks**</span></span>

<span data-ttu-id="7a311-119">Hét különböző típusú premium felügyelt lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok.</span><span class="sxs-lookup"><span data-stu-id="7a311-119">There are seven types of premium Managed disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="7a311-120">Vegye figyelembe a működés felső korlátjának hello prémium szintű lemez típusát a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális betölti kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="7a311-120">Consider these limits when choosing hello Premium disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="7a311-121">Prémium szintű lemezek típusa</span><span class="sxs-lookup"><span data-stu-id="7a311-121">Premium Disks Type</span></span>  | <span data-ttu-id="7a311-122">P4</span><span class="sxs-lookup"><span data-stu-id="7a311-122">P4</span></span>    | <span data-ttu-id="7a311-123">P6</span><span class="sxs-lookup"><span data-stu-id="7a311-123">P6</span></span>    | <span data-ttu-id="7a311-124">P10</span><span class="sxs-lookup"><span data-stu-id="7a311-124">P10</span></span>   | <span data-ttu-id="7a311-125">P20</span><span class="sxs-lookup"><span data-stu-id="7a311-125">P20</span></span>   | <span data-ttu-id="7a311-126">P30</span><span class="sxs-lookup"><span data-stu-id="7a311-126">P30</span></span>   | <span data-ttu-id="7a311-127">P40</span><span class="sxs-lookup"><span data-stu-id="7a311-127">P40</span></span>   | <span data-ttu-id="7a311-128">P50</span><span class="sxs-lookup"><span data-stu-id="7a311-128">P50</span></span>   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| <span data-ttu-id="7a311-129">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="7a311-129">Disk size</span></span>           | <span data-ttu-id="7a311-130">128 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-130">128 GB</span></span>| <span data-ttu-id="7a311-131">512 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-131">512 GB</span></span>| <span data-ttu-id="7a311-132">128 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-132">128 GB</span></span>| <span data-ttu-id="7a311-133">512 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-133">512 GB</span></span>            | <span data-ttu-id="7a311-134">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-134">1024 GB (1 TB)</span></span>    | <span data-ttu-id="7a311-135">2048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-135">2048 GB (2 TB)</span></span>    | <span data-ttu-id="7a311-136">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-136">4095 GB (4 TB)</span></span>    | 
| <span data-ttu-id="7a311-137">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="7a311-137">IOPS per disk</span></span>       | <span data-ttu-id="7a311-138">120</span><span class="sxs-lookup"><span data-stu-id="7a311-138">120</span></span>   | <span data-ttu-id="7a311-139">240</span><span class="sxs-lookup"><span data-stu-id="7a311-139">240</span></span>   | <span data-ttu-id="7a311-140">500</span><span class="sxs-lookup"><span data-stu-id="7a311-140">500</span></span>   | <span data-ttu-id="7a311-141">2300</span><span class="sxs-lookup"><span data-stu-id="7a311-141">2300</span></span>              | <span data-ttu-id="7a311-142">5000</span><span class="sxs-lookup"><span data-stu-id="7a311-142">5000</span></span>              | <span data-ttu-id="7a311-143">7500</span><span class="sxs-lookup"><span data-stu-id="7a311-143">7500</span></span>              | <span data-ttu-id="7a311-144">7500</span><span class="sxs-lookup"><span data-stu-id="7a311-144">7500</span></span>              | 
| <span data-ttu-id="7a311-145">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="7a311-145">Throughput per disk</span></span> | <span data-ttu-id="7a311-146">25 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-146">25 MB per second</span></span>  | <span data-ttu-id="7a311-147">50 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-147">50 MB per second</span></span>  | <span data-ttu-id="7a311-148">100 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-148">100 MB per second</span></span> | <span data-ttu-id="7a311-149">150 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-149">150 MB per second</span></span> | <span data-ttu-id="7a311-150">200 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-150">200 MB per second</span></span> | <span data-ttu-id="7a311-151">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-151">250 MB per second</span></span> | <span data-ttu-id="7a311-152">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-152">250 MB per second</span></span> | 

<span data-ttu-id="7a311-153">**Standard szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="7a311-153">**Standard Managed Disks**</span></span>

<span data-ttu-id="7a311-154">Standard szintű felügyelt lemez, amely a virtuális gép használható hét típusa van.</span><span class="sxs-lookup"><span data-stu-id="7a311-154">There are seven types of Standard Managed disks that can be used with your VM.</span></span> <span data-ttu-id="7a311-155">Azok a különböző kapacitással rendelkeznek, de azonos IOPS és átviteli sebességének korlátai.</span><span class="sxs-lookup"><span data-stu-id="7a311-155">Each of them have different capacity but have same IOPS and throughput limits.</span></span> <span data-ttu-id="7a311-156">Válassza ki az alkalmazás hello kapacitásigények alapján standard szintű felügyelt lemez hello típusú.</span><span class="sxs-lookup"><span data-stu-id="7a311-156">Choose hello type of Standard Managed disks based on hello capacity needs of your application.</span></span>

| <span data-ttu-id="7a311-157">Standard lemez típusa</span><span class="sxs-lookup"><span data-stu-id="7a311-157">Standard Disk Type</span></span>  | <span data-ttu-id="7a311-158">S4</span><span class="sxs-lookup"><span data-stu-id="7a311-158">S4</span></span>               | <span data-ttu-id="7a311-159">S6</span><span class="sxs-lookup"><span data-stu-id="7a311-159">S6</span></span>               | <span data-ttu-id="7a311-160">S10</span><span class="sxs-lookup"><span data-stu-id="7a311-160">S10</span></span>              | <span data-ttu-id="7a311-161">S20</span><span class="sxs-lookup"><span data-stu-id="7a311-161">S20</span></span>              | <span data-ttu-id="7a311-162">S30</span><span class="sxs-lookup"><span data-stu-id="7a311-162">S30</span></span>              | <span data-ttu-id="7a311-163">S40</span><span class="sxs-lookup"><span data-stu-id="7a311-163">S40</span></span>              | <span data-ttu-id="7a311-164">S50</span><span class="sxs-lookup"><span data-stu-id="7a311-164">S50</span></span>              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| <span data-ttu-id="7a311-165">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="7a311-165">Disk size</span></span>           | <span data-ttu-id="7a311-166">30 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-166">30 GB</span></span>            | <span data-ttu-id="7a311-167">64 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-167">64 GB</span></span>            | <span data-ttu-id="7a311-168">128 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-168">128 GB</span></span>           | <span data-ttu-id="7a311-169">512 GB</span><span class="sxs-lookup"><span data-stu-id="7a311-169">512 GB</span></span>           | <span data-ttu-id="7a311-170">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-170">1024 GB (1 TB)</span></span>   | <span data-ttu-id="7a311-171">2048 GB (2TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-171">2048 GB (2TB)</span></span>    | <span data-ttu-id="7a311-172">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="7a311-172">4095 GB (4 TB)</span></span>   | 
| <span data-ttu-id="7a311-173">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="7a311-173">IOPS per disk</span></span>       | <span data-ttu-id="7a311-174">500</span><span class="sxs-lookup"><span data-stu-id="7a311-174">500</span></span>              | <span data-ttu-id="7a311-175">500</span><span class="sxs-lookup"><span data-stu-id="7a311-175">500</span></span>              | <span data-ttu-id="7a311-176">500</span><span class="sxs-lookup"><span data-stu-id="7a311-176">500</span></span>              | <span data-ttu-id="7a311-177">500</span><span class="sxs-lookup"><span data-stu-id="7a311-177">500</span></span>              | <span data-ttu-id="7a311-178">500</span><span class="sxs-lookup"><span data-stu-id="7a311-178">500</span></span>              | <span data-ttu-id="7a311-179">500</span><span class="sxs-lookup"><span data-stu-id="7a311-179">500</span></span>             | <span data-ttu-id="7a311-180">500</span><span class="sxs-lookup"><span data-stu-id="7a311-180">500</span></span>              | 
| <span data-ttu-id="7a311-181">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="7a311-181">Throughput per disk</span></span> | <span data-ttu-id="7a311-182">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-182">60 MB per second</span></span> | <span data-ttu-id="7a311-183">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-183">60 MB per second</span></span> | <span data-ttu-id="7a311-184">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-184">60 MB per second</span></span> | <span data-ttu-id="7a311-185">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-185">60 MB per second</span></span> | <span data-ttu-id="7a311-186">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-186">60 MB per second</span></span> | <span data-ttu-id="7a311-187">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-187">60 MB per second</span></span> | <span data-ttu-id="7a311-188">60 MB / s</span><span class="sxs-lookup"><span data-stu-id="7a311-188">60 MB per second</span></span> | 


### <a name="disk-caching-policy"></a><span data-ttu-id="7a311-189">Lemez gyorsítótárazási házirend</span><span class="sxs-lookup"><span data-stu-id="7a311-189">Disk caching policy</span></span> 

<span data-ttu-id="7a311-190">**Prémium szintű felügyelt lemez**</span><span class="sxs-lookup"><span data-stu-id="7a311-190">**Premium Managed Disks**</span></span>

<span data-ttu-id="7a311-191">Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="7a311-191">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="7a311-192">A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez.</span><span class="sxs-lookup"><span data-stu-id="7a311-192">This configuration setting is recommended tooachieve hello optimal performance for your application’s IOs.</span></span> <span data-ttu-id="7a311-193">Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.</span><span class="sxs-lookup"><span data-stu-id="7a311-193">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span>

### <a name="pricing"></a><span data-ttu-id="7a311-194">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="7a311-194">Pricing</span></span>

<span data-ttu-id="7a311-195">Felülvizsgálati hello [kezelt lemezek árképzési](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="7a311-195">Review hello [pricing for Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/).</span></span> <span data-ttu-id="7a311-196">Prémium szintű felügyelt lemez árképzési legyen, mint hello nem felügyelt Premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="7a311-196">Pricing of Premium Managed Disks is same as hello Premium Unmanaged Disks.</span></span> <span data-ttu-id="7a311-197">Azonban a standard szintű felügyelt lemez árképzési más nem felügyelt Standard lemezeknél.</span><span class="sxs-lookup"><span data-stu-id="7a311-197">But pricing for Standard Managed Disks is different than Standard Unmanaged Disks.</span></span>


## <a name="checklist"></a><span data-ttu-id="7a311-198">Feladatlista</span><span class="sxs-lookup"><span data-stu-id="7a311-198">Checklist</span></span>

1.  <span data-ttu-id="7a311-199">Ha tooPremium kezelt lemezek telepít át, győződjön meg arról érhető el hello régió telepít át.</span><span class="sxs-lookup"><span data-stu-id="7a311-199">If you are migrating tooPremium Managed Disks, make sure it is available in hello region you are migrating to.</span></span>

2.  <span data-ttu-id="7a311-200">Döntse el, hello új Virtuálisgép-sorozat fog használni.</span><span class="sxs-lookup"><span data-stu-id="7a311-200">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="7a311-201">A prémium szintű Storage képes kell, ha telepít át tooPremium kezelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="7a311-201">It should be a Premium Storage capable if you are migrating tooPremium Managed Disks.</span></span>

3.  <span data-ttu-id="7a311-202">Döntse el, hello pontos Virtuálisgép-méretet használandó hello régió telepít át a rendelkezésre álló.</span><span class="sxs-lookup"><span data-stu-id="7a311-202">Decide hello exact VM size you will use which are available in hello region you are migrating to.</span></span> <span data-ttu-id="7a311-203">Virtuálisgép-méretet kell toobe elég nagy toosupport hello rendelkezik adatlemezek száma.</span><span class="sxs-lookup"><span data-stu-id="7a311-203">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="7a311-204">Például ha négy adatlemezek, hello virtuális gép két vagy több maggal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7a311-204">For example, if you have four data disks, hello VM must have two or more cores.</span></span> <span data-ttu-id="7a311-205">Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="7a311-205">Also, consider processing power, memory and network bandwidth needs.</span></span>

4.  <span data-ttu-id="7a311-206">Hello aktuális virtuális gép adatai lesz szüksége, beleértve a lemezek és a megfelelő VHD-blobok hello listája rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7a311-206">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="7a311-207">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="7a311-207">Prepare your application for downtime.</span></span> <span data-ttu-id="7a311-208">egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer.</span><span class="sxs-lookup"><span data-stu-id="7a311-208">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="7a311-209">Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon.</span><span class="sxs-lookup"><span data-stu-id="7a311-209">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="7a311-210">Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.</span><span class="sxs-lookup"><span data-stu-id="7a311-210">Downtime duration depends on hello amount of data in hello disks toomigrate.</span></span>


## <a name="migrate-hello-vm"></a><span data-ttu-id="7a311-211">Telepítse át a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="7a311-211">Migrate hello VM</span></span>

<span data-ttu-id="7a311-212">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="7a311-212">Prepare your application for downtime.</span></span> <span data-ttu-id="7a311-213">egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer.</span><span class="sxs-lookup"><span data-stu-id="7a311-213">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="7a311-214">Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon.</span><span class="sxs-lookup"><span data-stu-id="7a311-214">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="7a311-215">Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.</span><span class="sxs-lookup"><span data-stu-id="7a311-215">Downtime duration depends hello amount of data in hello disks toomigrate.</span></span>


1.  <span data-ttu-id="7a311-216">Első lépésként állítsa be az általános paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="7a311-216">First, set hello common parameters:</span></span>

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

2.  <span data-ttu-id="7a311-217">Hozzon létre egy felügyelt operációsrendszer-lemez használatával hello VHD hello a klasszikus virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7a311-217">Create a managed OS disk using hello VHD from hello classic VM.</span></span>

    <span data-ttu-id="7a311-218">Győződjön meg arról, hogy rendelkezik a megadott hello végezze el az operációs rendszer virtuális merevlemez toohello $osVhdUri paraméter hello URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="7a311-218">Ensure that you have provided hello complete URI of hello OS VHD toohello $osVhdUri parameter.</span></span> <span data-ttu-id="7a311-219">Emellett adja meg **- AccountType** , **PremiumLRS** vagy **StandardLRS** alapú lemezek (prémium és Standard) típusú végzi az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="7a311-219">Also, enter **-AccountType** as **PremiumLRS** or **StandardLRS** based on type of disks (Premium or Standard) you are migrating to.</span></span>

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $osVhdUri) '
    -ResourceGroupName $resourceGroupName
    ```

3.  <span data-ttu-id="7a311-220">Az operációs rendszer hello lemez toohello csatolása új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7a311-220">Attach hello OS disk toohello new VM.</span></span>

    ```powershell
    $VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -ManagedDiskId $osDisk.Id '
    -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

4.  <span data-ttu-id="7a311-221">Felügyelt adatlemezt készíteni hello VHD-fájlt, és adja hozzá toohello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7a311-221">Create a managed data disk from hello data VHD file and add it toohello new VM.</span></span>

    ```powershell
    $dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig '
    -AccountType PremiumLRS -Location $location -CreationDataCreateOption Import '
    -SourceUri $dataVhdUri ) -ResourceGroupName $resourceGroupName
    
    $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName '
    -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
    ```

5.  <span data-ttu-id="7a311-222">Hozzon létre új virtuális gép hello úgy, hogy nyilvános IP-, a virtuális hálózat és a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="7a311-222">Create hello new VM by setting public IP, Virtual Network and NIC.</span></span>

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
><span data-ttu-id="7a311-223">Előfordulhat, hogy további lépéseket szükséges toosupport az alkalmazás, amely nem elegendő az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="7a311-223">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="7a311-224">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a311-224">Next steps</span></span>

- <span data-ttu-id="7a311-225">Csatlakoztassa a toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7a311-225">Connect toohello virtual machine.</span></span> <span data-ttu-id="7a311-226">Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a311-226">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


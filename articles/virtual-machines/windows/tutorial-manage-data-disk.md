---
title: az Azure PowerShell hello lemezek aaaManage Azure |} Microsoft Docs
description: "Útmutató – az Azure PowerShell hello Azure lemezek kezelése"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2f61ad18bc94bab527d7ae593da603c6073adc89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-powershell"></a><span data-ttu-id="d7e14-103">Azure-lemezeket a PowerShell-lel kezelése</span><span class="sxs-lookup"><span data-stu-id="d7e14-103">Manage Azure disks with PowerShell</span></span>

<span data-ttu-id="d7e14-104">Az Azure virtuális gépek használata a lemezek toostore hello virtuális gépek operációs rendszer, alkalmazások és adatok.</span><span class="sxs-lookup"><span data-stu-id="d7e14-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="d7e14-105">Virtuális gép létrehozása esetén fontos toochoose a lemez méretét és a konfigurációs megfelelő toohello várható terhelést.</span><span class="sxs-lookup"><span data-stu-id="d7e14-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="d7e14-106">Ez az oktatóanyag ismerteti, központi telepítésére és felügyeletére a virtuális gépek lemezei.</span><span class="sxs-lookup"><span data-stu-id="d7e14-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="d7e14-107">További tudnivalók:</span><span class="sxs-lookup"><span data-stu-id="d7e14-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7e14-108">Az operációs rendszer és a ideiglenes lemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="d7e14-109">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-109">Data disks</span></span>
> * <span data-ttu-id="d7e14-110">Standard és Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="d7e14-111">Lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="d7e14-111">Disk performance</span></span>
> * <span data-ttu-id="d7e14-112">Csatlakoztatása és adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="d7e14-112">Attaching and preparing data disks</span></span>

<span data-ttu-id="d7e14-113">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7e14-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="d7e14-114">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="d7e14-114">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="d7e14-115">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="d7e14-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

## <a name="default-azure-disks"></a><span data-ttu-id="d7e14-116">Azure-lemezeket alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="d7e14-116">Default Azure disks</span></span>

<span data-ttu-id="d7e14-117">Egy Azure virtuális gép létrehozásakor a két lemez automatikusan csatolt toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d7e14-117">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="d7e14-118">**Operációsrendszer-lemez** - operációsrendszer-lemezek is kell méretezni too1 terabájt fel, és a gazdagépek hello virtuális gépek operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="d7e14-118">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span>  <span data-ttu-id="d7e14-119">hello operációsrendszer-lemez van rendelve egy meghajtóbetűjelét *c:* alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="d7e14-119">hello OS disk is assigned a drive letter of *c:* by default.</span></span> <span data-ttu-id="d7e14-120">hello lemez gyorsítótárazás hello operációsrendszer-lemez konfigurációját úgy optimalizálták, hogy az operációs rendszer teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d7e14-120">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="d7e14-121">az operációs rendszer hello lemez **nem kell** alkalmazások vagy az adatok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="d7e14-121">hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="d7e14-122">Az alkalmazások és adatok használja az adatlemezt, amely a cikk későbbi részében részleteit.</span><span class="sxs-lookup"><span data-stu-id="d7e14-122">For applications and data, use a data disk, which is detailed later in this article.</span></span>

<span data-ttu-id="d7e14-123">**Ideiglenes lemez** -ideiglenes lemezeken található SSD-meghajtó használatát hello azonos Azure-gazdagép, hello VM.</span><span class="sxs-lookup"><span data-stu-id="d7e14-123">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="d7e14-124">Ideiglenes lemezek magas performant és műveletek, például a ideiglenes adatfeldolgozási használható.</span><span class="sxs-lookup"><span data-stu-id="d7e14-124">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="d7e14-125">Azonban ha hello VM áthelyezett tooa új gazdagép, eltávolítják egy ideiglenes lemezen tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="d7e14-125">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="d7e14-126">Virtuálisgép-méretet hello hello hello ideiglenes lemez mérete határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d7e14-126">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="d7e14-127">Ideiglenes lemezek vannak hozzárendelve a meghajtóbetűjel *d:* alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="d7e14-127">Temporary disks are assigned a drive letter of *d:* by default.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="d7e14-128">Ideiglenes mérete</span><span class="sxs-lookup"><span data-stu-id="d7e14-128">Temporary disk sizes</span></span>

| <span data-ttu-id="d7e14-129">Típus</span><span class="sxs-lookup"><span data-stu-id="d7e14-129">Type</span></span> | <span data-ttu-id="d7e14-130">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="d7e14-130">VM Size</span></span> | <span data-ttu-id="d7e14-131">Maximális ideiglenes lemez (GB)</span><span class="sxs-lookup"><span data-stu-id="d7e14-131">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="d7e14-132">Általános célú</span><span class="sxs-lookup"><span data-stu-id="d7e14-132">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="d7e14-133">A-D sorozat része</span><span class="sxs-lookup"><span data-stu-id="d7e14-133">A and D series</span></span> | <span data-ttu-id="d7e14-134">800</span><span class="sxs-lookup"><span data-stu-id="d7e14-134">800</span></span> |
| [<span data-ttu-id="d7e14-135">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-135">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="d7e14-136">F sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-136">F series</span></span> | <span data-ttu-id="d7e14-137">800</span><span class="sxs-lookup"><span data-stu-id="d7e14-137">800</span></span> |
| [<span data-ttu-id="d7e14-138">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-138">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="d7e14-139">D és G sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-139">D and G series</span></span> | <span data-ttu-id="d7e14-140">6144</span><span class="sxs-lookup"><span data-stu-id="d7e14-140">6144</span></span> |
| [<span data-ttu-id="d7e14-141">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-141">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="d7e14-142">L sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-142">L series</span></span> | <span data-ttu-id="d7e14-143">5630</span><span class="sxs-lookup"><span data-stu-id="d7e14-143">5630</span></span> |
| [<span data-ttu-id="d7e14-144">GPU</span><span class="sxs-lookup"><span data-stu-id="d7e14-144">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="d7e14-145">N sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-145">N series</span></span> | <span data-ttu-id="d7e14-146">1440</span><span class="sxs-lookup"><span data-stu-id="d7e14-146">1440</span></span> |
| [<span data-ttu-id="d7e14-147">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="d7e14-147">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="d7e14-148">A és H sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-148">A and H series</span></span> | <span data-ttu-id="d7e14-149">2000</span><span class="sxs-lookup"><span data-stu-id="d7e14-149">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="d7e14-150">Az Azure data lemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-150">Azure data disks</span></span>

<span data-ttu-id="d7e14-151">További adatlemezt felvehető alkalmazások telepítése és adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="d7e14-151">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="d7e14-152">Az adatlemezek minden helyzetben, ahol van szükség az adatok tartósságát és rugalmas tárolási kell használni.</span><span class="sxs-lookup"><span data-stu-id="d7e14-152">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="d7e14-153">Minden adat lemez 1 terabájtnál maximális kapacitása nem.</span><span class="sxs-lookup"><span data-stu-id="d7e14-153">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="d7e14-154">hello virtuális gép hello mérete határozza meg, hány adatlemezek csatolt tooa virtuális gép is lehet.</span><span class="sxs-lookup"><span data-stu-id="d7e14-154">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="d7e14-155">A minden egyes virtuális core két adatlemezek kapcsolható ki.</span><span class="sxs-lookup"><span data-stu-id="d7e14-155">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="d7e14-156">Maximális adatlemezek virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="d7e14-156">Max data disks per VM</span></span>

| <span data-ttu-id="d7e14-157">Típus</span><span class="sxs-lookup"><span data-stu-id="d7e14-157">Type</span></span> | <span data-ttu-id="d7e14-158">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="d7e14-158">VM Size</span></span> | <span data-ttu-id="d7e14-159">Maximális adatlemezek virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="d7e14-159">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="d7e14-160">Általános célú</span><span class="sxs-lookup"><span data-stu-id="d7e14-160">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="d7e14-161">A-D sorozat része</span><span class="sxs-lookup"><span data-stu-id="d7e14-161">A and D series</span></span> | <span data-ttu-id="d7e14-162">32</span><span class="sxs-lookup"><span data-stu-id="d7e14-162">32</span></span> |
| [<span data-ttu-id="d7e14-163">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-163">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="d7e14-164">F sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-164">F series</span></span> | <span data-ttu-id="d7e14-165">32</span><span class="sxs-lookup"><span data-stu-id="d7e14-165">32</span></span> |
| [<span data-ttu-id="d7e14-166">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-166">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="d7e14-167">D és G sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-167">D and G series</span></span> | <span data-ttu-id="d7e14-168">64</span><span class="sxs-lookup"><span data-stu-id="d7e14-168">64</span></span> |
| [<span data-ttu-id="d7e14-169">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="d7e14-169">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="d7e14-170">L sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-170">L series</span></span> | <span data-ttu-id="d7e14-171">64</span><span class="sxs-lookup"><span data-stu-id="d7e14-171">64</span></span> |
| [<span data-ttu-id="d7e14-172">GPU</span><span class="sxs-lookup"><span data-stu-id="d7e14-172">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="d7e14-173">N sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-173">N series</span></span> | <span data-ttu-id="d7e14-174">48</span><span class="sxs-lookup"><span data-stu-id="d7e14-174">48</span></span> |
| [<span data-ttu-id="d7e14-175">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="d7e14-175">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="d7e14-176">A és H sorozat</span><span class="sxs-lookup"><span data-stu-id="d7e14-176">A and H series</span></span> | <span data-ttu-id="d7e14-177">32</span><span class="sxs-lookup"><span data-stu-id="d7e14-177">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="d7e14-178">Virtuális gép lemeztípusok</span><span class="sxs-lookup"><span data-stu-id="d7e14-178">VM disk types</span></span>

<span data-ttu-id="d7e14-179">Azure két lemez biztosít.</span><span class="sxs-lookup"><span data-stu-id="d7e14-179">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="d7e14-180">Standard méretű lemez</span><span class="sxs-lookup"><span data-stu-id="d7e14-180">Standard disk</span></span>

<span data-ttu-id="d7e14-181">A merevlemez-meghajtókra épülő Standard Storage költséghatékony tárolási megoldás, amely emellett jó teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="d7e14-181">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="d7e14-182">Standard lemezek ideális, ha a költséghatékony fejlesztői és munkaterhelés teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d7e14-182">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="d7e14-183">Prémium szintű lemez</span><span class="sxs-lookup"><span data-stu-id="d7e14-183">Premium disk</span></span>

<span data-ttu-id="d7e14-184">Premium lemezek SSD-alapú nagy teljesítményű, alacsony késésű lemez üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="d7e14-184">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="d7e14-185">Tökéletes éles munkaterhelést futtató virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="d7e14-185">Perfect for VMs running production workload.</span></span> <span data-ttu-id="d7e14-186">Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS sorozatnak és FS sorozatú virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="d7e14-186">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="d7e14-187">Prémium szintű lemezekhez három típusa (P10, P20, P30) helyen, és hello lemez mérete hello hello lemez típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d7e14-187">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="d7e14-188">Ha választja, a lemez mérete hello értéke függvény toohello következő típusa.</span><span class="sxs-lookup"><span data-stu-id="d7e14-188">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="d7e14-189">Például ha hello mérete nem éri el 128 GB-os hello lemeztípus lesz P10, 129 és 512 P20 és több mint 512 P30 között.</span><span class="sxs-lookup"><span data-stu-id="d7e14-189">For example, if hello size is below 128 GB hello disk type will be P10, between 129 and 512 P20, and over 512 P30.</span></span> 

### <a name="premium-disk-performance"></a><span data-ttu-id="d7e14-190">Prémium szintű lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="d7e14-190">Premium disk performance</span></span>

|<span data-ttu-id="d7e14-191">Prémium szintű storage lemeztípus</span><span class="sxs-lookup"><span data-stu-id="d7e14-191">Premium storage disk type</span></span> | <span data-ttu-id="d7e14-192">P10</span><span class="sxs-lookup"><span data-stu-id="d7e14-192">P10</span></span> | <span data-ttu-id="d7e14-193">P20</span><span class="sxs-lookup"><span data-stu-id="d7e14-193">P20</span></span> | <span data-ttu-id="d7e14-194">P30</span><span class="sxs-lookup"><span data-stu-id="d7e14-194">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d7e14-195">A lemezméret (kerek mentése)</span><span class="sxs-lookup"><span data-stu-id="d7e14-195">Disk size (round up)</span></span> | <span data-ttu-id="d7e14-196">128 GB</span><span class="sxs-lookup"><span data-stu-id="d7e14-196">128 GB</span></span> | <span data-ttu-id="d7e14-197">512 GB</span><span class="sxs-lookup"><span data-stu-id="d7e14-197">512 GB</span></span> | <span data-ttu-id="d7e14-198">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="d7e14-198">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="d7e14-199">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="d7e14-199">IOPS per disk</span></span> | <span data-ttu-id="d7e14-200">500</span><span class="sxs-lookup"><span data-stu-id="d7e14-200">500</span></span> | <span data-ttu-id="d7e14-201">2,300</span><span class="sxs-lookup"><span data-stu-id="d7e14-201">2,300</span></span> | <span data-ttu-id="d7e14-202">5,000</span><span class="sxs-lookup"><span data-stu-id="d7e14-202">5,000</span></span> |
<span data-ttu-id="d7e14-203">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="d7e14-203">Throughput per disk</span></span> | <span data-ttu-id="d7e14-204">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="d7e14-204">100 MB/s</span></span> | <span data-ttu-id="d7e14-205">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="d7e14-205">150 MB/s</span></span> | <span data-ttu-id="d7e14-206">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="d7e14-206">200 MB/s</span></span> |

<span data-ttu-id="d7e14-207">Táblázat felett hello azonosítja a maximális iops-érték lemezenként, magasabb szintű teljesítmény legyen elérhető több adatlemezek szétosztott.</span><span class="sxs-lookup"><span data-stu-id="d7e14-207">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="d7e14-208">Például lehet lemezeket 64 adatok csatolt tooStandard_GS5 virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d7e14-208">For instance, 64 data disks can be attached tooStandard_GS5 VM.</span></span> <span data-ttu-id="d7e14-209">Ha ezeknek a lemezeknek mindegyikének, egy P30 mérete, 80000 IOPS maximális érhető el.</span><span class="sxs-lookup"><span data-stu-id="d7e14-209">If each of these disks are sized as a P30, a maximum of 80,000 IOPS can be achieved.</span></span> <span data-ttu-id="d7e14-210">A virtuális gép maximális iops-értéket részletes információkért lásd: [Linux Virtuálisgép-méretek](./sizes.md).</span><span class="sxs-lookup"><span data-stu-id="d7e14-210">For detailed information on max IOPS per VM, see [Linux VM sizes](./sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="d7e14-211">Hozzon létre, és csatlakoztassa a lemezeket</span><span class="sxs-lookup"><span data-stu-id="d7e14-211">Create and attach disks</span></span>

<span data-ttu-id="d7e14-212">Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d7e14-212">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="d7e14-213">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="d7e14-213">If needed, this [script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) can create one for you.</span></span> <span data-ttu-id="d7e14-214">Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7e14-214">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

<span data-ttu-id="d7e14-215">Hozzon létre a kezdeti konfiguráció hello [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span><span class="sxs-lookup"><span data-stu-id="d7e14-215">Create hello initial configuration with [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig).</span></span> <span data-ttu-id="d7e14-216">a következő példa hello konfigurálja 128 GB-nál a lemezt.</span><span class="sxs-lookup"><span data-stu-id="d7e14-216">hello following example configures a disk that is 128 gigabytes in size.</span></span>

```powershell
$diskConfig = New-AzureRmDiskConfig -Location EastUS -CreateOption Empty -DiskSizeGB 128
```

<span data-ttu-id="d7e14-217">Hozzon létre hello adatlemez hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7e14-217">Create hello data disk with hello [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) command.</span></span>

```powershell
$dataDisk = New-AzureRmDisk -ResourceGroupName myResourceGroup -DiskName myDataDisk -Disk $diskConfig
```

<span data-ttu-id="d7e14-218">Get hello virtuális gép, amelyet az tooadd hello adatok lemez toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7e14-218">Get hello virtual machine that you want tooadd hello data disk toowith hello [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) command.</span></span>

```powershell
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

<span data-ttu-id="d7e14-219">Hozzáadás hello adatok lemez toohello virtuálisgép-konfigurációnak hello [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7e14-219">Add hello data disk toohello virtual machine configuration with hello [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
$vm = Add-AzureRmVMDataDisk -VM $vm -Name myDataDisk -CreateOption Attach -ManagedDiskId $dataDisk.Id -Lun 1
```

<span data-ttu-id="d7e14-220">Frissítse hello virtuális gépet hello [frissítés-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d7e14-220">Update hello virtual machine with hello [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk) command.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm
```

## <a name="prepare-data-disks"></a><span data-ttu-id="d7e14-221">Az adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="d7e14-221">Prepare data disks</span></span>

<span data-ttu-id="d7e14-222">A lemez virtuális géphez csatolt toohello követően hello operációs rendszer kell konfigurált toobe toouse hello lemez.</span><span class="sxs-lookup"><span data-stu-id="d7e14-222">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="d7e14-223">hello következő példa bemutatja, hogyan konfigurálhat egy hello első lemezt hozzá az toomanually toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d7e14-223">hello following example shows how toomanually configure hello first disk added toohello VM.</span></span> <span data-ttu-id="d7e14-224">Ez a folyamat is automatizálható hello segítségével [egyéni parancsprogramok futtatására szolgáló bővítmény](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="d7e14-224">This process can also be automated using hello [custom script extension](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="d7e14-225">Kézi konfigurálás</span><span class="sxs-lookup"><span data-stu-id="d7e14-225">Manual configuration</span></span>

<span data-ttu-id="d7e14-226">Hello virtuális gép RDP-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d7e14-226">Create an RDP connection with hello virtual machine.</span></span> <span data-ttu-id="d7e14-227">Nyissa meg a PowerShell és a parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="d7e14-227">Open up PowerShell and run this script.</span></span>

```powershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a><span data-ttu-id="d7e14-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7e14-228">Next steps</span></span>

<span data-ttu-id="d7e14-229">Ebben az oktatóprogramban megismerte méretű lemezek témakörök, mint:</span><span class="sxs-lookup"><span data-stu-id="d7e14-229">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7e14-230">Az operációs rendszer és a ideiglenes lemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-230">OS disks and temporary disks</span></span>
> * <span data-ttu-id="d7e14-231">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-231">Data disks</span></span>
> * <span data-ttu-id="d7e14-232">Standard és Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="d7e14-232">Standard and Premium disks</span></span>
> * <span data-ttu-id="d7e14-233">Lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="d7e14-233">Disk performance</span></span>
> * <span data-ttu-id="d7e14-234">Csatlakoztatása és adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="d7e14-234">Attaching and preparing data disks</span></span>

<span data-ttu-id="d7e14-235">Előzetes toohello oktatóanyag következő toolearn kapcsolatos automatizálása a Virtuálisgép-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d7e14-235">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d7e14-236">Virtuálisgép-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="d7e14-236">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)

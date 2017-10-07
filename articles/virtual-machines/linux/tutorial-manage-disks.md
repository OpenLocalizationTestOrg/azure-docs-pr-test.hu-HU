---
title: az Azure CLI hello lemezek aaaManage Azure |} Microsoft Docs
description: "Útmutató – az Azure CLI hello Azure lemezek kezelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: ad29cfbec5f9738f47705b19d0450c9a2f555492
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-disks-with-hello-azure-cli"></a><span data-ttu-id="53e07-103">Az Azure CLI hello Azure lemezek kezelése</span><span class="sxs-lookup"><span data-stu-id="53e07-103">Manage Azure disks with hello Azure CLI</span></span>

<span data-ttu-id="53e07-104">Az Azure virtuális gépek használata a lemezek toostore hello virtuális gépek operációs rendszer, alkalmazások és adatok.</span><span class="sxs-lookup"><span data-stu-id="53e07-104">Azure virtual machines use disks toostore hello VMs operating system, applications, and data.</span></span> <span data-ttu-id="53e07-105">Virtuális gép létrehozása esetén fontos toochoose a lemez méretét és a konfigurációs megfelelő toohello várható terhelést.</span><span class="sxs-lookup"><span data-stu-id="53e07-105">When creating a VM it is important toochoose a disk size and configuration appropriate toohello expected workload.</span></span> <span data-ttu-id="53e07-106">Ez az oktatóanyag ismerteti, központi telepítésére és felügyeletére a virtuális gépek lemezei.</span><span class="sxs-lookup"><span data-stu-id="53e07-106">This tutorial covers deploying and managing VM disks.</span></span> <span data-ttu-id="53e07-107">További tudnivalók:</span><span class="sxs-lookup"><span data-stu-id="53e07-107">You learn about:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53e07-108">Az operációs rendszer és a ideiglenes lemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-108">OS disks and temporary disks</span></span>
> * <span data-ttu-id="53e07-109">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-109">Data disks</span></span>
> * <span data-ttu-id="53e07-110">Standard és Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-110">Standard and Premium disks</span></span>
> * <span data-ttu-id="53e07-111">Lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="53e07-111">Disk performance</span></span>
> * <span data-ttu-id="53e07-112">Csatlakoztatása és adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="53e07-112">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="53e07-113">Lemezek átméretezése</span><span class="sxs-lookup"><span data-stu-id="53e07-113">Resizing disks</span></span>
> * <span data-ttu-id="53e07-114">Lemez pillanatképek</span><span class="sxs-lookup"><span data-stu-id="53e07-114">Disk snapshots</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="53e07-115">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="53e07-115">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="53e07-116">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="53e07-116">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="53e07-117">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="53e07-117">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="default-azure-disks"></a><span data-ttu-id="53e07-118">Azure-lemezeket alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="53e07-118">Default Azure disks</span></span>

<span data-ttu-id="53e07-119">Egy Azure virtuális gép létrehozásakor a két lemez automatikusan csatolt toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="53e07-119">When an Azure virtual machine is created, two disks are automatically attached toohello virtual machine.</span></span> 

<span data-ttu-id="53e07-120">**Operációsrendszer-lemez** - operációsrendszer-lemezek is kell méretezni too1 terabájt fel, és a gazdagépek hello virtuális gépek operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="53e07-120">**Operating system disk** - Operating system disks can be sized up too1 terabyte, and hosts hello VMs operating system.</span></span> <span data-ttu-id="53e07-121">az operációs rendszer hello lemez lett címkézve */dev/sda* alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="53e07-121">hello OS disk is labeled */dev/sda* by default.</span></span> <span data-ttu-id="53e07-122">hello lemez gyorsítótárazás hello operációsrendszer-lemez konfigurációját úgy optimalizálták, hogy az operációs rendszer teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="53e07-122">hello disk caching configuration of hello OS disk is optimized for OS performance.</span></span> <span data-ttu-id="53e07-123">Ez a konfiguráció miatt hello operációsrendszer-lemez **nem kell** alkalmazások vagy az adatok tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="53e07-123">Because of this configuration, hello OS disk **should not** host applications or data.</span></span> <span data-ttu-id="53e07-124">Az alkalmazások és adatok használja az adatlemezek, amely részletes leírást a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="53e07-124">For applications and data, use data disks, which are detailed later in this article.</span></span> 

<span data-ttu-id="53e07-125">**Ideiglenes lemez** -ideiglenes lemezeken található SSD-meghajtó használatát hello azonos Azure-gazdagép, hello VM.</span><span class="sxs-lookup"><span data-stu-id="53e07-125">**Temporary disk** - Temporary disks use a solid-state drive that is located on hello same Azure host as hello VM.</span></span> <span data-ttu-id="53e07-126">Ideiglenes lemezek magas performant és műveletek, például a ideiglenes adatfeldolgozási használható.</span><span class="sxs-lookup"><span data-stu-id="53e07-126">Temp disks are highly performant and may be used for operations such as temporary data processing.</span></span> <span data-ttu-id="53e07-127">Azonban ha hello VM áthelyezett tooa új gazdagép, eltávolítják egy ideiglenes lemezen tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="53e07-127">However, if hello VM is moved tooa new host, any data stored on a temporary disk is removed.</span></span> <span data-ttu-id="53e07-128">Virtuálisgép-méretet hello hello hello ideiglenes lemez mérete határozza meg.</span><span class="sxs-lookup"><span data-stu-id="53e07-128">hello size of hello temporary disk is determined by hello VM size.</span></span> <span data-ttu-id="53e07-129">Ideiglenes lemezek tartalma */dev/sdb* , és a csatlakoztatási pont a */mnt*.</span><span class="sxs-lookup"><span data-stu-id="53e07-129">Temporary disks are labeled */dev/sdb* and have a mountpoint of */mnt*.</span></span>

### <a name="temporary-disk-sizes"></a><span data-ttu-id="53e07-130">Ideiglenes mérete</span><span class="sxs-lookup"><span data-stu-id="53e07-130">Temporary disk sizes</span></span>

| <span data-ttu-id="53e07-131">Típus</span><span class="sxs-lookup"><span data-stu-id="53e07-131">Type</span></span> | <span data-ttu-id="53e07-132">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="53e07-132">VM Size</span></span> | <span data-ttu-id="53e07-133">Maximális ideiglenes lemez (GB)</span><span class="sxs-lookup"><span data-stu-id="53e07-133">Max temp disk size (GB)</span></span> |
|----|----|----|
| [<span data-ttu-id="53e07-134">Általános célú</span><span class="sxs-lookup"><span data-stu-id="53e07-134">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="53e07-135">A-D sorozat része</span><span class="sxs-lookup"><span data-stu-id="53e07-135">A and D series</span></span> | <span data-ttu-id="53e07-136">800</span><span class="sxs-lookup"><span data-stu-id="53e07-136">800</span></span> |
| [<span data-ttu-id="53e07-137">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-137">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="53e07-138">F sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-138">F series</span></span> | <span data-ttu-id="53e07-139">800</span><span class="sxs-lookup"><span data-stu-id="53e07-139">800</span></span> |
| [<span data-ttu-id="53e07-140">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-140">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="53e07-141">D és G sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-141">D and G series</span></span> | <span data-ttu-id="53e07-142">6144</span><span class="sxs-lookup"><span data-stu-id="53e07-142">6144</span></span> |
| [<span data-ttu-id="53e07-143">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-143">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="53e07-144">L sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-144">L series</span></span> | <span data-ttu-id="53e07-145">5630</span><span class="sxs-lookup"><span data-stu-id="53e07-145">5630</span></span> |
| [<span data-ttu-id="53e07-146">GPU</span><span class="sxs-lookup"><span data-stu-id="53e07-146">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="53e07-147">N sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-147">N series</span></span> | <span data-ttu-id="53e07-148">1440</span><span class="sxs-lookup"><span data-stu-id="53e07-148">1440</span></span> |
| [<span data-ttu-id="53e07-149">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="53e07-149">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="53e07-150">A és H sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-150">A and H series</span></span> | <span data-ttu-id="53e07-151">2000</span><span class="sxs-lookup"><span data-stu-id="53e07-151">2000</span></span> |

## <a name="azure-data-disks"></a><span data-ttu-id="53e07-152">Az Azure data lemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-152">Azure data disks</span></span>

<span data-ttu-id="53e07-153">További adatlemezt felvehető alkalmazások telepítése és adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="53e07-153">Additional data disks can be added for installing applications and storing data.</span></span> <span data-ttu-id="53e07-154">Az adatlemezek minden helyzetben, ahol van szükség az adatok tartósságát és rugalmas tárolási kell használni.</span><span class="sxs-lookup"><span data-stu-id="53e07-154">Data disks should be used in any situation where durable and responsive data storage is desired.</span></span> <span data-ttu-id="53e07-155">Minden adat lemez 1 terabájtnál maximális kapacitása nem.</span><span class="sxs-lookup"><span data-stu-id="53e07-155">Each data disk has a maximum capacity of 1 terabyte.</span></span> <span data-ttu-id="53e07-156">hello virtuális gép hello mérete határozza meg, hány adatlemezek csatolt tooa virtuális gép is lehet.</span><span class="sxs-lookup"><span data-stu-id="53e07-156">hello size of hello virtual machine determines how many data disks can be attached tooa VM.</span></span> <span data-ttu-id="53e07-157">A minden egyes virtuális core két adatlemezek kapcsolható ki.</span><span class="sxs-lookup"><span data-stu-id="53e07-157">For each VM core, two data disks can be attached.</span></span> 

### <a name="max-data-disks-per-vm"></a><span data-ttu-id="53e07-158">Maximális adatlemezek virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="53e07-158">Max data disks per VM</span></span>

| <span data-ttu-id="53e07-159">Típus</span><span class="sxs-lookup"><span data-stu-id="53e07-159">Type</span></span> | <span data-ttu-id="53e07-160">Virtuálisgép-mérettel</span><span class="sxs-lookup"><span data-stu-id="53e07-160">VM Size</span></span> | <span data-ttu-id="53e07-161">Maximális adatlemezek virtuális gépenként</span><span class="sxs-lookup"><span data-stu-id="53e07-161">Max data disks per VM</span></span> |
|----|----|----|
| [<span data-ttu-id="53e07-162">Általános célú</span><span class="sxs-lookup"><span data-stu-id="53e07-162">General purpose</span></span>](sizes-general.md) | <span data-ttu-id="53e07-163">A-D sorozat része</span><span class="sxs-lookup"><span data-stu-id="53e07-163">A and D series</span></span> | <span data-ttu-id="53e07-164">32</span><span class="sxs-lookup"><span data-stu-id="53e07-164">32</span></span> |
| [<span data-ttu-id="53e07-165">Számításra optimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-165">Compute optimized</span></span>](sizes-compute.md) | <span data-ttu-id="53e07-166">F sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-166">F series</span></span> | <span data-ttu-id="53e07-167">32</span><span class="sxs-lookup"><span data-stu-id="53e07-167">32</span></span> |
| [<span data-ttu-id="53e07-168">Memóriaoptimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-168">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md) | <span data-ttu-id="53e07-169">D és G sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-169">D and G series</span></span> | <span data-ttu-id="53e07-170">64</span><span class="sxs-lookup"><span data-stu-id="53e07-170">64</span></span> |
| [<span data-ttu-id="53e07-171">Tárolásra optimalizált</span><span class="sxs-lookup"><span data-stu-id="53e07-171">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md) | <span data-ttu-id="53e07-172">L sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-172">L series</span></span> | <span data-ttu-id="53e07-173">64</span><span class="sxs-lookup"><span data-stu-id="53e07-173">64</span></span> |
| [<span data-ttu-id="53e07-174">GPU</span><span class="sxs-lookup"><span data-stu-id="53e07-174">GPU</span></span>](sizes-gpu.md) | <span data-ttu-id="53e07-175">N sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-175">N series</span></span> | <span data-ttu-id="53e07-176">48</span><span class="sxs-lookup"><span data-stu-id="53e07-176">48</span></span> |
| [<span data-ttu-id="53e07-177">Magas teljesítmény</span><span class="sxs-lookup"><span data-stu-id="53e07-177">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="53e07-178">A és H sorozat</span><span class="sxs-lookup"><span data-stu-id="53e07-178">A and H series</span></span> | <span data-ttu-id="53e07-179">32</span><span class="sxs-lookup"><span data-stu-id="53e07-179">32</span></span> |

## <a name="vm-disk-types"></a><span data-ttu-id="53e07-180">Virtuális gép lemeztípusok</span><span class="sxs-lookup"><span data-stu-id="53e07-180">VM disk types</span></span>

<span data-ttu-id="53e07-181">Azure két lemez biztosít.</span><span class="sxs-lookup"><span data-stu-id="53e07-181">Azure provides two types of disk.</span></span>

### <a name="standard-disk"></a><span data-ttu-id="53e07-182">Standard méretű lemez</span><span class="sxs-lookup"><span data-stu-id="53e07-182">Standard disk</span></span>

<span data-ttu-id="53e07-183">A merevlemez-meghajtókra épülő Standard Storage költséghatékony tárolási megoldás, amely emellett jó teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="53e07-183">Standard Storage is backed by HDDs, and delivers cost-effective storage while still being performant.</span></span> <span data-ttu-id="53e07-184">Standard lemezek ideális, ha a költséghatékony fejlesztői és munkaterhelés teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="53e07-184">Standard disks are ideal for a cost effective dev and test workload.</span></span>

### <a name="premium-disk"></a><span data-ttu-id="53e07-185">Prémium szintű lemez</span><span class="sxs-lookup"><span data-stu-id="53e07-185">Premium disk</span></span>

<span data-ttu-id="53e07-186">Premium lemezek SSD-alapú nagy teljesítményű, alacsony késésű lemez üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="53e07-186">Premium disks are backed by SSD-based high-performance, low-latency disk.</span></span> <span data-ttu-id="53e07-187">Tökéletes éles munkaterhelést futtató virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="53e07-187">Perfect for VMs running production workload.</span></span> <span data-ttu-id="53e07-188">Prémium szintű Storage támogatja a DS-méretek, DSv2-méretek, GS sorozatnak és FS sorozatú virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="53e07-188">Premium Storage supports DS-series, DSv2-series, GS-series, and FS-series VMs.</span></span> <span data-ttu-id="53e07-189">Prémium szintű lemezekhez három típusa (P10, P20, P30) helyen, és hello lemez mérete hello hello lemez típusa határozza meg.</span><span class="sxs-lookup"><span data-stu-id="53e07-189">Premium disks come in three types (P10, P20, P30), hello size of hello disk determines hello disk type.</span></span> <span data-ttu-id="53e07-190">Ha választja, a lemez mérete hello értéke függvény toohello következő típusa.</span><span class="sxs-lookup"><span data-stu-id="53e07-190">When selecting, a disk size hello value is rounded up toohello next type.</span></span> <span data-ttu-id="53e07-191">Például ha hello lemez mérete 128 GB-nál kevesebb, hello lemeztípus P10.</span><span class="sxs-lookup"><span data-stu-id="53e07-191">For example, if hello disk size is less than 128 GB, hello disk type is P10.</span></span> <span data-ttu-id="53e07-192">Ha hello lemezméretet 129 GB és 512 GB közötti, hello mérete egy P20.</span><span class="sxs-lookup"><span data-stu-id="53e07-192">If hello disk size is between 129 GB and 512 GB, hello size is a P20.</span></span> <span data-ttu-id="53e07-193">Több mint 512 GB semmit hello mérete a P30.</span><span class="sxs-lookup"><span data-stu-id="53e07-193">Anything over 512 GB, hello size is a P30.</span></span>

### <a name="premium-disk-performance"></a><span data-ttu-id="53e07-194">Prémium szintű lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="53e07-194">Premium disk performance</span></span>

|<span data-ttu-id="53e07-195">Prémium szintű storage lemeztípus</span><span class="sxs-lookup"><span data-stu-id="53e07-195">Premium storage disk type</span></span> | <span data-ttu-id="53e07-196">P10</span><span class="sxs-lookup"><span data-stu-id="53e07-196">P10</span></span> | <span data-ttu-id="53e07-197">P20</span><span class="sxs-lookup"><span data-stu-id="53e07-197">P20</span></span> | <span data-ttu-id="53e07-198">P30</span><span class="sxs-lookup"><span data-stu-id="53e07-198">P30</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="53e07-199">A lemezméret (kerek mentése)</span><span class="sxs-lookup"><span data-stu-id="53e07-199">Disk size (round up)</span></span> | <span data-ttu-id="53e07-200">128 GB</span><span class="sxs-lookup"><span data-stu-id="53e07-200">128 GB</span></span> | <span data-ttu-id="53e07-201">512 GB</span><span class="sxs-lookup"><span data-stu-id="53e07-201">512 GB</span></span> | <span data-ttu-id="53e07-202">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="53e07-202">1,024 GB (1 TB)</span></span> |
| <span data-ttu-id="53e07-203">Lemezenkénti maximális IOPS-érték</span><span class="sxs-lookup"><span data-stu-id="53e07-203">Max IOPS per disk</span></span> | <span data-ttu-id="53e07-204">500</span><span class="sxs-lookup"><span data-stu-id="53e07-204">500</span></span> | <span data-ttu-id="53e07-205">2,300</span><span class="sxs-lookup"><span data-stu-id="53e07-205">2,300</span></span> | <span data-ttu-id="53e07-206">5,000</span><span class="sxs-lookup"><span data-stu-id="53e07-206">5,000</span></span> |
<span data-ttu-id="53e07-207">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="53e07-207">Throughput per disk</span></span> | <span data-ttu-id="53e07-208">100 MB/s</span><span class="sxs-lookup"><span data-stu-id="53e07-208">100 MB/s</span></span> | <span data-ttu-id="53e07-209">150 MB/s</span><span class="sxs-lookup"><span data-stu-id="53e07-209">150 MB/s</span></span> | <span data-ttu-id="53e07-210">200 MB/s</span><span class="sxs-lookup"><span data-stu-id="53e07-210">200 MB/s</span></span> |

<span data-ttu-id="53e07-211">Táblázat felett hello azonosítja a maximális iops-érték lemezenként, magasabb szintű teljesítmény legyen elérhető több adatlemezek szétosztott.</span><span class="sxs-lookup"><span data-stu-id="53e07-211">While hello above table identifies max IOPS per disk, a higher level of performance can be achieved by striping multiple data disks.</span></span> <span data-ttu-id="53e07-212">Virtuális gép Standard_GS5 például 80000 IOPS maximális érhető el.</span><span class="sxs-lookup"><span data-stu-id="53e07-212">For instance, a Standard_GS5 VM can achieve a maximum of 80,000 IOPS.</span></span> <span data-ttu-id="53e07-213">A virtuális gép maximális iops-értéket részletes információkért lásd: [Linux Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="53e07-213">For detailed information on max IOPS per VM, see [Linux VM sizes](sizes.md).</span></span>

## <a name="create-and-attach-disks"></a><span data-ttu-id="53e07-214">Hozzon létre, és csatlakoztassa a lemezeket</span><span class="sxs-lookup"><span data-stu-id="53e07-214">Create and attach disks</span></span>

<span data-ttu-id="53e07-215">Az adatlemezek hozható létre és csatolni a virtuális gép létrehozásakor vagy meglévő virtuális gép tooan.</span><span class="sxs-lookup"><span data-stu-id="53e07-215">Data disks can be created and attached at VM creation time or tooan existing VM.</span></span>

### <a name="attach-disk-at-vm-creation"></a><span data-ttu-id="53e07-216">A virtuális gép létrehozásakor lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="53e07-216">Attach disk at VM creation</span></span>

<span data-ttu-id="53e07-217">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-217">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

<span data-ttu-id="53e07-218">Hello virtuális gép létrehozása [az virtuális gép létrehozása]( /cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-218">Create a VM using hello [az vm create]( /cli/azure/vm#create) command.</span></span> <span data-ttu-id="53e07-219">Hello `--datadisk-sizes-gb` argumentum használt toospecify, hogy egy további szabad létre kell hoznia és toohello virtuális géphez csatolni.</span><span class="sxs-lookup"><span data-stu-id="53e07-219">hello `--datadisk-sizes-gb` argument is used toospecify that an additional disk should be created and attached toohello virtual machine.</span></span> <span data-ttu-id="53e07-220">toocreate és egynél több lemez csatolása, a lemez mérete értékének szóközökkel elválasztott kötetnevek listáját használja.</span><span class="sxs-lookup"><span data-stu-id="53e07-220">toocreate and attach more than one disk, use a space-delimited list of disk size values.</span></span> <span data-ttu-id="53e07-221">A következő példa hello a virtuális gépek két adatlemezek, mindkét 128 GB hozza létre.</span><span class="sxs-lookup"><span data-stu-id="53e07-221">In hello following example, a VM is created with two data disks, both 128 GB.</span></span> <span data-ttu-id="53e07-222">Mivel hello mérete 128 GB-os, ezek a lemezek egyaránt be van állítva, P10s, amely legfeljebb 500 iops-érték lemezenként.</span><span class="sxs-lookup"><span data-stu-id="53e07-222">Because hello disk sizes are 128 GB, these disks are both configured as P10s, which provide maximum 500 IOPS per disk.</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-tooexisting-vm"></a><span data-ttu-id="53e07-223">Csatlakoztassa a lemezt tooexisting VM</span><span class="sxs-lookup"><span data-stu-id="53e07-223">Attach disk tooexisting VM</span></span>

<span data-ttu-id="53e07-224">toocreate és csatlakoztassa az új lemez tooan meglévő virtuális gépet, hello használata [az méretű lemez csatolása](/cli/azure/vm/disk#attach) parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-224">toocreate and attach a new disk tooan existing virtual machine, use hello [az vm disk attach](/cli/azure/vm/disk#attach) command.</span></span> <span data-ttu-id="53e07-225">hello alábbi példa létrehoz egy prémium szintű lemez 128 GB-nál, és csatolja azt toohello hello utolsó lépésben létrehozott virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="53e07-225">hello following example creates a premium disk, 128 gigabytes in size, and attaches it toohello VM created in hello last step.</span></span>

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a><span data-ttu-id="53e07-226">Az adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="53e07-226">Prepare data disks</span></span>

<span data-ttu-id="53e07-227">A lemez virtuális géphez csatolt toohello követően hello operációs rendszer kell konfigurált toobe toouse hello lemez.</span><span class="sxs-lookup"><span data-stu-id="53e07-227">Once a disk has been attached toohello virtual machine, hello operating system needs toobe configured toouse hello disk.</span></span> <span data-ttu-id="53e07-228">hello a következő példa bemutatja, hogyan konfigurálhat egy toomanually egy lemezt.</span><span class="sxs-lookup"><span data-stu-id="53e07-228">hello following example shows how toomanually configure a disk.</span></span> <span data-ttu-id="53e07-229">Ez a folyamat is automatizálható a felhő-init használatával egy [az oktatóanyag későbbi](./tutorial-automate-vm-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="53e07-229">This process can also be automated using cloud-init, which is covered in a [later tutorial](./tutorial-automate-vm-deployment.md).</span></span>

### <a name="manual-configuration"></a><span data-ttu-id="53e07-230">Kézi konfigurálás</span><span class="sxs-lookup"><span data-stu-id="53e07-230">Manual configuration</span></span>

<span data-ttu-id="53e07-231">Az SSH-kapcsolat létrehozása hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="53e07-231">Create an SSH connection with hello virtual machine.</span></span> <span data-ttu-id="53e07-232">Cserélje le hello példa IP-cím hello nyilvános IP-cím hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="53e07-232">Replace hello example IP address with hello public IP of hello virtual machine.</span></span>

```azurecli-interactive 
ssh 52.174.34.95
```

<span data-ttu-id="53e07-233">Partíció hello lemezzel `fdisk`.</span><span class="sxs-lookup"><span data-stu-id="53e07-233">Partition hello disk with `fdisk`.</span></span>

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="53e07-234">Egy fájl rendszerpartíciót toohello írási hello segítségével `mkfs` parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-234">Write a file system toohello partition by using hello `mkfs` command.</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="53e07-235">Csatlakoztassa a hello új lemezt, így hello operációs rendszerben érhető el.</span><span class="sxs-lookup"><span data-stu-id="53e07-235">Mount hello new disk so that it is accessible in hello operating system.</span></span>

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="53e07-236">hello lemez most már a következőkkel érhetők el hello *datadrive* hello futtatásával ellenőrizheti csatlakozási pont `df -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-236">hello disk can now be accessed through hello *datadrive* mountpoint, which can be verified by running hello `df -h` command.</span></span> 

```bash
df -h
```

<span data-ttu-id="53e07-237">hello az alábbiakat mutatja be hello új meghajtót csatlakoztatott */datadrive*.</span><span class="sxs-lookup"><span data-stu-id="53e07-237">hello output shows hello new drive mounted on */datadrive*.</span></span>

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

<span data-ttu-id="53e07-238">amely a meghajtó hello tooensure van csatlakoztatni, a rendszer újraindítása után, akkor hozzá kell adni toohello */etc/fstab* fájlt.</span><span class="sxs-lookup"><span data-stu-id="53e07-238">tooensure that hello drive is remounted after a reboot, it must be added toohello */etc/fstab* file.</span></span> <span data-ttu-id="53e07-239">toodo tehát beolvasása hello UUID hello lemez a hello `blkid` segédprogram.</span><span class="sxs-lookup"><span data-stu-id="53e07-239">toodo so, get hello UUID of hello disk with hello `blkid` utility.</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="53e07-240">hello kimenete felsorolja hello hello meghajtó UUID `/dev/sdc1` ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="53e07-240">hello output displays hello UUID of hello drive, `/dev/sdc1` in this case.</span></span>

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

<span data-ttu-id="53e07-241">Adja hozzá a következő toohello sor hasonló toohello */etc/fstab* fájlt.</span><span class="sxs-lookup"><span data-stu-id="53e07-241">Add a line similar toohello following toohello */etc/fstab* file.</span></span> <span data-ttu-id="53e07-242">Is vegye figyelembe, hogy az írási korlátok használatával tiltható *korlát = 0*, ez a konfiguráció lemez a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="53e07-242">Also note that write barriers can be disabled using *barrier=0*, this configuration can improve disk performance.</span></span> 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

<span data-ttu-id="53e07-243">Most, hogy hello lemez van konfigurálva, zárja be a hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="53e07-243">Now that hello disk has been configured, close hello SSH session.</span></span>

```bash
exit
```

## <a name="resize-vm-disk"></a><span data-ttu-id="53e07-244">Virtuálisgép-lemez átméretezése</span><span class="sxs-lookup"><span data-stu-id="53e07-244">Resize VM disk</span></span>

<span data-ttu-id="53e07-245">Ha egy virtuális Gépre van telepítve, a hello operációsrendszer-lemez vagy a mellékelt adatok lemezzel mérete növelhető.</span><span class="sxs-lookup"><span data-stu-id="53e07-245">Once a VM has been deployed, hello operating system disk or any attached data disks can be increased in size.</span></span> <span data-ttu-id="53e07-246">A lemez mérete hello növelése akkor előnyös, ha több tárhelyet igényel vagy magasabb szintű teljesítmény (P10, P20, P30).</span><span class="sxs-lookup"><span data-stu-id="53e07-246">Increasing hello size of a disk is beneficial when needing more storage space or a higher level of performance (P10, P20, P30).</span></span> <span data-ttu-id="53e07-247">Vegye figyelembe, hogy lemezek csökkenteni mérete nem lehet.</span><span class="sxs-lookup"><span data-stu-id="53e07-247">Note, disks cannot be decreased in size.</span></span>

<span data-ttu-id="53e07-248">Lemez méretének növelése, előtt azonosító hello, vagy hello lemez neve van szükség.</span><span class="sxs-lookup"><span data-stu-id="53e07-248">Before increasing disk size, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="53e07-249">Használjon hello [az Lemezlista](/cli/azure/vm/disk#list) parancs tooreturn összes erőforráscsoport lemezeit.</span><span class="sxs-lookup"><span data-stu-id="53e07-249">Use hello [az disk list](/cli/azure/vm/disk#list) command tooreturn all disks in a resource group.</span></span> <span data-ttu-id="53e07-250">Jegyezze fel a hello lemez neve, hogy szeretné-e tooresize.</span><span class="sxs-lookup"><span data-stu-id="53e07-250">Take note of hello disk name that you would like tooresize.</span></span>

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

<span data-ttu-id="53e07-251">virtuális gép hello is felszabadított kell lennie.</span><span class="sxs-lookup"><span data-stu-id="53e07-251">hello VM must also be deallocated.</span></span> <span data-ttu-id="53e07-252">Használjon hello [az virtuális gép felszabadítása]( /cli/azure/vm#deallocate) toostop parancsot, és hello VM felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="53e07-252">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="53e07-253">Használjon hello [az lemez frissítés](/cli/azure/vm/disk#update) parancs tooresize hello lemez.</span><span class="sxs-lookup"><span data-stu-id="53e07-253">Use hello [az disk update](/cli/azure/vm/disk#update) command tooresize hello disk.</span></span> <span data-ttu-id="53e07-254">Ez a példa átméretezi nevű lemez *myDataDisk* too1 terabájt.</span><span class="sxs-lookup"><span data-stu-id="53e07-254">This example resizes a disk named *myDataDisk* too1 terabyte.</span></span>

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

<span data-ttu-id="53e07-255">Hello átméretezés befejezése után indítsa el a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="53e07-255">Once hello resize operation has completed, start hello VM.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="53e07-256">Ha már átméretezték, ezért hello operációs rendszer tárolására, a rendszer automatikusan kiegészíti a hello partíció.</span><span class="sxs-lookup"><span data-stu-id="53e07-256">If you’ve resized hello operating system disk, hello partition is automatically be expanded.</span></span> <span data-ttu-id="53e07-257">Ha adatlemezt átméretezte, aktuális partíciókat kell toobe hello virtuális gépek operációs rendszer bővíthető.</span><span class="sxs-lookup"><span data-stu-id="53e07-257">If you have resized a data disk, any current partitions need toobe expanded in hello VMs operating system.</span></span>

## <a name="snapshot-azure-disks"></a><span data-ttu-id="53e07-258">Pillanatkép készítése az Azure-lemezeket</span><span class="sxs-lookup"><span data-stu-id="53e07-258">Snapshot Azure disks</span></span>

<span data-ttu-id="53e07-259">Lemez pillanatkép létrehozása van folyamatban hello lemez olvasási csak, pont időponthoz kötött másolatot készít.</span><span class="sxs-lookup"><span data-stu-id="53e07-259">Taking a disk snapshot creates a read only, point-in-time copy of hello disk.</span></span> <span data-ttu-id="53e07-260">Az Azure virtuális gép pillanatképei hasznos, ha gyorsan állapotmentést hello a virtuális gépek konfigurációs módosítások végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="53e07-260">Azure VM snapshots are useful for quickly saving hello state of a VM before making configuration changes.</span></span> <span data-ttu-id="53e07-261">Hello esemény hello konfigurációs módosításokat a rendszer nem kívánt toobe, VM állapotának hello pillanatkép állíthatók vissza.</span><span class="sxs-lookup"><span data-stu-id="53e07-261">In hello event hello configuration changes prove toobe undesired, VM state can be restored using hello snapshot.</span></span> <span data-ttu-id="53e07-262">Ha egy virtuális gép több lemez, a rendszer pillanatképet készít minden lemez függetlenül hello másoknak.</span><span class="sxs-lookup"><span data-stu-id="53e07-262">When a VM has more than one disk, a snapshot is taken of each disk independently of hello others.</span></span> <span data-ttu-id="53e07-263">Alkalmazás konzisztens biztonsági másolatok fogadására, fontolja meg a lemez pillanatképek készítése előtt hello VM leállítása.</span><span class="sxs-lookup"><span data-stu-id="53e07-263">For taking application consistent backups, consider stopping hello VM before taking disk snapshots.</span></span> <span data-ttu-id="53e07-264">Másik megoldásként használhatja a hello [Azure Backup szolgáltatás](/azure/backup/), mely lehetővé teszi, hogy Ön tooperform automatikus biztonsági mentés hello virtuális gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="53e07-264">Alternatively, use hello [Azure Backup service](/azure/backup/), which enables you tooperform automated backups while hello VM is running.</span></span>

### <a name="create-snapshot"></a><span data-ttu-id="53e07-265">Pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="53e07-265">Create snapshot</span></span>

<span data-ttu-id="53e07-266">Mielőtt létrehozna egy virtuálisgép-lemez pillanatképét, hello hello lemez nevű vagy azonosítójú van szükség.</span><span class="sxs-lookup"><span data-stu-id="53e07-266">Before creating a virtual machine disk snapshot, hello Id or name of hello disk is needed.</span></span> <span data-ttu-id="53e07-267">Használjon hello [az vm megjelenítése](https://docs.microsoft.com/en-us/cli/azure/vm#show) parancs tooreturn hello lemezazonosítót. Ebben a példában hello lemezazonosító van egy változó tárolja, hogy egy későbbi lépésben használható.</span><span class="sxs-lookup"><span data-stu-id="53e07-267">Use hello [az vm show](https://docs.microsoft.com/en-us/cli/azure/vm#show) command tooreturn hello disk id. In this example, hello disk id is stored in a variable so that it can be used in a later step.</span></span>

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

<span data-ttu-id="53e07-268">Most, hogy hello virtuálisgép-lemez hello azonosítója, hello következő parancs létrehoz egy pillanatképet hello lemez.</span><span class="sxs-lookup"><span data-stu-id="53e07-268">Now that you have hello id of hello virtual machine disk, hello following command creates a snapshot of hello disk.</span></span>

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a><span data-ttu-id="53e07-269">Pillanatképből lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="53e07-269">Create disk from snapshot</span></span>

<span data-ttu-id="53e07-270">A pillanatkép majd létrehozható egy lemezt, amely használt toorecreate hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="53e07-270">This snapshot can then be converted into a disk, which can be used toorecreate hello virtual machine.</span></span>

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a><span data-ttu-id="53e07-271">Virtuális gép pillanatkép visszaállítása</span><span class="sxs-lookup"><span data-stu-id="53e07-271">Restore virtual machine from snapshot</span></span>

<span data-ttu-id="53e07-272">toodemonstrate virtuális gép helyreállítása, törlési hello meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="53e07-272">toodemonstrate virtual machine recovery, delete hello existing virtual machine.</span></span> 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

<span data-ttu-id="53e07-273">Hozzon létre egy új virtuális gép hello pillanatkép lemezről.</span><span class="sxs-lookup"><span data-stu-id="53e07-273">Create a new virtual machine from hello snapshot disk.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a><span data-ttu-id="53e07-274">Csatlakoztassa újra az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="53e07-274">Reattach data disk</span></span>

<span data-ttu-id="53e07-275">Minden adatlemezek kell objektumkörnyezetben toobe toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="53e07-275">All data disks need toobe reattached toohello virtual machine.</span></span>

<span data-ttu-id="53e07-276">Először található hello adatok Lemeznév hello segítségével [az Lemezlista](https://docs.microsoft.com/cli/azure/disk#list) parancsot.</span><span class="sxs-lookup"><span data-stu-id="53e07-276">First find hello data disk name using hello [az disk list](https://docs.microsoft.com/cli/azure/disk#list) command.</span></span> <span data-ttu-id="53e07-277">Ez a Példa helyek hello nevű változóban hello lemez neve *datadisk*, amellyel hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="53e07-277">This example places hello name of hello disk in a variable named *datadisk*, which is used in hello next step.</span></span>

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

<span data-ttu-id="53e07-278">Használjon hello [az méretű lemez csatolása](https://docs.microsoft.com/cli/azure/vm/disk#attach) parancs tooattach hello lemez.</span><span class="sxs-lookup"><span data-stu-id="53e07-278">Use hello [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach) command tooattach hello disk.</span></span>

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a><span data-ttu-id="53e07-279">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53e07-279">Next steps</span></span>

<span data-ttu-id="53e07-280">Ebben az oktatóprogramban megismerte méretű lemezek témakörök, mint:</span><span class="sxs-lookup"><span data-stu-id="53e07-280">In this tutorial, you learned about VM disks topics such as:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="53e07-281">Az operációs rendszer és a ideiglenes lemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-281">OS disks and temporary disks</span></span>
> * <span data-ttu-id="53e07-282">Az adatlemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-282">Data disks</span></span>
> * <span data-ttu-id="53e07-283">Standard és Premium lemezek</span><span class="sxs-lookup"><span data-stu-id="53e07-283">Standard and Premium disks</span></span>
> * <span data-ttu-id="53e07-284">Lemez teljesítménye</span><span class="sxs-lookup"><span data-stu-id="53e07-284">Disk performance</span></span>
> * <span data-ttu-id="53e07-285">Csatlakoztatása és adatlemezek előkészítése</span><span class="sxs-lookup"><span data-stu-id="53e07-285">Attaching and preparing data disks</span></span>
> * <span data-ttu-id="53e07-286">Lemezek átméretezése</span><span class="sxs-lookup"><span data-stu-id="53e07-286">Resizing disks</span></span>
> * <span data-ttu-id="53e07-287">Lemez pillanatképek</span><span class="sxs-lookup"><span data-stu-id="53e07-287">Disk snapshots</span></span>

<span data-ttu-id="53e07-288">Előzetes toohello oktatóanyag következő toolearn kapcsolatos automatizálása a Virtuálisgép-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="53e07-288">Advance toohello next tutorial toolearn about automating VM configuration.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="53e07-289">Virtuálisgép-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="53e07-289">Automate VM configuration</span></span>](./tutorial-automate-vm-deployment.md)

---
title: "Prémium szintű Azure Storage áttelepítése virtuális gépek |} Microsoft Docs"
description: "Prémium szintű Azure Storage telepíthet át a meglévő virtuális gépekre. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
services: storage
documentationcenter: na
author: yuemlu
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: yuemlu
ms.openlocfilehash: ca893f87b155a92c457e3bf6d9d39aaf86bf5fb3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="migrating-to-azure-premium-storage-unmanaged-disks"></a><span data-ttu-id="35a04-104">Prémium szintű Azure Storage (nem felügyelt lemezek) áttelepítése</span><span class="sxs-lookup"><span data-stu-id="35a04-104">Migrating to Azure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-105">A cikkből megtudhatja, hogyan telepíthet át egy virtuális Gépet, amely egy nem felügyelt prémium lemezeket használó virtuális géphez nem felügyelt standard lemezek használja.</span><span class="sxs-lookup"><span data-stu-id="35a04-105">This article discusses how to migrate a VM that uses unmanaged standard disks to a VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="35a04-106">Javasoljuk, hogy Azure felügyelt lemezek használatakor az új virtuális gépek, és a korábbi nem felügyelt lemezek átalakítása felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="35a04-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks to managed disks.</span></span> <span data-ttu-id="35a04-107">Felügyelt lemezek leíró az alapul szolgáló storage-fiókok, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="35a04-107">Managed Disks handle the underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="35a04-108">További információkért lásd: a [felügyelt lemezekhez – áttekintés](../../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-108">For more information, please see our [Managed Disks Overview](../../virtual-machines/windows/managed-disks-overview.md).</span></span>
>

<span data-ttu-id="35a04-109">Prémium szintű Storage nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása nyújt.</span><span class="sxs-lookup"><span data-stu-id="35a04-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="35a04-110">Prémium szintű Azure Storage át kell telepítenie az alkalmazás virtuális gépek lemezei is igénybe vehet a sebesség előnyeit, és ezek a lemezek teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="35a04-110">You can take advantage of the speed and performance of these disks by migrating your application's VM disks to Azure Premium Storage.</span></span>

<span data-ttu-id="35a04-111">Ez az útmutató célja a prémium szintű Azure Storage jobb új felhasználók zökkenőmentes váltásban az aktuális rendszerből prémium szintű Storage előkészítése.</span><span class="sxs-lookup"><span data-stu-id="35a04-111">The purpose of this guide is to help new users of Azure Premium Storage better prepare to make a smooth transition from their current system to Premium Storage.</span></span> <span data-ttu-id="35a04-112">Az útmutató foglalkozik a legfontosabb összetevők, a folyamat három:</span><span class="sxs-lookup"><span data-stu-id="35a04-112">The guide addresses three of the key components in this process:</span></span>

* [<span data-ttu-id="35a04-113">Prémium szintű Storage az áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="35a04-113">Plan for the Migration to Premium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="35a04-114">Készítse elő, és másolja át a virtuális merevlemezek (VHD) prémium szintű Storage</span><span class="sxs-lookup"><span data-stu-id="35a04-114">Prepare and Copy Virtual Hard Disks (VHDs) to Premium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="35a04-115">Prémium szintű Storage használata Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="35a04-116">Prémium szintű Azure Storage át virtuális gépeket más platformokon, vagy meglévő Azure virtuális gépek áttelepítésére Standard tárolási prémium szintű Storage.</span><span class="sxs-lookup"><span data-stu-id="35a04-116">You can either migrate VMs from other platforms to Azure Premium Storage or migrate existing Azure VMs from Standard Storage to Premium Storage.</span></span> <span data-ttu-id="35a04-117">Ez az útmutató mindkét két forgatókönyv lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="35a04-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="35a04-118">Hajtsa végre a forgatókönyvtől függően vonatkozó szakaszában megadott lépéseket.</span><span class="sxs-lookup"><span data-stu-id="35a04-118">Follow the steps specified in the relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-119">A szolgáltatás áttekintése és a prémium szintű Storage, a prémium szintű Storage árképzési található: [nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="35a04-120">Azt javasoljuk, hogy minden virtuális gép lemezét, prémium szintű Azure Storage magas IOPS igénylő a legjobb teljesítmény érdekében az alkalmazás áttelepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-120">We recommend migrating any virtual machine disk requiring high IOPS to Azure Premium Storage for the best performance for your application.</span></span> <span data-ttu-id="35a04-121">Ha a lemez nem igényli a magas iops értéket, korlátozhatja a költségek megőrizve a szabványos tárolóban, a (merevlemezes HDD) meghajtók SSD-k helyett virtuálisgép-lemez adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="35a04-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="35a04-122">Az áttelepítési folyamat egészében befejezése szükség lehet további műveletek előtt és után az útmutatóban ismertetett lépéseket.</span><span class="sxs-lookup"><span data-stu-id="35a04-122">Completing the migration process in its entirety may require additional actions both before and after the steps provided in this guide.</span></span> <span data-ttu-id="35a04-123">Például konfigurálható virtuális hálózatok és a végpontok vagy magának az alkalmazásnak, amely előfordulhat, hogy az alkalmazás bizonyos időre leállítást belül kód módosítása.</span><span class="sxs-lookup"><span data-stu-id="35a04-123">Examples include configuring virtual networks or endpoints or making code changes within the application itself which may require some downtime in your application.</span></span> <span data-ttu-id="35a04-124">Ezek a műveletek minden alkalmazáshoz egyedi, és el kell végeznie ezeket az itt ismertetett lépéseket: Ez az útmutató az átállásra teljes prémium szintű Storage, zökkenőmentes lehető együtt.</span><span class="sxs-lookup"><span data-stu-id="35a04-124">These actions are unique to each application, and you should complete them along with the steps provided in this guide to make the full transition to Premium Storage as seamless as possible.</span></span>

## <span data-ttu-id="35a04-125"><a name="plan-the-migration-to-premium-storage"></a>Prémium szintű Storage az áttelepítés tervezése</span><span class="sxs-lookup"><span data-stu-id="35a04-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for the migration to Premium Storage</span></span>
<span data-ttu-id="35a04-126">Ez a szakasz biztosítja, hogy készen áll az áttelepítési lépéseket a cikkben, és segítséget nyújt a legjobb döntést a virtuális gép és a lemez típusok.</span><span class="sxs-lookup"><span data-stu-id="35a04-126">This section ensures that you are ready to follow the migration steps in this article, and helps you to make the best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="35a04-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35a04-127">Prerequisites</span></span>
* <span data-ttu-id="35a04-128">Szüksége lesz egy Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="35a04-128">You will need an Azure subscription.</span></span> <span data-ttu-id="35a04-129">Ha még nincs fiókja, létrehozhat egy hónapos [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/) előfizetés, vagy keresse fel [Azure díjszabása](https://azure.microsoft.com/pricing/) további lehetőségekért.</span><span class="sxs-lookup"><span data-stu-id="35a04-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="35a04-130">PowerShell-parancsmagokkal hajtható végre, szüksége lesz a Microsoft Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="35a04-130">To execute PowerShell cmdlets, you will need the Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="35a04-131">A telepítési helyre és a telepítésre vonatkozó utasításokért lásd: [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="35a04-131">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the install point and installation instructions.</span></span>
* <span data-ttu-id="35a04-132">Prémium szintű Storage futó Azure virtuális gépek tervezésekor kell használnia a prémium szintű Storage képes a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="35a04-132">When you plan to use Azure VMs running on Premium Storage, you need to use the Premium Storage capable VMs.</span></span> <span data-ttu-id="35a04-133">Prémium szintű Storage képes a virtuális gépek a Standard és prémium szintű Storage lemezek is használhatók.</span><span class="sxs-lookup"><span data-stu-id="35a04-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="35a04-134">Prémium szintű storage lemezek lesz elérhető további VM-típussal a jövőben.</span><span class="sxs-lookup"><span data-stu-id="35a04-134">Premium storage disks will be available with more VM types in the future.</span></span> <span data-ttu-id="35a04-135">További információ az összes elérhető Azure virtuális lemez típusát és méretét: [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Felhőszolgáltatások mérete](../../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="35a04-136">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="35a04-136">Considerations</span></span>
<span data-ttu-id="35a04-137">Egy Azure virtuális gép támogatja a prémium szintű Storage több lemezt csatolni, így az alkalmazások legfeljebb 256 TB-nyi tárhelyre virtuális gépenként.</span><span class="sxs-lookup"><span data-stu-id="35a04-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up to 256 TB of storage per VM.</span></span> <span data-ttu-id="35a04-138">Prémium szintű Storage az alkalmazások egy második lemez adatátviteli sebessége virtuális gépenként a rendkívül alacsony késleltetésű az olvasási műveletek érhet 80000 iops-értéket (bemeneti/kimeneti műveletek száma másodpercenként) virtuális Gépet és 2000 MB.</span><span class="sxs-lookup"><span data-stu-id="35a04-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="35a04-139">Virtuális gépek és a lemezek számos lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="35a04-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="35a04-140">Ez a szakasz célja segíteni olyan beállítás, amely a legjobban megfelel a számítási feladatok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-140">This section is to help you to find an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="35a04-141">A virtuális gépek mérete</span><span class="sxs-lookup"><span data-stu-id="35a04-141">VM sizes</span></span>
<span data-ttu-id="35a04-142">Az Azure virtuális gép mérete paramétereknek szereplő [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="35a04-142">The Azure VM size specifications are listed in [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="35a04-143">Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok teljesítményétől.</span><span class="sxs-lookup"><span data-stu-id="35a04-143">Review the performance characteristics of virtual machines that work with Premium Storage and choose the most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="35a04-144">Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális Gépet, a lemez forgalom alapjául.</span><span class="sxs-lookup"><span data-stu-id="35a04-144">Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="35a04-145">Lemezméretek</span><span class="sxs-lookup"><span data-stu-id="35a04-145">Disk sizes</span></span>
<span data-ttu-id="35a04-146">Öt különböző típusú lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok.</span><span class="sxs-lookup"><span data-stu-id="35a04-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="35a04-147">Vegye figyelembe a működés felső korlátjának Ha a lemez kiválasztása a virtuális gép alapján a kapacitás, a teljesítmény, méretezhetőség az alkalmazás igényeinek megfelelően, és csúcs tölti be.</span><span class="sxs-lookup"><span data-stu-id="35a04-147">Take into consideration these limits when choosing the disk type for your VM based on the needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="35a04-148">Prémium szintű lemezek típusa</span><span class="sxs-lookup"><span data-stu-id="35a04-148">Premium Disks Type</span></span>  | <span data-ttu-id="35a04-149">P10</span><span class="sxs-lookup"><span data-stu-id="35a04-149">P10</span></span>   | <span data-ttu-id="35a04-150">P20</span><span class="sxs-lookup"><span data-stu-id="35a04-150">P20</span></span>   | <span data-ttu-id="35a04-151">P30</span><span class="sxs-lookup"><span data-stu-id="35a04-151">P30</span></span>            | <span data-ttu-id="35a04-152">P40</span><span class="sxs-lookup"><span data-stu-id="35a04-152">P40</span></span>            | <span data-ttu-id="35a04-153">P50</span><span class="sxs-lookup"><span data-stu-id="35a04-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="35a04-154">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="35a04-154">Disk size</span></span>           | <span data-ttu-id="35a04-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="35a04-155">128 GB</span></span>| <span data-ttu-id="35a04-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="35a04-156">512 GB</span></span>| <span data-ttu-id="35a04-157">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="35a04-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="35a04-158">2048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="35a04-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="35a04-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="35a04-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="35a04-160">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="35a04-160">IOPS per disk</span></span>       | <span data-ttu-id="35a04-161">500</span><span class="sxs-lookup"><span data-stu-id="35a04-161">500</span></span>   | <span data-ttu-id="35a04-162">2300</span><span class="sxs-lookup"><span data-stu-id="35a04-162">2300</span></span>  | <span data-ttu-id="35a04-163">5000</span><span class="sxs-lookup"><span data-stu-id="35a04-163">5000</span></span>           | <span data-ttu-id="35a04-164">7500</span><span class="sxs-lookup"><span data-stu-id="35a04-164">7500</span></span>           | <span data-ttu-id="35a04-165">7500</span><span class="sxs-lookup"><span data-stu-id="35a04-165">7500</span></span>           | 
| <span data-ttu-id="35a04-166">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="35a04-166">Throughput per disk</span></span> | <span data-ttu-id="35a04-167">100 MB / s</span><span class="sxs-lookup"><span data-stu-id="35a04-167">100 MB per second</span></span> | <span data-ttu-id="35a04-168">150 MB / s</span><span class="sxs-lookup"><span data-stu-id="35a04-168">150 MB per second</span></span> | <span data-ttu-id="35a04-169">200 MB / s</span><span class="sxs-lookup"><span data-stu-id="35a04-169">200 MB per second</span></span> | <span data-ttu-id="35a04-170">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="35a04-170">250 MB per second</span></span> | <span data-ttu-id="35a04-171">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="35a04-171">250 MB per second</span></span> |

<span data-ttu-id="35a04-172">Attól függően, hogy a munkaterhelés annak eldöntése, hogy adatlemeznek a virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="35a04-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="35a04-173">Több állandó adatlemezt lehet kapcsolódni a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="35a04-173">You can attach several persistent data disks to your VM.</span></span> <span data-ttu-id="35a04-174">Ha szükséges, a is paritásos a lemezeken, a kapacitás és a kötet teljesítményének növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="35a04-174">If needed, you can stripe across the disks to increase the capacity and performance of the volume.</span></span> <span data-ttu-id="35a04-175">(Megtudhatja, mi lemez csíkozást [Itt](storage-premium-storage-performance.md#disk-striping).) Ha Ön paritásos prémium szintű Storage adatlemezek használata [tárolóhelyek][4], úgy kell beállítania, egyoszlopos használt lemezek.</span><span class="sxs-lookup"><span data-stu-id="35a04-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="35a04-176">Ellenkező esetben a csíkozott kötet teljesítménye lehet alacsonyabb, mint a várt forgalom egyenetlen eloszlását miatt a lemezeken.</span><span class="sxs-lookup"><span data-stu-id="35a04-176">Otherwise, the overall performance of the striped volume may be lower than expected due to uneven distribution of traffic across the disks.</span></span> <span data-ttu-id="35a04-177">A Linux virtuális gépek használhatják a *mdadm* segédprogram azonos eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-177">For Linux VMs you can use the *mdadm* utility to achieve the same.</span></span> <span data-ttu-id="35a04-178">Cikke [szoftver RAID konfigurálása Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="35a04-178">See article [Configure Software RAID on Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="35a04-179">Tárfiókra vonatkozó méretezhetőségi célok</span><span class="sxs-lookup"><span data-stu-id="35a04-179">Storage account scalability targets</span></span>
<span data-ttu-id="35a04-180">Prémium szintű Storage-fiókok a következő méretezhetőségi célok kívül van a [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-180">Premium Storage accounts have the following scalability targets in addition to the [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="35a04-181">Ha az alkalmazás követelményeinek túllépi a méretezhetőségi célok egyetlen tárfiók, építenie az alkalmazást több tárfiókot használni, és az adatok particionálása adott tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="35a04-181">If your application requirements exceed the scalability targets of a single storage account, build your application to use multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="35a04-182">Teljes kapacitásával</span><span class="sxs-lookup"><span data-stu-id="35a04-182">Total Account Capacity</span></span> | <span data-ttu-id="35a04-183">A helyileg redundáns tárolás fiók teljes sávszélesség</span><span class="sxs-lookup"><span data-stu-id="35a04-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="35a04-184">Lemez kapacitás: 35TB</span><span class="sxs-lookup"><span data-stu-id="35a04-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="35a04-185">Pillanatkép-kapacitás: 10 TB</span><span class="sxs-lookup"><span data-stu-id="35a04-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="35a04-186">Legfeljebb 50 Gigabit / másodperc, a bejövő + kimenő</span><span class="sxs-lookup"><span data-stu-id="35a04-186">Up to 50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="35a04-187">A prémium szintű Storage specifikációk további információkért tekintse meg [méretezhetőséget és a prémium szintű Storage használatakor Performance Targets](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="35a04-187">For the more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="35a04-188">Lemez gyorsítótárazási házirend</span><span class="sxs-lookup"><span data-stu-id="35a04-188">Disk caching policy</span></span>
<span data-ttu-id="35a04-189">Alapértelmezés szerint a gyorsítótárazási házirend lemez van *csak olvasható* prémium adatok lemezein, és *írható-olvasható* az a prémium szintű operációsrendszer-lemez csatolva a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="35a04-189">By default, disk caching policy is *Read-Only* for all the Premium data disks, and *Read-Write* for the Premium operating system disk attached to the VM.</span></span> <span data-ttu-id="35a04-190">A konfigurációs beállítás ajánlott az alkalmazás IOs rendszerhez az optimális teljesítmény eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-190">This configuration setting is recommended to achieve the optimal performance for your application's IOs.</span></span> <span data-ttu-id="35a04-191">Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.</span><span class="sxs-lookup"><span data-stu-id="35a04-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="35a04-192">A meglévő adatlemezek gyorsítótár beállításait is frissítve [Azure Portal](https://portal.azure.com) vagy a *- HostCaching* paramétere a *Set-AzureDataDisk* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="35a04-192">The cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or the *-HostCaching* parameter of the *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="35a04-193">Hely</span><span class="sxs-lookup"><span data-stu-id="35a04-193">Location</span></span>
<span data-ttu-id="35a04-194">Jelölje ki a helyet, ahol a prémium szintű Azure Storage áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="35a04-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="35a04-195">Lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="35a04-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="35a04-196">A virtuális gépeket, hogy a lemezek, a virtuális gép lesz áruházak sok különböző régiókban azok jobb teljesítményt a tárfiók ugyanabban a régióban található.</span><span class="sxs-lookup"><span data-stu-id="35a04-196">VMs located in the same region as the Storage account that stores the disks for the VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="35a04-197">Egyéb Azure virtuális gép konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="35a04-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="35a04-198">Egy Azure virtuális gép létrehozásakor a rendszer kéri, hogy az egyes virtuális gép beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="35a04-198">When creating an Azure VM, you will be asked to configure certain VM settings.</span></span> <span data-ttu-id="35a04-199">Ne feledje, hogy néhány beállításainak rögzítése élettartama idején a virtuális gép, amíg módosíthatja, vagy később fel más.</span><span class="sxs-lookup"><span data-stu-id="35a04-199">Remember, few settings are fixed for the lifetime of the VM, while you can modify or add others later.</span></span> <span data-ttu-id="35a04-200">Tekintse át a Azure virtuális gép konfigurációs beállítások, és győződjön meg arról, hogy ezeknek a konfigurációja megfelelő a munkaterhelési követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="35a04-200">Review these Azure VM configuration settings and make sure that these are configured appropriately to match your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="35a04-201">Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="35a04-201">Optimization</span></span>
<span data-ttu-id="35a04-202">[Prémium szintű Storage: Nagy teljesítményű kialakítása](storage-premium-storage-performance.md) útmutatást nyújt a prémium szintű Azure Storage használatával nagy teljesítményű alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="35a04-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="35a04-203">Követheti, hogy az irányelveket, az alkalmazás által használt technológiák alkalmazandó ajánlott eljárások teljesítményének együtt.</span><span class="sxs-lookup"><span data-stu-id="35a04-203">You can follow the guidelines combined with performance best practices applicable to technologies used by your application.</span></span>

## <span data-ttu-id="35a04-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Készítse elő, és másolja át a virtuális merevlemezek (VHD) prémium szintű Storage</span><span class="sxs-lookup"><span data-stu-id="35a04-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) to Premium Storage</span></span>
<span data-ttu-id="35a04-205">A következő szakaszban talál útmutatást a virtuális merevlemezek a virtuális gép előkészítése és a VHD-k másolása az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="35a04-205">The following section provides guidelines for preparing VHDs from your VM and copying VHDs to Azure Storage.</span></span>

* [<span data-ttu-id="35a04-206">1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek a prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="35a04-206">Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="35a04-207">2. forgatókönyv: "I vagyok telepít át virtuális gépeket más platformokon a prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="35a04-207">Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="35a04-208">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35a04-208">Prerequisites</span></span>
<span data-ttu-id="35a04-209">A virtuális merevlemezeket az áttelepítés előkészítéséhez lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="35a04-209">To prepare the VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="35a04-210">Azure-előfizetéssel, egy tárfiókot, és egy tároló az adott storage-fiókot, amelyhez a VHD-t is másolhatja.</span><span class="sxs-lookup"><span data-stu-id="35a04-210">An Azure subscription, a storage account, and a container in that storage account to which you can copy your VHD.</span></span> <span data-ttu-id="35a04-211">Vegye figyelembe, hogy a cél tárfiókkal lehet-e a Standard vagy prémium szintű Storage fiók igényektől függően.</span><span class="sxs-lookup"><span data-stu-id="35a04-211">Note that the destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="35a04-212">Olyan eszköz, amely a virtuális merevlemez generalize, ha azt tervezi, hogy több Virtuálisgép-példányok készíteni.</span><span class="sxs-lookup"><span data-stu-id="35a04-212">A tool to generalize the VHD if you plan to create multiple VM instances from it.</span></span> <span data-ttu-id="35a04-213">Például a Windows vagy adatb-sysprep Ubuntu a sysprep.</span><span class="sxs-lookup"><span data-stu-id="35a04-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="35a04-214">Olyan eszköz, amely a VHD-fájl feltöltése a tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="35a04-214">A tool to upload the VHD file to the Storage account.</span></span> <span data-ttu-id="35a04-215">Lásd: [adatátvitel az AzCopy parancssori segédprogram a](storage-use-azcopy.md) , vagy használjon egy [Azure Tártallózó](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="35a04-215">See [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="35a04-216">Ez az útmutató ismerteti, másolja a VHD-t az AzCopy eszközzel.</span><span class="sxs-lookup"><span data-stu-id="35a04-216">This guide describes copying your VHD using the AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-217">Ha úgy dönt, a szinkron másolatot beállítást az AzCopy, az optimális teljesítmény érdekében másolja a VHD-való futtatásával az eszközöket egy Azure virtuális Gépen, amely a cél tárfiókkal ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="35a04-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in the same region as the destination storage account.</span></span> <span data-ttu-id="35a04-218">Virtuális merevlemez másolása az Azure virtuális gép egy másik régióban található, a teljesítmény csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="35a04-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="35a04-219">Nagy mennyiségű adatot felülírását korlátozott sávszélességű, fontolja meg [az Azure Import/Export szolgáltatás használatával az adatok átviteléhez a Blob Storage](../storage-import-export-service.md); Ez lehetővé teszi, hogy az adatok átvitele a merevlemez egy Azure-adatközpontban szállítási.</span><span class="sxs-lookup"><span data-stu-id="35a04-219">For copying a large amount of data over limited bandwidth, consider [using the Azure Import/Export service to transfer data to Blob Storage](../storage-import-export-service.md); this allows you to transfer your data by shipping hard disk drives to an Azure datacenter.</span></span> <span data-ttu-id="35a04-220">Az Azure Import/Export szolgáltatás segítségével másolja az adatokat a csak egy standard szintű tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="35a04-220">You can use the Azure Import/Export service to copy data to a standard storage account only.</span></span> <span data-ttu-id="35a04-221">Ha az adatokat a standard szintű tárfiókot, használhatja a [másolási Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) vagy az AzCopy segítségével az adatok átvitele a prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="35a04-221">Once the data is in your standard storage account, you can use either the [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy to transfer the data to your premium storage account.</span></span>
>
> <span data-ttu-id="35a04-222">Ügyeljen arra, hogy a Microsoft Azure csak támogatja-e a rögzített méretű VHD-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="35a04-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="35a04-223">A VHDX-fájlok vagy a dinamikus VHD-k nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="35a04-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="35a04-224">Ha egy dinamikus virtuális merevlemez, átalakíthatja rögzített méretű használata a [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="35a04-224">If you have a dynamic VHD, you can convert it to fixed size using the [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="35a04-225"><a name="scenario1"></a>1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek a prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="35a04-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs to Azure Premium Storage."</span></span>
<span data-ttu-id="35a04-226">Meglévő Azure virtuális gépeket telepít át, ha a virtuális gép leállítása, készítse elő a / kívánt VHD típusú virtuális merevlemezeket, és másolja a VHD AzCopy vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="35a04-226">If you are migrating existing Azure VMs, stop the VM, prepare VHDs per the type of VHD you want, and then copy the VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="35a04-227">A virtuális gép kell lennie a tiszta állapotot áttelepítéséhez teljesen le.</span><span class="sxs-lookup"><span data-stu-id="35a04-227">The VM needs to be completely down to migrate a clean state.</span></span> <span data-ttu-id="35a04-228">Lesz a leállás addig, amíg az áttelepítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="35a04-228">There will be a downtime until the migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="35a04-229">1. lépés</span><span class="sxs-lookup"><span data-stu-id="35a04-229">Step 1.</span></span> <span data-ttu-id="35a04-230">Virtuális merevlemezek Felkészülés az áttelepítésre</span><span class="sxs-lookup"><span data-stu-id="35a04-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="35a04-231">Ha a meglévő Azure virtuális gépek áttelepítés prémium szintű Storage, a virtuális merevlemez lehet:</span><span class="sxs-lookup"><span data-stu-id="35a04-231">If you are migrating existing Azure VMs to Premium Storage, your VHD may be:</span></span>

* <span data-ttu-id="35a04-232">Általános operációsrendszer-lemezkép elkészítése</span><span class="sxs-lookup"><span data-stu-id="35a04-232">A generalized operating system image</span></span>
* <span data-ttu-id="35a04-233">Egy egyedi operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="35a04-233">A unique operating system disk</span></span>
* <span data-ttu-id="35a04-234">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="35a04-234">A data disk</span></span>

<span data-ttu-id="35a04-235">Az alábbiakban azt végezze el a virtuális merevlemez előkészítése 3 forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="35a04-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a><span data-ttu-id="35a04-236">Több Virtuálisgép-példány létrehozásához használja az operációs rendszer általánosított virtuális Merevlemezt</span><span class="sxs-lookup"><span data-stu-id="35a04-236">Use a generalized Operating System VHD to create multiple VM instances</span></span>
<span data-ttu-id="35a04-237">Ha több általános Azure Virtuálisgép-példány létrehozásához használt virtuális Merevlemezt, először meg kell generalize virtuális Merevlemezt a sysprep segédprogrammal.</span><span class="sxs-lookup"><span data-stu-id="35a04-237">If you are uploading a VHD that will be used to create multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="35a04-238">Ez vonatkozik, amely a helyi virtuális merevlemez vagy a felhőben.</span><span class="sxs-lookup"><span data-stu-id="35a04-238">This applies to a VHD that is on-premises or in the cloud.</span></span> <span data-ttu-id="35a04-239">Sysprep eltávolítása a VHD-t bármely gépen-specifikus adatait.</span><span class="sxs-lookup"><span data-stu-id="35a04-239">Sysprep removes any machine-specific information from the VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="35a04-240">Készítsen pillanatképet, vagy biztonsági mentése a virtuális gép előtt normalizálása azt.</span><span class="sxs-lookup"><span data-stu-id="35a04-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="35a04-241">A sysprep fut le fog állni, és a Virtuálisgép-példány felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="35a04-241">Running sysprep will stop and deallocate the VM instance.</span></span> <span data-ttu-id="35a04-242">A Windows operációs rendszer virtuális Merevlemezt a sysprep kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="35a04-242">Follow steps below to sysprep a Windows OS VHD.</span></span> <span data-ttu-id="35a04-243">Vegye figyelembe, hogy a Sysprep parancsot futtató van szükség, hogy a virtuális gép leállítása.</span><span class="sxs-lookup"><span data-stu-id="35a04-243">Note that running the Sysprep command will require you to shut down the virtual machine.</span></span> <span data-ttu-id="35a04-244">További információ a Sysprep: [Sysprep áttekintése](http://technet.microsoft.com/library/hh825209.aspx) vagy [Sysprep műszaki útmutatója](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="35a04-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="35a04-245">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="35a04-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="35a04-246">Adja meg a következő parancs futtatásával nyissa meg a Sysprep:</span><span class="sxs-lookup"><span data-stu-id="35a04-246">Enter the following command to open Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="35a04-247">A rendszer-előkészítő eszközt, jelölje be adja meg a rendszer Out-of-Box élmény (OOBE), jelölje be a Generalize jelölőnégyzetet, válassza ki **leállítási**, és kattintson a **OK**, az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="35a04-247">In the System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select the Generalize check box, select **Shutdown**, and then click **OK**, as shown in the image below.</span></span> <span data-ttu-id="35a04-248">A Sysprep rendszer általánosítja az operációs rendszert, és állítsa le a rendszer.</span><span class="sxs-lookup"><span data-stu-id="35a04-248">Sysprep will generalize the operating system and shut down the system.</span></span>

    ![][1]

<span data-ttu-id="35a04-249">Ubuntu virtuális gép használja a sysprep adatb azonos eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-249">For an Ubuntu VM, use virt-sysprep to achieve the same.</span></span> <span data-ttu-id="35a04-250">Lásd: [adatb-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="35a04-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="35a04-251">További információ a nyílt forráskódú némelyike [Linux Server kiépítés szoftver](http://www.cyberciti.biz/tips/server-provisioning-software.html) más Linux operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-251">See also some of the open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a><span data-ttu-id="35a04-252">Egyetlen Virtuálisgép-példány létrehozásához használja az operációs rendszer virtuális merevlemez egyedi</span><span class="sxs-lookup"><span data-stu-id="35a04-252">Use a unique Operating System VHD to create a single VM instance</span></span>
<span data-ttu-id="35a04-253">Ha a virtuális gép adatokat igénylő futó alkalmazást, nem generalize a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="35a04-253">If you have an application running on the VM which requires the machine specific data, do not generalize the VHD.</span></span> <span data-ttu-id="35a04-254">Nem általánosított virtuális merevlemez segítségével hozzon létre egyedi Azure Virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="35a04-254">A non-generalized VHD can be used to create a unique Azure VM instance.</span></span> <span data-ttu-id="35a04-255">Például ha a tartományvezérlő van a VHD-t, sysprep végrehajtása megkönnyítő hatástalan tartományvezérlőként.</span><span class="sxs-lookup"><span data-stu-id="35a04-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="35a04-256">Tekintse át a virtuális Gépet, és azokat a sysprep futtatása előtt a virtuális merevlemez normalizálása hatásának futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="35a04-256">Review the applications running on your VM and the impact of running sysprep on them before generalizing the VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="35a04-257">Virtuális merevlemez adatlemeze regisztrálása</span><span class="sxs-lookup"><span data-stu-id="35a04-257">Register data disk VHD</span></span>
<span data-ttu-id="35a04-258">Ha adatlemezt kell áttelepíteni az Azure-ban, meg kell győződnie arról az adott adatok lemezeket használó virtuális gépek állnak le.</span><span class="sxs-lookup"><span data-stu-id="35a04-258">If you have data disks in Azure to be migrated, you must make sure the VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="35a04-259">Prémium szintű Azure Storage másolja a VHD-t, és regisztrálhatja azt kiosztott adatok lemezként az alább ismertetett lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="35a04-259">Follow the steps described below to copy VHD to Azure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="35a04-260">2. lépés</span><span class="sxs-lookup"><span data-stu-id="35a04-260">Step 2.</span></span> <span data-ttu-id="35a04-261">A cél a virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-261">Create the destination for your VHD</span></span>
<span data-ttu-id="35a04-262">Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="35a04-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="35a04-263">A virtuális merevlemezek tárolási helyének megtervezésekor, vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="35a04-263">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="35a04-264">A célként prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="35a04-264">The target Premium storage account.</span></span>
* <span data-ttu-id="35a04-265">A tárfiók helyének meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat utolsó szakaszában.</span><span class="sxs-lookup"><span data-stu-id="35a04-265">The storage account location must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="35a04-266">Nem átmásolni egy új tárfiókot, vagy a igények alapján ugyanazt a tárfiókot használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="35a04-266">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="35a04-267">Másolja ki és mentse a tárfiók hívóbetűjét, a cél tárfiókkal a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="35a04-267">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="35a04-268">Az adatlemezek esetén dönthet úgy, hogy néhány adatlemezek tartsa egy standard szintű tárfiókot (például lemezek hűtőre tárhellyel rendelkező), de határozottan javasoljuk, hogy az üzemi alkalmazások és szolgáltatások a prémium szintű storage minden adat áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="35a04-268">For data disks, you can choose to keep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <span data-ttu-id="35a04-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>3. lépés.</span><span class="sxs-lookup"><span data-stu-id="35a04-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="35a04-270">Másolja a VHD AzCopy vagy a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="35a04-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="35a04-271">Szüksége lesz a tároló elérési útját és a tárolási fiók kulcs található feldolgozni az alábbi két lehetőség közül.</span><span class="sxs-lookup"><span data-stu-id="35a04-271">You will need to find your container path and storage account key to process either of these two options.</span></span> <span data-ttu-id="35a04-272">Tároló elérési útja és a tároló kulcsa megtalálható **Azure Portal** > **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="35a04-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="35a04-273">A tároló URL-címe például a "https://myaccount.blob.core.windows.net/mycontainer/" lesz.</span><span class="sxs-lookup"><span data-stu-id="35a04-273">The container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="35a04-274">1. lehetőség: Az AzCopy (aszinkron másolhatja azokat) egy virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="35a04-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="35a04-275">Használja az AzCopy, egyszerűen feltöltheti a VHD-t az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="35a04-275">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="35a04-276">A virtuális merevlemezek méretétől függően ez időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="35a04-276">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="35a04-277">Fontos, hogy ellenőrizze a tárfiókok be-és kilépési korlátai, ez a beállítás használata esetén.</span><span class="sxs-lookup"><span data-stu-id="35a04-277">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="35a04-278">Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="35a04-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="35a04-279">Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="35a04-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="35a04-280">Nyissa meg az Azure PowerShell és a mappában, amelyen telepítve van-e az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="35a04-280">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="35a04-281">A következő parancs segítségével másolja a VHD-fájlt a "Forrás" a "Cél".</span><span class="sxs-lookup"><span data-stu-id="35a04-281">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="35a04-282">Példa:</span><span class="sxs-lookup"><span data-stu-id="35a04-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="35a04-283">Az alábbiakban az AzCopy parancs paraméterei leírása:</span><span class="sxs-lookup"><span data-stu-id="35a04-283">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="35a04-284">**/ Forrás:  *&lt;forrás&gt;:***  a mappa vagy a tárolási tároló URL-címet, amely a VHD-t tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="35a04-284">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="35a04-285">**/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  a forrás tárfiók Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="35a04-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="35a04-286">**/ Dest:  *&lt;cél&gt;:***  másolja a VHD-fájlt tároló tároló URL-címet.</span><span class="sxs-lookup"><span data-stu-id="35a04-286">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="35a04-287">**/ DestKey:  *&lt;cél fiókkulcs&gt;:***  a cél tárfiókkal a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="35a04-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="35a04-288">**/ Mintát:  *&lt;Fájlnév&gt;:***  adja meg a fájlnevet a virtuális merevlemez másolása.</span><span class="sxs-lookup"><span data-stu-id="35a04-288">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="35a04-289">AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram a](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-289">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="35a04-290">2. lehetőség: A PowerShell használatával (Synchronized másolás) virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="35a04-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="35a04-291">A PowerShell-parancsmaggal a Start-AzureStorageBlobCopy VHD-fájlt is másolhatja.</span><span class="sxs-lookup"><span data-stu-id="35a04-291">You can also copy the VHD file using the PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="35a04-292">Az Azure PowerShell az alábbi parancs segítségével másolja a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="35a04-292">Use the following command on Azure PowerShell to copy VHD.</span></span> <span data-ttu-id="35a04-293">Cserélje le a <> értékei a forrás és cél tárfiók megfelelő értékeivel.</span><span class="sxs-lookup"><span data-stu-id="35a04-293">Replace the values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="35a04-294">Ezen parancs használatához rendelkeznie kell a cél tárfiókkal nevezett VHD-k tárolója.</span><span class="sxs-lookup"><span data-stu-id="35a04-294">To use this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="35a04-295">Ha a tároló nem létezik, hozzon létre egyet a parancs futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="35a04-295">If the container doesn't exist, create one before running the command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="35a04-296">Példa:</span><span class="sxs-lookup"><span data-stu-id="35a04-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="35a04-297"><a name="scenario2"></a>2. forgatókönyv: "I vagyok telepít át virtuális gépeket más platformokon a prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="35a04-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms to Azure Premium Storage."</span></span>
<span data-ttu-id="35a04-298">Ha az áttelepítés VHD-t a nem - Azure felhőalapú tárolást az Azure-ba, először exportálnia kell a virtuális merevlemez helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="35a04-298">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="35a04-299">A teljes forrás könyvtár elérési útja a helyi virtuális merevlemez tároló lesz szüksége van, és az AzCopy segítségével töltse fel az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="35a04-299">Have the complete source path of the local directory where VHD is stored handy, and then using AzCopy to upload it to Azure Storage.</span></span>

#### <a name="step-1-export-vhd-to-a-local-directory"></a><span data-ttu-id="35a04-300">1. lépés</span><span class="sxs-lookup"><span data-stu-id="35a04-300">Step 1.</span></span> <span data-ttu-id="35a04-301">A VHD exportálása egy helyi könyvtárba</span><span class="sxs-lookup"><span data-stu-id="35a04-301">Export VHD to a local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="35a04-302">Másolja a VHD-t AWS</span><span class="sxs-lookup"><span data-stu-id="35a04-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="35a04-303">Ha AWS használ, exportálja a EC2 példány az Amazon S3 gyűjtő virtuális.</span><span class="sxs-lookup"><span data-stu-id="35a04-303">If you are using AWS, export the EC2 instance to a VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="35a04-304">Az Amazon EC2 példányok telepítse az Amazon EC2 parancssori felület (CLI) eszközt, és a létrehozás-példány-export-tevékenység parancsot a EC2 példány exportálni egy VHD-fájl exportálása Amazon dokumentációjában ismertetett lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="35a04-304">Follow the steps described in the Amazon documentation for Exporting Amazon EC2 Instances to install the Amazon EC2 command-line interface (CLI) tool and run the create-instance-export-task command to export the EC2 instance to a VHD file.</span></span> <span data-ttu-id="35a04-305">Használjon **VHD** a lemez &#95; kép &#95; FORMÁTUM változó futtatásakor a **-példány-export-feladat létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="35a04-305">Be sure to use **VHD** for the DISK&#95;IMAGE&#95;FORMAT variable when running the **create-instance-export-task** command.</span></span> <span data-ttu-id="35a04-306">Az exportált VHD-fájl kerül az Amazon S3 gyűjtő jelöl ki, a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="35a04-306">The exported VHD file is saved in the Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="35a04-307">Töltse le a VHD-fájlt a S3 gyűjtő.</span><span class="sxs-lookup"><span data-stu-id="35a04-307">Download the VHD file from the S3 bucket.</span></span> <span data-ttu-id="35a04-308">Válassza ki a VHD-fájlt, majd **műveletek** > **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="35a04-308">Select the VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="35a04-309">Másolja a VHD-t más-Azure felhő</span><span class="sxs-lookup"><span data-stu-id="35a04-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="35a04-310">Ha az áttelepítés VHD-t a nem - Azure felhőalapú tárolást az Azure-ba, először exportálnia kell a virtuális merevlemez helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="35a04-310">If you are migrating VHD from non-Azure Cloud Storage to Azure, you must first export the VHD to a local directory.</span></span> <span data-ttu-id="35a04-311">Másolja a teljes forrás könyvtár elérési útja a helyi virtuális merevlemez tárolásához.</span><span class="sxs-lookup"><span data-stu-id="35a04-311">Copy the complete source path of the local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="35a04-312">Másolja a VHD-t a helyszíni</span><span class="sxs-lookup"><span data-stu-id="35a04-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="35a04-313">Ha a VHD-t a helyszíni környezetben telepít, szüksége lesz a virtuális merevlemez tárolásához, a teljes forrás elérési útja.</span><span class="sxs-lookup"><span data-stu-id="35a04-313">If you are migrating VHD from an on-premises environment, you will need the complete source path where VHD is stored.</span></span> <span data-ttu-id="35a04-314">A forrás elérési útja egy helyen vagy kiszolgálómegosztás lehet.</span><span class="sxs-lookup"><span data-stu-id="35a04-314">The source path could be a server location or file share.</span></span>

#### <a name="step-2-create-the-destination-for-your-vhd"></a><span data-ttu-id="35a04-315">2. lépés</span><span class="sxs-lookup"><span data-stu-id="35a04-315">Step 2.</span></span> <span data-ttu-id="35a04-316">A cél a virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-316">Create the destination for your VHD</span></span>
<span data-ttu-id="35a04-317">Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="35a04-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="35a04-318">A virtuális merevlemezek tárolási helyének megtervezésekor, vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="35a04-318">Consider the following points when planning where to store your VHDs:</span></span>

* <span data-ttu-id="35a04-319">A céloldali tárfiók lehet standard vagy prémium szintű storage, attól függően, hogy az alkalmazás követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="35a04-319">The target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="35a04-320">A tárfiók régiója meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat utolsó szakaszában.</span><span class="sxs-lookup"><span data-stu-id="35a04-320">The storage account region must be same as Premium Storage capable Azure VMs you will create in the final stage.</span></span> <span data-ttu-id="35a04-321">Nem átmásolni egy új tárfiókot, vagy a igények alapján ugyanazt a tárfiókot használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="35a04-321">You could copy to a new storage account, or plan to use the same storage account based on your needs.</span></span>
* <span data-ttu-id="35a04-322">Másolja ki és mentse a tárfiók hívóbetűjét, a cél tárfiókkal a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="35a04-322">Copy and save the storage account key of the destination storage account for the next stage.</span></span>

<span data-ttu-id="35a04-323">Határozottan javasoljuk, a prémium szintű storage üzemi terhelés minden adat áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="35a04-323">We strongly recommend you moving all data for production workload to use premium storage.</span></span>

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a><span data-ttu-id="35a04-324">3. lépés</span><span class="sxs-lookup"><span data-stu-id="35a04-324">Step 3.</span></span> <span data-ttu-id="35a04-325">A VHD-fájlt feltölti az Azure Storage</span><span class="sxs-lookup"><span data-stu-id="35a04-325">Upload the VHD to Azure Storage</span></span>
<span data-ttu-id="35a04-326">Most, hogy a VHD-t a helyi címtárban, AzCopy vagy AzurePowerShell használhatja a .vhd fájl feltöltése az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="35a04-326">Now that you have your VHD in the local directory, you can use AzCopy or AzurePowerShell to upload the .vhd file to Azure Storage.</span></span> <span data-ttu-id="35a04-327">Mindkét lehetőség itt találhatók:</span><span class="sxs-lookup"><span data-stu-id="35a04-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a><span data-ttu-id="35a04-328">1. lehetőség: Azure PowerShell Add-AzureVhd fel kell töltenie a .vhd fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="35a04-328">Option 1: Using Azure PowerShell Add-AzureVhd to upload the .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="35a04-329">Példa <Uri> előfordulhat, hogy ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="35a04-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="35a04-330">Példa <FileInfo> előfordulhat, hogy ***"C:\path\to\upload.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="35a04-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a><span data-ttu-id="35a04-331">2. lehetőség: Az AzCopy segítségével a .vhd fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="35a04-331">Option 2: Using AzCopy to upload the .vhd file</span></span>
<span data-ttu-id="35a04-332">Használja az AzCopy, egyszerűen feltöltheti a VHD-t az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="35a04-332">Using AzCopy, you can easily upload the VHD over the Internet.</span></span> <span data-ttu-id="35a04-333">A virtuális merevlemezek méretétől függően ez időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="35a04-333">Depending on the size of the VHDs, this may take time.</span></span> <span data-ttu-id="35a04-334">Fontos, hogy ellenőrizze a tárfiókok be-és kilépési korlátai, ez a beállítás használata esetén.</span><span class="sxs-lookup"><span data-stu-id="35a04-334">Remember to check the storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="35a04-335">Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="35a04-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="35a04-336">Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="35a04-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="35a04-337">Nyissa meg az Azure PowerShell és a mappában, amelyen telepítve van-e az AzCopy.</span><span class="sxs-lookup"><span data-stu-id="35a04-337">Open Azure PowerShell and go to the folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="35a04-338">A következő parancs segítségével másolja a VHD-fájlt a "Forrás" a "Cél".</span><span class="sxs-lookup"><span data-stu-id="35a04-338">Use the following command to copy the VHD file from "Source" to "Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="35a04-339">Példa:</span><span class="sxs-lookup"><span data-stu-id="35a04-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="35a04-340">Az alábbiakban az AzCopy parancs paraméterei leírása:</span><span class="sxs-lookup"><span data-stu-id="35a04-340">Here are descriptions of the parameters used in the AzCopy command:</span></span>

   * <span data-ttu-id="35a04-341">**/ Forrás:  *&lt;forrás&gt;:***  a mappa vagy a tárolási tároló URL-címet, amely a VHD-t tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="35a04-341">**/Source: *&lt;source&gt;:*** Location of the folder or storage container URL that contains the VHD.</span></span>
   * <span data-ttu-id="35a04-342">**/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  a forrás tárfiók Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="35a04-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of the source storage account.</span></span>
   * <span data-ttu-id="35a04-343">**/ Dest:  *&lt;cél&gt;:***  másolja a VHD-fájlt tároló tároló URL-címet.</span><span class="sxs-lookup"><span data-stu-id="35a04-343">**/Dest: *&lt;destination&gt;:*** Storage container URL to copy the VHD to.</span></span>
   * <span data-ttu-id="35a04-344">**/ DestKey:  *&lt;cél fiókkulcs&gt;:***  a cél tárfiókkal a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="35a04-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of the destination storage account.</span></span>
   * <span data-ttu-id="35a04-345">**/ BlobType: lap:** Megadja, hogy a cél oldalakra vonatkozó blob.</span><span class="sxs-lookup"><span data-stu-id="35a04-345">**/BlobType: page:** Specifies that the destination is a page blob.</span></span>
   * <span data-ttu-id="35a04-346">**/ Mintát:  *&lt;Fájlnév&gt;:***  adja meg a fájlnevet a virtuális merevlemez másolása.</span><span class="sxs-lookup"><span data-stu-id="35a04-346">**/Pattern: *&lt;file-name&gt;:*** Specify the file name of the VHD to copy.</span></span>

<span data-ttu-id="35a04-347">AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram a](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="35a04-347">For details on using AzCopy tool, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="35a04-348">Más beállításokat a virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="35a04-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="35a04-349">Feltöltheti a virtuális merevlemez a tárfiókhoz, az alábbi eszközök egyikével:</span><span class="sxs-lookup"><span data-stu-id="35a04-349">You can also upload a VHD to your storage account using one of the following means:</span></span>

* [<span data-ttu-id="35a04-350">Az Azure Storage másolási Blob API</span><span class="sxs-lookup"><span data-stu-id="35a04-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="35a04-351">Az Azure Storage Explorer feltöltése a BLOB</span><span class="sxs-lookup"><span data-stu-id="35a04-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="35a04-352">Storage Import/Export szolgáltatás REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="35a04-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="35a04-353">Ajánlott Import/Export szolgáltatás használata, ha becsült a 7 napnál hosszabb idő feltöltése.</span><span class="sxs-lookup"><span data-stu-id="35a04-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="35a04-354">Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) becsléséhez adatok méretét és átviteli egység időpontját.</span><span class="sxs-lookup"><span data-stu-id="35a04-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) to estimate the time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="35a04-355">Importálási/exportálási segítségével másolja egy standard szintű tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="35a04-355">Import/Export can be used to copy to a standard storage account.</span></span> <span data-ttu-id="35a04-356">Szüksége lesz a prémium szintű storage-fiókra egy eszköz, például az AzCopy standard tárolási másolja.</span><span class="sxs-lookup"><span data-stu-id="35a04-356">You will need to copy from standard storage to premium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="35a04-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Prémium szintű Storage használata Azure virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="35a04-358">Után a virtuális merevlemez töltenek fel vagy másolja a kívánt tárfiókot, kövesse az ebben a szakaszban található a virtuális merevlemez regisztrálja az operációsrendszer-lemezképek, vagy a forgatókönyvtől függően az operációsrendszer-lemez, és a Virtuálisgép-példány készíteni.</span><span class="sxs-lookup"><span data-stu-id="35a04-358">After the VHD is uploaded or copied to the desired storage account, follow the instructions in this section to register the VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="35a04-359">A virtuális merevlemez adatlemeze csatolható a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="35a04-359">The data disk VHD can be attached to the VM once it is created.</span></span>
<span data-ttu-id="35a04-360">Ez a szakasz végén egy áttelepítési parancsfájlt valósul meg.</span><span class="sxs-lookup"><span data-stu-id="35a04-360">A sample migration script is provided at the end of this section.</span></span> <span data-ttu-id="35a04-361">Ez egyszerű parancsprogram nem felel meg minden forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="35a04-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="35a04-362">A parancsfájl az adott helyzetnek megfelelő frissítésére lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="35a04-362">You may need to update the script to match with your specific scenario.</span></span> <span data-ttu-id="35a04-363">Ha ezt a parancsfájlt a forgatókönyv vonatkozik-e, olvassa el alább [A Parancsfájlpéldát áttelepítési](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="35a04-363">To see if this script applies to your scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="35a04-364">Feladatlista</span><span class="sxs-lookup"><span data-stu-id="35a04-364">Checklist</span></span>
1. <span data-ttu-id="35a04-365">Várja meg, amíg minden, a Másolás VHD lemezek befejeződött.</span><span class="sxs-lookup"><span data-stu-id="35a04-365">Wait until all the VHD disks copying is complete.</span></span>
2. <span data-ttu-id="35a04-366">Ellenőrizze, hogy prémium szintű Storage érhető el a régióban végzi az áttelepítést.</span><span class="sxs-lookup"><span data-stu-id="35a04-366">Make sure Premium Storage is available in the region you are migrating to.</span></span>
3. <span data-ttu-id="35a04-367">Döntse el, az új Virtuálisgép-sorozat fog használni.</span><span class="sxs-lookup"><span data-stu-id="35a04-367">Decide the new VM series you will be using.</span></span> <span data-ttu-id="35a04-368">A prémium szintű Storage képes legyen, és a méret kell a rendelkezésre állási régióban függően előfordulhat, és igényei szerint.</span><span class="sxs-lookup"><span data-stu-id="35a04-368">It should be a Premium Storage capable, and the size should be depending on the availability in the region and based on your needs.</span></span>
4. <span data-ttu-id="35a04-369">Döntse el, a pontos használandó Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="35a04-369">Decide the exact VM size you will use.</span></span> <span data-ttu-id="35a04-370">Virtuálisgép-méretet kell lennie, elég nagy legyen rendelkezik adatlemezek számának támogatásához.</span><span class="sxs-lookup"><span data-stu-id="35a04-370">VM size needs to be large enough to support the number of data disks you have.</span></span> <span data-ttu-id="35a04-371">Például</span><span class="sxs-lookup"><span data-stu-id="35a04-371">E.g.</span></span> <span data-ttu-id="35a04-372">Ha 4 adatlemezek, a virtuális gép 2 vagy több maggal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="35a04-372">if you have 4 data disks, the VM must have 2 or more cores.</span></span> <span data-ttu-id="35a04-373">Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="35a04-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="35a04-374">Prémium szintű Storage-fiók létrehozása a cél régióban.</span><span class="sxs-lookup"><span data-stu-id="35a04-374">Create a Premium Storage account in the target region.</span></span> <span data-ttu-id="35a04-375">Ez az a fiók, az új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="35a04-375">This is the account you will use for the new VM.</span></span>
6. <span data-ttu-id="35a04-376">Az aktuális virtuális gép adatai lesz szüksége, beleértve a megfelelő VHD-blobok és lemezek listáját rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="35a04-376">Have the current VM details handy, including the list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="35a04-377">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="35a04-377">Prepare your application for downtime.</span></span> <span data-ttu-id="35a04-378">Egy tiszta az áttelepítés végrehajtásához, akkor állítsa le a feldolgozás az aktuális rendszerben.</span><span class="sxs-lookup"><span data-stu-id="35a04-378">To do a clean migration, you have to stop all the processing in the current system.</span></span> <span data-ttu-id="35a04-379">Csak ezután beszerezheti a konzisztens állapotú. Ez az új platformon is áttelepíthetők.</span><span class="sxs-lookup"><span data-stu-id="35a04-379">Only then you can get it to consistent state which you can migrate to the new platform.</span></span> <span data-ttu-id="35a04-380">Állásidő időtartama áttelepítéséhez a lemezeken mennyiségétől függ.</span><span class="sxs-lookup"><span data-stu-id="35a04-380">Downtime duration will depend on the amount of data in the disks to migrate.</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-381">Az Azure Resource Manager virtuális gép speciális VHD lemez létrehozásakor, tekintse meg [sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) erőforrás-kezelő virtuális gépet a meglévő lemezt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="35a04-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer to [this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="35a04-382">A virtuális merevlemez regisztrálása</span><span class="sxs-lookup"><span data-stu-id="35a04-382">Register your VHD</span></span>
<span data-ttu-id="35a04-383">A virtuális gép létrehozása az operációs rendszer virtuális merevlemezről vagy adatlemezt csatolni egy új virtuális Gépet, először regisztrálnia kell őket.</span><span class="sxs-lookup"><span data-stu-id="35a04-383">To create a VM from OS VHD or to attach a data disk to a new VM, you must first register them.</span></span> <span data-ttu-id="35a04-384">Kövesse az alábbi lépéseket attól függően, hogy a VHD-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="35a04-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="35a04-385">Operációs rendszer virtuális merevlemez létrehozása több Azure Virtuálisgép-példányok általánosítva</span><span class="sxs-lookup"><span data-stu-id="35a04-385">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="35a04-386">Miután általánosított virtuális Merevlemezt az operációsrendszer-lemezképek feltöltése a tárfiókba, regisztrálja azt egy **Azure Virtuálisgép-lemezkép** , hogy egy vagy több Virtuálisgép-példányok hozhat létre belőle.</span><span class="sxs-lookup"><span data-stu-id="35a04-386">After generalized OS image VHD is uploaded to the storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="35a04-387">A következő PowerShell-parancsmagok segítségével regisztrálja a virtuális merevlemez, egy Azure virtuális gép operációsrendszer-lemezképben.</span><span class="sxs-lookup"><span data-stu-id="35a04-387">Use the following PowerShell cmdlets to register your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="35a04-388">Adja meg a teljes tároló URL-cím, ahol VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="35a04-388">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="35a04-389">Másolja ki és mentse az új Azure Virtuálisgép-lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="35a04-389">Copy and save the name of this new Azure VM Image.</span></span> <span data-ttu-id="35a04-390">A fenti példa, hogy a rendszer *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="35a04-390">In the example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="35a04-391">Egyedi, operációs rendszer virtuális merevlemez egyetlen Azure Virtuálisgép-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-391">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="35a04-392">Az egyedi az operációs rendszer virtuális merevlemez feltöltése a tárfiókba, után regisztrálja azt egy **Azure operációsrendszer-lemez** , hogy egy Virtuálisgép-példányt hozhat létre belőle.</span><span class="sxs-lookup"><span data-stu-id="35a04-392">After the unique OS VHD is uploaded to the storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="35a04-393">PowerShell-parancsmagok segítségével regisztrálja a VHD-t Azure operációsrendszer-lemezként.</span><span class="sxs-lookup"><span data-stu-id="35a04-393">Use these PowerShell cmdlets to register your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="35a04-394">Adja meg a teljes tároló URL-cím, ahol VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="35a04-394">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="35a04-395">Másolja ki és mentse az új Azure operációsrendszer-lemez neve.</span><span class="sxs-lookup"><span data-stu-id="35a04-395">Copy and save the name of this new Azure OS Disk.</span></span> <span data-ttu-id="35a04-396">A fenti példa, hogy a rendszer *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="35a04-396">In the example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a><span data-ttu-id="35a04-397">Adatok lemez virtuális merevlemez csatolva legyen új Azure Virtuálisgép-példányokat</span><span class="sxs-lookup"><span data-stu-id="35a04-397">Data disk VHD to be attached to new Azure VM instance(s)</span></span>
<span data-ttu-id="35a04-398">Miután a virtuális merevlemez adatlemeze tárfiókkal töltheti fel, regisztrálja egy Azure-adatlemez, úgy, hogy az új DS-méretek, DSv2-méretek és GS adatsorozat Azure Virtuálisgép-példány csatolható.</span><span class="sxs-lookup"><span data-stu-id="35a04-398">After the data disk VHD is uploaded to storage account, register it as an Azure Data Disk so that it can be attached to your new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="35a04-399">PowerShell-parancsmagok segítségével regisztrálja a virtuális merevlemez egy Azure-adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="35a04-399">Use these PowerShell cmdlets to register your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="35a04-400">Adja meg a teljes tároló URL-cím, ahol VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="35a04-400">Provide the complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="35a04-401">Másolja ki és mentse az új Azure-adatlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="35a04-401">Copy and save the name of this new Azure Data Disk.</span></span> <span data-ttu-id="35a04-402">A fenti példa, hogy a rendszer *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="35a04-402">In the example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="35a04-403">Prémium szintű Storage képes a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="35a04-404">Egyszer operációsrendszer-lemezképet, vagy az operációsrendszer-lemez van regisztrálva, új DS-méretek, DSv2-sorozat vagy GS sorozatnak virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="35a04-404">Once the OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="35a04-405">Fogja használni az operációs rendszeri lemezkép vagy operációs rendszer Lemeznév regisztrált.</span><span class="sxs-lookup"><span data-stu-id="35a04-405">You will be using the operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="35a04-406">Válassza ki a virtuális gép a prémium szintű Storage rétegtől.</span><span class="sxs-lookup"><span data-stu-id="35a04-406">Select the VM type from the Premium Storage tier.</span></span> <span data-ttu-id="35a04-407">Az alábbi példában használjuk a *Standard_DS2* Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="35a04-407">In example below, we are using the *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-408">A lemez mérete győződjön meg arról, hogy megegyezzen a kapacitás és teljesítmény követelményeket, valamint az elérhető Azure lemezméret frissítése.</span><span class="sxs-lookup"><span data-stu-id="35a04-408">Update the disk size to make sure it matches your capacity and performance requirements and the available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="35a04-409">Kövesse az új virtuális gép létrehozása az alábbi lépésről lépésre PowerShell-parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="35a04-409">Follow the step by step PowerShell cmdlets below to create the new VM.</span></span> <span data-ttu-id="35a04-410">Első lépésként állítsa be a következő általános paramétereket:</span><span class="sxs-lookup"><span data-stu-id="35a04-410">First, set the common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="35a04-411">Először hozzon létre egy felhőalapú szolgáltatás, amelyben, amelyen az új virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="35a04-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="35a04-412">Ezt a forgatókönyvtől függően létrehozni az operációsrendszer-lemezképek vagy az operációs rendszer lemezének regisztrált az Azure Virtuálisgép-példány.</span><span class="sxs-lookup"><span data-stu-id="35a04-412">Next, depending on your scenario, create the Azure VM instance from either the OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a><span data-ttu-id="35a04-413">Operációs rendszer virtuális merevlemez létrehozása több Azure Virtuálisgép-példányok általánosítva</span><span class="sxs-lookup"><span data-stu-id="35a04-413">Generalized Operating System VHD to create multiple Azure VM instances</span></span>
<span data-ttu-id="35a04-414">Hozzon létre egy vagy több új DS adatsorozat Azure Virtuálisgép-példányára használatával a **Azure operációsrendszer-lemezképek** regisztrált.</span><span class="sxs-lookup"><span data-stu-id="35a04-414">Create the one or more new DS Series Azure VM instances using the **Azure OS Image** that you registered.</span></span> <span data-ttu-id="35a04-415">Adja meg a operációsrendszer-lemezképek nevét a Virtuálisgép-konfiguráció, amikor új virtuális gép létrehozása az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="35a04-415">Specify this OS Image name in the VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a><span data-ttu-id="35a04-416">Egyedi, operációs rendszer virtuális merevlemez egyetlen Azure Virtuálisgép-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="35a04-416">Unique Operating System VHD to create a single Azure VM instance</span></span>
<span data-ttu-id="35a04-417">Új létrehozása DS adatsorozat Azure virtuális gép példány a **Azure operációsrendszer-lemez** regisztrált.</span><span class="sxs-lookup"><span data-stu-id="35a04-417">Create a new DS series Azure VM instance using the **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="35a04-418">Adja meg az operációs rendszer lemezének neve a Virtuálisgép-konfiguráció, ha az új virtuális gép létrehozása az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="35a04-418">Specify this OS Disk name in the VM configuration when creating the new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="35a04-419">Adjon meg más Azure Virtuálisgép-adatok, például egy felhőalapú szolgáltatás, régió, tárfiókot, a rendelkezésre állási csoport és gyorsítótárazási házirend.</span><span class="sxs-lookup"><span data-stu-id="35a04-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="35a04-420">Vegye figyelembe, hogy a Virtuálisgép-példány csak futó operációs rendszer vagy az adatlemezek együtt, a kijelölt felhőalapú szolgáltatás, valamint régió és tárolási fiók kell lenniük és ugyanazon a helyen az alapul szolgáló virtuális merevlemezek lemezek.</span><span class="sxs-lookup"><span data-stu-id="35a04-420">Note that the VM instance must be co-located with associated operating system or data disks, so the selected cloud service, region and storage account must all be in the same location as the underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="35a04-421">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="35a04-421">Attach data disk</span></span>
<span data-ttu-id="35a04-422">Végül Ha adatlemezt VHD regisztrálta, csatolja az új prémium szintű Storage kompatibilis Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="35a04-422">Lastly, if you have registered data disk VHDs, attach them to the new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="35a04-423">Használja a következő PowerShell-parancsmag adatlemezt csatolni az új virtuális Gépet, és adja meg a gyorsítótárazási házirendet.</span><span class="sxs-lookup"><span data-stu-id="35a04-423">Use following PowerShell cmdlet to attach data disk to the new VM and specify the caching policy.</span></span> <span data-ttu-id="35a04-424">Az alábbi példában a gyorsítótárazási házirend beállítása *ReadOnly*.</span><span class="sxs-lookup"><span data-stu-id="35a04-424">In example below the caching policy is set to *ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="35a04-425">További lépésekre lehet szükség az alkalmazás, amely támogatja az útmutató nem vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="35a04-425">There may be additional steps necessary to support your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="35a04-426">Ellenőrzése és a biztonsági mentés tervezése</span><span class="sxs-lookup"><span data-stu-id="35a04-426">Checking and plan backup</span></span>
<span data-ttu-id="35a04-427">Ha az új virtuális gép fut, hozzáférhessenek megegyező bejelentkezési azonosítóval, és jelszó van, mint az eredeti virtuális gép, és győződjön meg arról, hogy minden az elvárásoknak megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="35a04-427">Once the new VM is up and running, access it using the same login id and password is as the original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="35a04-428">Az összes beállítás a csíkozott kötetek, beleértve az új virtuális gép jelen lehet.</span><span class="sxs-lookup"><span data-stu-id="35a04-428">All the settings, including the striped volumes, would be present in the new VM.</span></span>

<span data-ttu-id="35a04-429">Az utolsó lépése az, hogy a biztonsági mentési terv, és az új virtuális gép karbantartási ütemezését az alkalmazás igények alapján.</span><span class="sxs-lookup"><span data-stu-id="35a04-429">The last step is to plan backup and maintenance schedule for the new VM based on the application's needs.</span></span>

### <span data-ttu-id="35a04-430"><a name="a-sample-migration-script"></a>Egy áttelepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="35a04-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="35a04-431">Ha több virtuális gép áttelepítése, automatizálás PowerShell-parancsprogramok hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="35a04-431">If you have multiple VMs to migrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="35a04-432">Az alábbiakban látható egy minta parancsfájlt, amely automatizálja az áttelepítés a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="35a04-432">Following is a sample script that automates the migration of a VM.</span></span> <span data-ttu-id="35a04-433">Megjegyzés: alábbi parancsfájl, amely csak egy példa, és nincsenek a jelenlegi virtuális gép lemezeivel kapcsolatos tett néhány feltételezéseket.</span><span class="sxs-lookup"><span data-stu-id="35a04-433">Note that below script is only an example, and there are few assumptions made about the current VM disks.</span></span> <span data-ttu-id="35a04-434">A parancsfájl az adott helyzetnek megfelelő frissítésére lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="35a04-434">You may need to update the script to match with your specific scenario.</span></span>

<span data-ttu-id="35a04-435">Az Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="35a04-435">The assumptions are:</span></span>

* <span data-ttu-id="35a04-436">Klasszikus Azure virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="35a04-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="35a04-437">A forrás operációs rendszer és a forrás adatok lemezek vannak ugyanabban a tárfiókban és az ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="35a04-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="35a04-438">Ha az operációs rendszer és a adatok lemezek nem ugyanazon a helyen, AzCopy vagy az Azure PowerShell használatával virtuális merevlemezek másolja át a storage-fiókok és a tárolók.</span><span class="sxs-lookup"><span data-stu-id="35a04-438">If your OS Disks and Data Disks are not in the same place, you can use AzCopy or Azure PowerShell to copy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="35a04-439">Tekintse meg az előző lépésben: [másolási VHD AzCopy vagy a PowerShell használatával](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="35a04-439">Refer to the previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="35a04-440">Ezt a parancsfájlt a forgatókönyvnek megfelelő szerkesztése egy újabb választási lehetőség, de azt javasoljuk, mert az egyszerűbb és gyorsabb AzCopy vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="35a04-440">Editing this script to meet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="35a04-441">Az automatizálási parancsfájl lejjebb tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="35a04-441">The automation script is provided below.</span></span> <span data-ttu-id="35a04-442">Szöveg cseréje az adatait, és frissítse a parancsfájlt, amellyel felelnek meg az adott forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="35a04-442">Replace text with your information and update the script to match with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="35a04-443">A meglévő parancsfájl használatával nem őrzi meg a hálózati konfigurációt a forrás virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="35a04-443">Using the existing script does not preserve the network configuration of your source VM.</span></span> <span data-ttu-id="35a04-444">Szüksége lesz az ismételt-config a hálózati beállításokat az áttelepített virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="35a04-444">You will need to re-config the networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you'd like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="35a04-445"><a name="optimization"></a>Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="35a04-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="35a04-446">Az aktuális Virtuálisgép-konfiguráció kifejezetten az működnek jól Standard lemezek szabható testre.</span><span class="sxs-lookup"><span data-stu-id="35a04-446">Your current VM configuration may be customized specifically to work well with Standard disks.</span></span> <span data-ttu-id="35a04-447">Például a teljesítmény növelése érdekében azáltal, hogy sok csíkozott kötetek használatával.</span><span class="sxs-lookup"><span data-stu-id="35a04-447">For instance, to increase the performance by using many disks in a striped volume.</span></span> <span data-ttu-id="35a04-448">Például helyett 4 lemezek külön-külön a prémium szintű Storage, esetleg képes optimalizálni a költségeket azzal, hogy egyetlen lemezre.</span><span class="sxs-lookup"><span data-stu-id="35a04-448">For example, instead of using 4 disks separately on Premium Storage, you may be able to optimize the cost by having a single disk.</span></span> <span data-ttu-id="35a04-449">Optimalizálás, például a szükséges eseti alapon kell kezelni, és egyéni lépéseket igényelnek az áttelepítés után.</span><span class="sxs-lookup"><span data-stu-id="35a04-449">Optimizations like this need to be handled on a case by case basis and require custom steps after the migration.</span></span> <span data-ttu-id="35a04-450">Emellett vegye figyelembe, hogy ez a folyamat jól nem feltétlenül alkalmas adatbázisok és a lemez elrendezése, a telepítő definiált függő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="35a04-450">Also, note that this process may not well work for databases and applications that depend on the disk layout defined in the setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="35a04-451">Előkészítése</span><span class="sxs-lookup"><span data-stu-id="35a04-451">Preparation</span></span>
1. <span data-ttu-id="35a04-452">Az áttelepítéshez egyszerű a korábbi szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="35a04-452">Complete the Simple Migration as described in the earlier section.</span></span> <span data-ttu-id="35a04-453">Optimalizálás végrehajtására kerül sor az új virtuális Gépet az áttelepítés után.</span><span class="sxs-lookup"><span data-stu-id="35a04-453">Optimizations will be performed on the new VM after the migration.</span></span>
2. <span data-ttu-id="35a04-454">Adja meg az új lemez méretét, az optimalizált konfigurálásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="35a04-454">Define the new disk sizes needed for the optimized configuration.</span></span>
3. <span data-ttu-id="35a04-455">Határozza meg, és az új lemez paramétereknek aktuális lemezek vagy kötetek hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="35a04-455">Determine mapping of the current disks/volumes to the new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="35a04-456">Végrehajtási lépések</span><span class="sxs-lookup"><span data-stu-id="35a04-456">Execution steps</span></span>
1. <span data-ttu-id="35a04-457">Hozzon létre új lemezek a megfelelő méretek a prémium szintű Storage virtuális Gépre.</span><span class="sxs-lookup"><span data-stu-id="35a04-457">Create the new disks with the right sizes on the Premium Storage VM.</span></span>
2. <span data-ttu-id="35a04-458">Bejelentkezés a virtuális gép és az adatok másolása az aktuális kötetről, hogy az új lemezt, amely, hogy a köteten van leképezve.</span><span class="sxs-lookup"><span data-stu-id="35a04-458">Login to the VM and copy the data from the current volume to the new disk that maps to that volume.</span></span> <span data-ttu-id="35a04-459">Ehhez a jelenlegi köteteket, amelyeket egy új lemezre van leképezve.</span><span class="sxs-lookup"><span data-stu-id="35a04-459">Do this for all the current volumes that need to map to a new disk.</span></span>
3. <span data-ttu-id="35a04-460">Ezután a váltson át az új lemezek beállításainak módosítása, és a régi kötetet leválasztani.</span><span class="sxs-lookup"><span data-stu-id="35a04-460">Next, change the application settings to switch to the new disks, and detach the old volumes.</span></span>

<span data-ttu-id="35a04-461">Az alkalmazás a jobb teljesítmény érdekében lemez hangolása, olvassa el [alkalmazások teljesítményének optimalizálása](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="35a04-461">For tuning the application for better disk performance, please refer to [Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="35a04-462">Alkalmazás-áttelepítések</span><span class="sxs-lookup"><span data-stu-id="35a04-462">Application migrations</span></span>
<span data-ttu-id="35a04-463">Adatbázisok és más összetett alkalmazások lehet szükség különleges lépések az alkalmazás-szolgáltató az áttelepítés által definiált konfigurációjának kialakításához.</span><span class="sxs-lookup"><span data-stu-id="35a04-463">Databases and other complex applications may require special steps as defined by the application provider for the migration.</span></span> <span data-ttu-id="35a04-464">Tekintse meg a megfelelő alkalmazás dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="35a04-464">Please refer to respective application documentation.</span></span> <span data-ttu-id="35a04-465">Például</span><span class="sxs-lookup"><span data-stu-id="35a04-465">E.g.</span></span> <span data-ttu-id="35a04-466">általában adatbázisok telepíthetők át a biztonsági mentés és visszaállítás.</span><span class="sxs-lookup"><span data-stu-id="35a04-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35a04-467">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35a04-467">Next steps</span></span>
<span data-ttu-id="35a04-468">A virtuális gépek meghatározott forgatókönyvek a következő cikkekben találhat:</span><span class="sxs-lookup"><span data-stu-id="35a04-468">See the following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="35a04-469">Az Azure virtuális gépek közötti Storage-fiókok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="35a04-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="35a04-470">Hozzon létre, és a Windows Server VHD feltöltése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="35a04-470">Create and upload a Windows Server VHD to Azure.</span></span>](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="35a04-471">Létrehozás és a Linux operációs rendszert tartalmazó virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="35a04-471">Creating and Uploading a Virtual Hard Disk that Contains the Linux Operating System</span></span>](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="35a04-472">Virtuális gépek áttelepítési Amazon AWS a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="35a04-472">Migrating Virtual Machines from Amazon AWS to Microsoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="35a04-473">A következő források további információt az Azure Storage és az Azure virtuális gépek lásd még:</span><span class="sxs-lookup"><span data-stu-id="35a04-473">Also, see the following resources to learn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="35a04-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="35a04-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="35a04-475">Az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="35a04-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="35a04-476">Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz</span><span class="sxs-lookup"><span data-stu-id="35a04-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx

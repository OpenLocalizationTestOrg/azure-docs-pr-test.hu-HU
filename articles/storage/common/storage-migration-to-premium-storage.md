---
title: "aaaMigrating virtuális gépek tooAzure prémium szintű Storage |} Microsoft Docs"
description: "Telepítse át a meglévő virtuális gépek tooAzure prémium szintű Storage. Prémium szintű Storage nagy teljesítményű, alacsony késésű támogatása az Azure virtuális gépeken futó I/O-igényes munkaterhelések kínál."
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
ms.openlocfilehash: 19aaf2b7594e570f5a964baa00958a7a8eaae97b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-premium-storage-unmanaged-disks"></a><span data-ttu-id="198d7-104">Prémium szintű Storage (felügyelet lemezek) áttelepítése tooAzure</span><span class="sxs-lookup"><span data-stu-id="198d7-104">Migrating tooAzure Premium Storage (Unmanaged Disks)</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-105">A cikk ismerteti, hogyan toomigrate egy virtuális gép által használt virtuális gép által használt nem felügyelt standard lemezek tooa nem felügyelt premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="198d7-105">This article discusses how toomigrate a VM that uses unmanaged standard disks tooa VM that uses unmanaged premium disks.</span></span> <span data-ttu-id="198d7-106">Javasoljuk, hogy Azure felügyelt lemezek használatakor az új virtuális gépekhez, és, hogy az előző nem felügyelt lemezek toomanaged lemezek konvertálása.</span><span class="sxs-lookup"><span data-stu-id="198d7-106">We recommend that you use Azure Managed Disks for new VMs, and that you convert your previous unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="198d7-107">Felügyelt lemezek leíró hello alapul szolgáló storage-fiókok, így nem kell.</span><span class="sxs-lookup"><span data-stu-id="198d7-107">Managed Disks handle hello underlying storage accounts so you don't have to.</span></span> <span data-ttu-id="198d7-108">További információkért lásd: a [felügyelt lemezekhez – áttekintés](../../virtual-machines/windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-108">For more information, please see our [Managed Disks Overview](../../virtual-machines/windows/managed-disks-overview.md).</span></span>
>

<span data-ttu-id="198d7-109">Prémium szintű Storage nagy teljesítményű, alacsony késésű lemez I/O-igényes munkaterhelések futó virtuális gépek támogatása nyújt.</span><span class="sxs-lookup"><span data-stu-id="198d7-109">Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads.</span></span> <span data-ttu-id="198d7-110">Telepítse át az alkalmazás virtuális lemezek tooAzure prémium szintű Storage is igénybe vehet hello sebesség előnyeit, és ezek a lemezek teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="198d7-110">You can take advantage of hello speed and performance of these disks by migrating your application's VM disks tooAzure Premium Storage.</span></span>

<span data-ttu-id="198d7-111">hello Ez az útmutató célja toohelp új prémium szintű Azure Storage jobb felhasználói előkészítése toomake a jelenlegi rendszer tooPremium tárolási zavartalan átmenetet.</span><span class="sxs-lookup"><span data-stu-id="198d7-111">hello purpose of this guide is toohelp new users of Azure Premium Storage better prepare toomake a smooth transition from their current system tooPremium Storage.</span></span> <span data-ttu-id="198d7-112">hello útmutató foglalkozik három hello legfontosabb összetevők, a folyamat:</span><span class="sxs-lookup"><span data-stu-id="198d7-112">hello guide addresses three of hello key components in this process:</span></span>

* [<span data-ttu-id="198d7-113">Hello áttelepítési tooPremium tárolás tervezése</span><span class="sxs-lookup"><span data-stu-id="198d7-113">Plan for hello Migration tooPremium Storage</span></span>](#plan-the-migration-to-premium-storage)
* [<span data-ttu-id="198d7-114">Készítse elő és másolási virtuális merevlemezeket (VHD) tooPremium tároló</span><span class="sxs-lookup"><span data-stu-id="198d7-114">Prepare and Copy Virtual Hard Disks (VHDs) tooPremium Storage</span></span>](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
* [<span data-ttu-id="198d7-115">Prémium szintű Storage használata Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d7-115">Create Azure Virtual Machine using Premium Storage</span></span>](#create-azure-virtual-machine-using-premium-storage)

<span data-ttu-id="198d7-116">Virtuális gépek áttelepítése a más platformok tooAzure prémium szintű Storage, vagy meglévő Azure virtuális gépek áttelepítésére Standard tárolási tooPremium tároló.</span><span class="sxs-lookup"><span data-stu-id="198d7-116">You can either migrate VMs from other platforms tooAzure Premium Storage or migrate existing Azure VMs from Standard Storage tooPremium Storage.</span></span> <span data-ttu-id="198d7-117">Ez az útmutató mindkét két forgatókönyv lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="198d7-117">This guide covers steps for both two scenarios.</span></span> <span data-ttu-id="198d7-118">A forgatókönyvtől függően hello vonatkozó szakaszában megadott hello lépéseit kövesse.</span><span class="sxs-lookup"><span data-stu-id="198d7-118">Follow hello steps specified in hello relevant section depending on your scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-119">A szolgáltatás áttekintése és a prémium szintű Storage, a prémium szintű Storage árképzési található: [nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-119">You can find a feature overview and pricing of Premium Storage in Premium Storage: [High-Performance Storage for Azure Virtual Machine Workloads](storage-premium-storage.md).</span></span> <span data-ttu-id="198d7-120">Azt javasoljuk, hogy minden virtuális gép lemezét magas IOPS tooAzure prémium szintű Storage igénylő hello legjobb teljesítmény érdekében az alkalmazás áttelepítése.</span><span class="sxs-lookup"><span data-stu-id="198d7-120">We recommend migrating any virtual machine disk requiring high IOPS tooAzure Premium Storage for hello best performance for your application.</span></span> <span data-ttu-id="198d7-121">Ha a lemez nem igényli a magas iops értéket, korlátozhatja a költségek megőrizve a szabványos tárolóban, a (merevlemezes HDD) meghajtók SSD-k helyett virtuálisgép-lemez adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="198d7-121">If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.</span></span>
>

<span data-ttu-id="198d7-122">Teljes egészében hello áttelepítési folyamat befejezése szükség lehet további műveletek előtt és után a jelen útmutatóban hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="198d7-122">Completing hello migration process in its entirety may require additional actions both before and after hello steps provided in this guide.</span></span> <span data-ttu-id="198d7-123">Például a virtuális hálózatok és a végpontok konfigurálásához, vagy a kód módosítása hello alkalmazásban, saját magát, és amely előfordulhat, hogy az alkalmazás bizonyos időre leállítást.</span><span class="sxs-lookup"><span data-stu-id="198d7-123">Examples include configuring virtual networks or endpoints or making code changes within hello application itself which may require some downtime in your application.</span></span> <span data-ttu-id="198d7-124">Ezek a műveletek egyedi tooeach alkalmazás, és el kell végeznie ezeket hello ismertetett lépéseket: Ez az útmutató toomake hello teljes átmenet tooPremium, zökkenőmentes lehető tárolási együtt.</span><span class="sxs-lookup"><span data-stu-id="198d7-124">These actions are unique tooeach application, and you should complete them along with hello steps provided in this guide toomake hello full transition tooPremium Storage as seamless as possible.</span></span>

## <span data-ttu-id="198d7-125"><a name="plan-the-migration-to-premium-storage"></a>Hello áttelepítési tooPremium tárolás tervezése</span><span class="sxs-lookup"><span data-stu-id="198d7-125"><a name="plan-the-migration-to-premium-storage"></a>Plan for hello migration tooPremium Storage</span></span>
<span data-ttu-id="198d7-126">Ez a szakasz biztosítja, hogy készen áll a toofollow hello áttelepítési cikkben leírt lépéseket, és segít toomake hello legjobb döntést a virtuális gép és a lemez típusok.</span><span class="sxs-lookup"><span data-stu-id="198d7-126">This section ensures that you are ready toofollow hello migration steps in this article, and helps you toomake hello best decision on VM and Disk types.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="198d7-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="198d7-127">Prerequisites</span></span>
* <span data-ttu-id="198d7-128">Szüksége lesz egy Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="198d7-128">You will need an Azure subscription.</span></span> <span data-ttu-id="198d7-129">Ha még nincs fiókja, létrehozhat egy hónapos [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/) előfizetés, vagy keresse fel [Azure díjszabása](https://azure.microsoft.com/pricing/) további lehetőségekért.</span><span class="sxs-lookup"><span data-stu-id="198d7-129">If you don't have one, you can create a one-month [free trial](https://azure.microsoft.com/pricing/free-trial/) subscription or visit [Azure Pricing](https://azure.microsoft.com/pricing/) for more options.</span></span>
* <span data-ttu-id="198d7-130">PowerShell-parancsmagok tooexecute, szüksége lesz a hello Microsoft Azure PowerShell modul.</span><span class="sxs-lookup"><span data-stu-id="198d7-130">tooexecute PowerShell cmdlets, you will need hello Microsoft Azure PowerShell module.</span></span> <span data-ttu-id="198d7-131">Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello telepítse pont és a telepítési utasításokat.</span><span class="sxs-lookup"><span data-stu-id="198d7-131">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello install point and installation instructions.</span></span>
* <span data-ttu-id="198d7-132">Ha azt tervezi, toouse Azure virtuális gépeken futó a prémium szintű Storage, prémium szintű Storage képes a virtuális gépek toouse hello kell.</span><span class="sxs-lookup"><span data-stu-id="198d7-132">When you plan toouse Azure VMs running on Premium Storage, you need toouse hello Premium Storage capable VMs.</span></span> <span data-ttu-id="198d7-133">Prémium szintű Storage képes a virtuális gépek a Standard és prémium szintű Storage lemezek is használhatók.</span><span class="sxs-lookup"><span data-stu-id="198d7-133">You can use both Standard and Premium Storage disks with Premium Storage capable VMs.</span></span> <span data-ttu-id="198d7-134">Prémium szintű storage lemezek lesz jövőbeli hello további Virtuálisgép-típusokon érhető el.</span><span class="sxs-lookup"><span data-stu-id="198d7-134">Premium storage disks will be available with more VM types in hello future.</span></span> <span data-ttu-id="198d7-135">További információ az összes elérhető Azure virtuális lemez típusát és méretét: [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Felhőszolgáltatások mérete](../../cloud-services/cloud-services-sizes-specs.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-135">For more information on all available Azure VM disk types and sizes, see [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and [Sizes for Cloud Services](../../cloud-services/cloud-services-sizes-specs.md).</span></span>

### <a name="considerations"></a><span data-ttu-id="198d7-136">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="198d7-136">Considerations</span></span>
<span data-ttu-id="198d7-137">Egy Azure virtuális gép támogatja a prémium szintű Storage több lemezt csatolni, hogy az alkalmazások too256 TB-nyi tárhelyre virtuális gépenként legfeljebb tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="198d7-137">An Azure VM supports attaching several Premium Storage disks so that your applications can have up too256 TB of storage per VM.</span></span> <span data-ttu-id="198d7-138">Prémium szintű Storage az alkalmazások egy második lemez adatátviteli sebessége virtuális gépenként a rendkívül alacsony késleltetésű az olvasási műveletek érhet 80000 iops-értéket (bemeneti/kimeneti műveletek száma másodpercenként) virtuális Gépet és 2000 MB.</span><span class="sxs-lookup"><span data-stu-id="198d7-138">With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.</span></span> <span data-ttu-id="198d7-139">Virtuális gépek és a lemezek számos lehetősége van.</span><span class="sxs-lookup"><span data-stu-id="198d7-139">You have options in a variety of VMs and Disks.</span></span> <span data-ttu-id="198d7-140">Ez a szakasz az toohelp toofind olyan beállítás, amely a legjobban megfelel a számítási feladathoz.</span><span class="sxs-lookup"><span data-stu-id="198d7-140">This section is toohelp you toofind an option that best suits your workload.</span></span>

#### <a name="vm-sizes"></a><span data-ttu-id="198d7-141">A virtuális gépek mérete</span><span class="sxs-lookup"><span data-stu-id="198d7-141">VM sizes</span></span>
<span data-ttu-id="198d7-142">hello Azure virtuális gép mérete specifikációk szereplő [virtuális gépek méretei](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="198d7-142">hello Azure VM size specifications are listed in [Sizes for virtual machines](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="198d7-143">Tekintse át a virtuális gépek, amely együttműködik a prémium szintű Storage, és válassza ki a hello leginkább megfelelő virtuális gép méretét, amely a legjobban megfelel a számítási feladatok hello teljesítményétől.</span><span class="sxs-lookup"><span data-stu-id="198d7-143">Review hello performance characteristics of virtual machines that work with Premium Storage and choose hello most appropriate VM size that best suits your workload.</span></span> <span data-ttu-id="198d7-144">Győződjön meg arról, hogy nincs elegendő sávszélesség érhető el a virtuális gép toodrive hello lemez forgalom.</span><span class="sxs-lookup"><span data-stu-id="198d7-144">Make sure that there is sufficient bandwidth available on your VM toodrive hello disk traffic.</span></span>

#### <a name="disk-sizes"></a><span data-ttu-id="198d7-145">Lemezméretek</span><span class="sxs-lookup"><span data-stu-id="198d7-145">Disk sizes</span></span>
<span data-ttu-id="198d7-146">Öt különböző típusú lemezek, amelyek együtt a virtuális Gépet, és mindegyik rendelkezik-e adott iops-érték és átviteli korlátok.</span><span class="sxs-lookup"><span data-stu-id="198d7-146">There are five types of disks that can be used with your VM and each has specific IOPs and throughput limits.</span></span> <span data-ttu-id="198d7-147">Vegye figyelembe a működés felső korlátjának hello lemez kiválasztása a virtuális gép alapján az alkalmazás kapacitása, teljesítmény, méretezhetőség hello igényeinek, és a maximális tölti be.</span><span class="sxs-lookup"><span data-stu-id="198d7-147">Take into consideration these limits when choosing hello disk type for your VM based on hello needs of your application in terms of capacity, performance, scalability, and peak loads.</span></span>

| <span data-ttu-id="198d7-148">Prémium szintű lemezek típusa</span><span class="sxs-lookup"><span data-stu-id="198d7-148">Premium Disks Type</span></span>  | <span data-ttu-id="198d7-149">P10</span><span class="sxs-lookup"><span data-stu-id="198d7-149">P10</span></span>   | <span data-ttu-id="198d7-150">P20</span><span class="sxs-lookup"><span data-stu-id="198d7-150">P20</span></span>   | <span data-ttu-id="198d7-151">P30</span><span class="sxs-lookup"><span data-stu-id="198d7-151">P30</span></span>            | <span data-ttu-id="198d7-152">P40</span><span class="sxs-lookup"><span data-stu-id="198d7-152">P40</span></span>            | <span data-ttu-id="198d7-153">P50</span><span class="sxs-lookup"><span data-stu-id="198d7-153">P50</span></span>            | 
|:-------------------:|:-----:|:-----:|:--------------:|:--------------:|:--------------:|
| <span data-ttu-id="198d7-154">Lemezméret</span><span class="sxs-lookup"><span data-stu-id="198d7-154">Disk size</span></span>           | <span data-ttu-id="198d7-155">128 GB</span><span class="sxs-lookup"><span data-stu-id="198d7-155">128 GB</span></span>| <span data-ttu-id="198d7-156">512 GB</span><span class="sxs-lookup"><span data-stu-id="198d7-156">512 GB</span></span>| <span data-ttu-id="198d7-157">1024 GB (1 TB)</span><span class="sxs-lookup"><span data-stu-id="198d7-157">1024 GB (1 TB)</span></span> | <span data-ttu-id="198d7-158">2048 GB (2 TB)</span><span class="sxs-lookup"><span data-stu-id="198d7-158">2048 GB (2 TB)</span></span> | <span data-ttu-id="198d7-159">4095 GB (4 TB)</span><span class="sxs-lookup"><span data-stu-id="198d7-159">4095 GB (4 TB)</span></span> | 
| <span data-ttu-id="198d7-160">IOPS-érték lemezenként</span><span class="sxs-lookup"><span data-stu-id="198d7-160">IOPS per disk</span></span>       | <span data-ttu-id="198d7-161">500</span><span class="sxs-lookup"><span data-stu-id="198d7-161">500</span></span>   | <span data-ttu-id="198d7-162">2300</span><span class="sxs-lookup"><span data-stu-id="198d7-162">2300</span></span>  | <span data-ttu-id="198d7-163">5000</span><span class="sxs-lookup"><span data-stu-id="198d7-163">5000</span></span>           | <span data-ttu-id="198d7-164">7500</span><span class="sxs-lookup"><span data-stu-id="198d7-164">7500</span></span>           | <span data-ttu-id="198d7-165">7500</span><span class="sxs-lookup"><span data-stu-id="198d7-165">7500</span></span>           | 
| <span data-ttu-id="198d7-166">Adattovábbítás lemezenként</span><span class="sxs-lookup"><span data-stu-id="198d7-166">Throughput per disk</span></span> | <span data-ttu-id="198d7-167">100 MB / s</span><span class="sxs-lookup"><span data-stu-id="198d7-167">100 MB per second</span></span> | <span data-ttu-id="198d7-168">150 MB / s</span><span class="sxs-lookup"><span data-stu-id="198d7-168">150 MB per second</span></span> | <span data-ttu-id="198d7-169">200 MB / s</span><span class="sxs-lookup"><span data-stu-id="198d7-169">200 MB per second</span></span> | <span data-ttu-id="198d7-170">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="198d7-170">250 MB per second</span></span> | <span data-ttu-id="198d7-171">250 MB / s</span><span class="sxs-lookup"><span data-stu-id="198d7-171">250 MB per second</span></span> |

<span data-ttu-id="198d7-172">Attól függően, hogy a munkaterhelés annak eldöntése, hogy adatlemeznek a virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="198d7-172">Depending on your workload, determine if additional data disks are necessary for your VM.</span></span> <span data-ttu-id="198d7-173">Több állandó adatok lemezek tooyour VM csatolható.</span><span class="sxs-lookup"><span data-stu-id="198d7-173">You can attach several persistent data disks tooyour VM.</span></span> <span data-ttu-id="198d7-174">Ha szükséges, a is paritásos hello lemezek tooincrease hello kapacitást és teljesítményt hello kötet között.</span><span class="sxs-lookup"><span data-stu-id="198d7-174">If needed, you can stripe across hello disks tooincrease hello capacity and performance of hello volume.</span></span> <span data-ttu-id="198d7-175">(Megtudhatja, mi lemez csíkozást [Itt](storage-premium-storage-performance.md#disk-striping).) Ha Ön paritásos prémium szintű Storage adatlemezek használata [tárolóhelyek][4], úgy kell beállítania, egyoszlopos használt lemezek.</span><span class="sxs-lookup"><span data-stu-id="198d7-175">(See what is Disk Striping [here](storage-premium-storage-performance.md#disk-striping).) If you stripe Premium Storage data disks using [Storage Spaces][4], you should configure it with one column for each disk that is used.</span></span> <span data-ttu-id="198d7-176">Ellenkező esetben hello teljes csíkozott hello kötet lehet a teljesítmény kisebb, mint a várt forgalom toouneven terjesztési miatt hello lemezek között.</span><span class="sxs-lookup"><span data-stu-id="198d7-176">Otherwise, hello overall performance of hello striped volume may be lower than expected due toouneven distribution of traffic across hello disks.</span></span> <span data-ttu-id="198d7-177">A Linux virtuális gépek használhatják hello *mdadm* segédprogram tooachieve hello azonos.</span><span class="sxs-lookup"><span data-stu-id="198d7-177">For Linux VMs you can use hello *mdadm* utility tooachieve hello same.</span></span> <span data-ttu-id="198d7-178">Cikke [szoftver RAID konfigurálása Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="198d7-178">See article [Configure Software RAID on Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for details.</span></span>

#### <a name="storage-account-scalability-targets"></a><span data-ttu-id="198d7-179">Tárfiókra vonatkozó méretezhetőségi célok</span><span class="sxs-lookup"><span data-stu-id="198d7-179">Storage account scalability targets</span></span>
<span data-ttu-id="198d7-180">Prémium szintű Storage-fiókok rendelkezik méretezhetőségi célok hozzáadása toohello a következő hello [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-180">Premium Storage accounts have hello following scalability targets in addition toohello [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).</span></span> <span data-ttu-id="198d7-181">Ha az alkalmazás követelményeinek ennél nagyobb hello méretezhetőségi célok egyetlen tárfiók, hozza létre a az alkalmazás toouse több tárfiókot, és az adatok particionálása adott tárfiókok között.</span><span class="sxs-lookup"><span data-stu-id="198d7-181">If your application requirements exceed hello scalability targets of a single storage account, build your application toouse multiple storage accounts, and partition your data across those storage accounts.</span></span>

| <span data-ttu-id="198d7-182">Teljes kapacitásával</span><span class="sxs-lookup"><span data-stu-id="198d7-182">Total Account Capacity</span></span> | <span data-ttu-id="198d7-183">A helyileg redundáns tárolás fiók teljes sávszélesség</span><span class="sxs-lookup"><span data-stu-id="198d7-183">Total Bandwidth for a Locally Redundant Storage Account</span></span> |
|:--- |:--- |
| <span data-ttu-id="198d7-184">Lemez kapacitás: 35TB</span><span class="sxs-lookup"><span data-stu-id="198d7-184">Disk capacity: 35TB</span></span><br /><span data-ttu-id="198d7-185">Pillanatkép-kapacitás: 10 TB</span><span class="sxs-lookup"><span data-stu-id="198d7-185">Snapshot capacity: 10 TB</span></span> |<span data-ttu-id="198d7-186">Too50 Gigabit / másodperc, a bejövő + kimenő mentése</span><span class="sxs-lookup"><span data-stu-id="198d7-186">Up too50 gigabits per second for Inbound + Outbound</span></span> |

<span data-ttu-id="198d7-187">A prémium szintű Storage specifikációk bővebben hello, tekintse meg [méretezhetőséget és a prémium szintű Storage használatakor Performance Targets](storage-premium-storage.md#scalability-and-performance-targets).</span><span class="sxs-lookup"><span data-stu-id="198d7-187">For hello more information on Premium Storage specifications, check out [Scalability and Performance Targets when using Premium Storage](storage-premium-storage.md#scalability-and-performance-targets).</span></span>

#### <a name="disk-caching-policy"></a><span data-ttu-id="198d7-188">Lemez gyorsítótárazási házirend</span><span class="sxs-lookup"><span data-stu-id="198d7-188">Disk caching policy</span></span>
<span data-ttu-id="198d7-189">Alapértelmezés szerint a gyorsítótárazási házirend lemez van *írásvédett* az összes hello prémium adatlemezek, és *írható-olvasható* hello prémium operációsrendszer-lemez csatolni a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="198d7-189">By default, disk caching policy is *Read-Only* for all hello Premium data disks, and *Read-Write* for hello Premium operating system disk attached toohello VM.</span></span> <span data-ttu-id="198d7-190">A konfigurációs beállítás ajánlott tooachieve hello optimális teljesítménye az alkalmazás IOs-hez.</span><span class="sxs-lookup"><span data-stu-id="198d7-190">This configuration setting is recommended tooachieve hello optimal performance for your application's IOs.</span></span> <span data-ttu-id="198d7-191">Írási műveleteket vagy a csak írható adatlemezek (köztük SQL Server) tiltsa le a lemezt gyorsítótárazás, hogy az alkalmazás jobb teljesítményt érhet el.</span><span class="sxs-lookup"><span data-stu-id="198d7-191">For write-heavy or write-only data disks (such as SQL Server log files), disable disk caching so that you can achieve better application performance.</span></span> <span data-ttu-id="198d7-192">a meglévő adatlemezek hello gyorsítótár beállításait is frissítve [Azure Portal](https://portal.azure.com) vagy hello *- HostCaching* hello paramétere *Set-AzureDataDisk* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="198d7-192">hello cache settings for existing data disks can be updated using [Azure Portal](https://portal.azure.com) or hello *-HostCaching* parameter of hello *Set-AzureDataDisk* cmdlet.</span></span>

#### <a name="location"></a><span data-ttu-id="198d7-193">Hely</span><span class="sxs-lookup"><span data-stu-id="198d7-193">Location</span></span>
<span data-ttu-id="198d7-194">Jelölje ki a helyet, ahol a prémium szintű Azure Storage áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="198d7-194">Pick a location where Azure Premium Storage is available.</span></span> <span data-ttu-id="198d7-195">Lásd: [Azure-szolgáltatások régiónként](https://azure.microsoft.com/regions/#services) elérhető helyről naprakész tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="198d7-195">See [Azure Services by Region](https://azure.microsoft.com/regions/#services) for up-to-date information on available locations.</span></span> <span data-ttu-id="198d7-196">Virtuális gépek található hello azonos módon, hogy a tárolja a virtuális gép hello ad sok különböző régiókban azok jobb teljesítményt hello lemezeket a tárfiók hello régióban.</span><span class="sxs-lookup"><span data-stu-id="198d7-196">VMs located in hello same region as hello Storage account that stores hello disks for hello VM will give much better performance than if they are in separate regions.</span></span>

#### <a name="other-azure-vm-configuration-settings"></a><span data-ttu-id="198d7-197">Egyéb Azure virtuális gép konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="198d7-197">Other Azure VM configuration settings</span></span>
<span data-ttu-id="198d7-198">Egy Azure virtuális gép létrehozásakor meg kell adnia tooconfigure egyes virtuális gép beállításait.</span><span class="sxs-lookup"><span data-stu-id="198d7-198">When creating an Azure VM, you will be asked tooconfigure certain VM settings.</span></span> <span data-ttu-id="198d7-199">Ne feledje, hogy néhány beállításainak rögzítése a virtuális gép, hello hello élettartama során módosítása, vagy később fel más.</span><span class="sxs-lookup"><span data-stu-id="198d7-199">Remember, few settings are fixed for hello lifetime of hello VM, while you can modify or add others later.</span></span> <span data-ttu-id="198d7-200">Tekintse át a Azure virtuális gép konfigurációs beállítások, és győződjön meg arról, hogy ezek a munkaterhelés igényeihez toomatch megfelelően konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="198d7-200">Review these Azure VM configuration settings and make sure that these are configured appropriately toomatch your workload requirements.</span></span>

### <a name="optimization"></a><span data-ttu-id="198d7-201">Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="198d7-201">Optimization</span></span>
<span data-ttu-id="198d7-202">[Prémium szintű Storage: Nagy teljesítményű kialakítása](storage-premium-storage-performance.md) útmutatást nyújt a prémium szintű Azure Storage használatával nagy teljesítményű alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="198d7-202">[Azure Premium Storage: Design for High Performance](storage-premium-storage-performance.md) provides guidelines for building high-performance applications using Azure Premium Storage.</span></span> <span data-ttu-id="198d7-203">Teljesítmény bevált gyakorlatok alkalmazható tootechnologies az alkalmazás által használt együtt hello irányelvek követésével.</span><span class="sxs-lookup"><span data-stu-id="198d7-203">You can follow hello guidelines combined with performance best practices applicable tootechnologies used by your application.</span></span>

## <span data-ttu-id="198d7-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Készítse elő, és másolja a virtuális merevlemezek (VHD) tooPremium tároló</span><span class="sxs-lookup"><span data-stu-id="198d7-204"><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Prepare and copy virtual hard disks (VHDs) tooPremium Storage</span></span>
<span data-ttu-id="198d7-205">a következő szakasz hello előkészítése virtuális merevlemezek a virtuális gépről, és másolja a VHD-k tooAzure tárolási útmutatásokat.</span><span class="sxs-lookup"><span data-stu-id="198d7-205">hello following section provides guidelines for preparing VHDs from your VM and copying VHDs tooAzure Storage.</span></span>

* [<span data-ttu-id="198d7-206">1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek tooAzure prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="198d7-206">Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>](#scenario1)
* [<span data-ttu-id="198d7-207">2. forgatókönyv: "I vagyok telepít virtuális gépeket más platformokon tooAzure prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="198d7-207">Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>](#scenario2)

### <a name="prerequisites"></a><span data-ttu-id="198d7-208">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="198d7-208">Prerequisites</span></span>
<span data-ttu-id="198d7-209">virtuális merevlemezek tooprepare hello az áttelepítéshez, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="198d7-209">tooprepare hello VHDs for migration, you'll need:</span></span>

* <span data-ttu-id="198d7-210">Azure-előfizetéssel, a tárfiók és egy tárolóját, hogy a tárolási fiók toowhich másolhatja a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="198d7-210">An Azure subscription, a storage account, and a container in that storage account toowhich you can copy your VHD.</span></span> <span data-ttu-id="198d7-211">Vegye figyelembe, hogy a cél tárfiókkal hello lehet-e a Standard vagy prémium szintű Storage fiók igényektől függően.</span><span class="sxs-lookup"><span data-stu-id="198d7-211">Note that hello destination storage account can be a Standard or Premium Storage account depending on your requirement.</span></span>
* <span data-ttu-id="198d7-212">Egy eszköz toogeneralize hello VHD-t, ha azt tervezi, toocreate több Virtuálisgép-példányok belőle.</span><span class="sxs-lookup"><span data-stu-id="198d7-212">A tool toogeneralize hello VHD if you plan toocreate multiple VM instances from it.</span></span> <span data-ttu-id="198d7-213">Például a Windows vagy adatb-sysprep Ubuntu a sysprep.</span><span class="sxs-lookup"><span data-stu-id="198d7-213">For example, sysprep for Windows or virt-sysprep for Ubuntu.</span></span>
* <span data-ttu-id="198d7-214">Egy eszköz tooupload hello VHD fájl toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="198d7-214">A tool tooupload hello VHD file toohello Storage account.</span></span> <span data-ttu-id="198d7-215">Lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md) , vagy használjon egy [Azure Tártallózó](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span><span class="sxs-lookup"><span data-stu-id="198d7-215">See [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md) or use an [Azure storage explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx).</span></span> <span data-ttu-id="198d7-216">Ez az útmutató ismerteti, hogy a virtuális merevlemez hello AzCopy eszközzel másolása.</span><span class="sxs-lookup"><span data-stu-id="198d7-216">This guide describes copying your VHD using hello AzCopy tool.</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-217">A lehetőséghez szinkron másolatot az AzCopy, az optimális teljesítmény érdekében másolja a VHD-való futtatásával az eszközöket egy Azure virtuális Gépen, amely hello hello cél tárfiókkal megegyező régióban.</span><span class="sxs-lookup"><span data-stu-id="198d7-217">If you choose synchronous copy option with AzCopy, for optimal performance, copy your VHD by running one of these tools from an Azure VM that is in hello same region as hello destination storage account.</span></span> <span data-ttu-id="198d7-218">Virtuális merevlemez másolása az Azure virtuális gép egy másik régióban található, a teljesítmény csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="198d7-218">If you are copying a VHD from an Azure VM in a different region, your performance may be slower.</span></span>
>
> <span data-ttu-id="198d7-219">Nagy mennyiségű adatot felülírását korlátozott sávszélességű, fontolja meg [hello Azure Import/Export szolgáltatás tootransfer adatok tooBlob Storage használatával](../storage-import-export-service.md); ez tootransfer lehetővé teszi az adatok szállítási merevlemez-meghajtók tooan Azure Datacenter.</span><span class="sxs-lookup"><span data-stu-id="198d7-219">For copying a large amount of data over limited bandwidth, consider [using hello Azure Import/Export service tootransfer data tooBlob Storage](../storage-import-export-service.md); this allows you tootransfer your data by shipping hard disk drives tooan Azure datacenter.</span></span> <span data-ttu-id="198d7-220">Hello Azure Import/Export szolgáltatás toocopy adatok tooa standard szintű tárfiók csak is használhatja.</span><span class="sxs-lookup"><span data-stu-id="198d7-220">You can use hello Azure Import/Export service toocopy data tooa standard storage account only.</span></span> <span data-ttu-id="198d7-221">Ha a standard szintű tárfiók hello adatok, vagy hello használhatja [másolási Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) vagy AzCopy tootransfer hello adatok tooyour prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="198d7-221">Once hello data is in your standard storage account, you can use either hello [Copy Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) or AzCopy tootransfer hello data tooyour premium storage account.</span></span>
>
> <span data-ttu-id="198d7-222">Ügyeljen arra, hogy a Microsoft Azure csak támogatja-e a rögzített méretű VHD-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="198d7-222">Note that Microsoft Azure only supports fixed size VHD files.</span></span> <span data-ttu-id="198d7-223">A VHDX-fájlok vagy a dinamikus VHD-k nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="198d7-223">VHDX files or dynamic VHDs are not supported.</span></span> <span data-ttu-id="198d7-224">Ha egy dinamikus VHD-t, akkor átalakíthatja toofixed méretét hello segítségével [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="198d7-224">If you have a dynamic VHD, you can convert it toofixed size using hello [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) cmdlet.</span></span>
>
>

### <span data-ttu-id="198d7-225"><a name="scenario1"></a>1. forgatókönyv: "I vagyok áttelepíti meglévő Azure virtuális gépek tooAzure prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="198d7-225"><a name="scenario1"></a>Scenario 1: "I am migrating existing Azure VMs tooAzure Premium Storage."</span></span>
<span data-ttu-id="198d7-226">Ha az áttelepítés meglévő Azure virtuális gépek, virtuális gépek leállítási hello készítse elő a virtuális merevlemezek hello típusonkénti kívánt VHD-fájl, és másolja hello VHD AzCopy vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="198d7-226">If you are migrating existing Azure VMs, stop hello VM, prepare VHDs per hello type of VHD you want, and then copy hello VHD with AzCopy or PowerShell.</span></span>

<span data-ttu-id="198d7-227">hello VM kell toobe toomigrate tiszta le teljesen.</span><span class="sxs-lookup"><span data-stu-id="198d7-227">hello VM needs toobe completely down toomigrate a clean state.</span></span> <span data-ttu-id="198d7-228">Lesz olyan állásidő hello áttelepítésének befejeződéséig.</span><span class="sxs-lookup"><span data-stu-id="198d7-228">There will be a downtime until hello migration completes.</span></span>

#### <a name="step-1-prepare-vhds-for-migration"></a><span data-ttu-id="198d7-229">1. lépés</span><span class="sxs-lookup"><span data-stu-id="198d7-229">Step 1.</span></span> <span data-ttu-id="198d7-230">Virtuális merevlemezek Felkészülés az áttelepítésre</span><span class="sxs-lookup"><span data-stu-id="198d7-230">Prepare VHDs for migration</span></span>
<span data-ttu-id="198d7-231">Ha a meglévő Azure virtuális gépek tooPremium tárolási telepít át, a virtuális merevlemez lehet:</span><span class="sxs-lookup"><span data-stu-id="198d7-231">If you are migrating existing Azure VMs tooPremium Storage, your VHD may be:</span></span>

* <span data-ttu-id="198d7-232">Általános operációsrendszer-lemezkép elkészítése</span><span class="sxs-lookup"><span data-stu-id="198d7-232">A generalized operating system image</span></span>
* <span data-ttu-id="198d7-233">Egy egyedi operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="198d7-233">A unique operating system disk</span></span>
* <span data-ttu-id="198d7-234">Adatlemez</span><span class="sxs-lookup"><span data-stu-id="198d7-234">A data disk</span></span>

<span data-ttu-id="198d7-235">Az alábbiakban azt végezze el a virtuális merevlemez előkészítése 3 forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="198d7-235">Below we walk through these 3 scenarios for preparing your VHD.</span></span>

##### <a name="use-a-generalized-operating-system-vhd-toocreate-multiple-vm-instances"></a><span data-ttu-id="198d7-236">Egy általános operációs rendszer virtuális merevlemez toocreate több Virtuálisgép-példány használata</span><span class="sxs-lookup"><span data-stu-id="198d7-236">Use a generalized Operating System VHD toocreate multiple VM instances</span></span>
<span data-ttu-id="198d7-237">Ha virtuális Merevlemezt, amely lesz használt toocreate több általános Azure Virtuálisgép-példányok, először meg kell generalize virtuális Merevlemezt a sysprep segédprogrammal.</span><span class="sxs-lookup"><span data-stu-id="198d7-237">If you are uploading a VHD that will be used toocreate multiple generic Azure VM instances, you must first generalize VHD using a sysprep utility.</span></span> <span data-ttu-id="198d7-238">Ez vonatkozik, amely a helyi virtuális merevlemez tooa vagy hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="198d7-238">This applies tooa VHD that is on-premises or in hello cloud.</span></span> <span data-ttu-id="198d7-239">Sysprep eltávolítása hello VHD-t bármely gépen-specifikus adatait.</span><span class="sxs-lookup"><span data-stu-id="198d7-239">Sysprep removes any machine-specific information from hello VHD.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="198d7-240">Készítsen pillanatképet, vagy biztonsági mentése a virtuális gép előtt normalizálása azt.</span><span class="sxs-lookup"><span data-stu-id="198d7-240">Take a snapshot or backup your VM before generalizing it.</span></span> <span data-ttu-id="198d7-241">A sysprep fut le fog állni, és hello Virtuálisgép-példány felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="198d7-241">Running sysprep will stop and deallocate hello VM instance.</span></span> <span data-ttu-id="198d7-242">Kövesse az alábbi Windows operációs rendszer virtuális merevlemez toosysprep lépéseket.</span><span class="sxs-lookup"><span data-stu-id="198d7-242">Follow steps below toosysprep a Windows OS VHD.</span></span> <span data-ttu-id="198d7-243">Vegye figyelembe, hogy hello a Sysprep parancsot futtatja fogja a szükséges tooshut le hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="198d7-243">Note that running hello Sysprep command will require you tooshut down hello virtual machine.</span></span> <span data-ttu-id="198d7-244">További információ a Sysprep: [Sysprep áttekintése](http://technet.microsoft.com/library/hh825209.aspx) vagy [Sysprep műszaki útmutatója](http://technet.microsoft.com/library/cc766049.aspx).</span><span class="sxs-lookup"><span data-stu-id="198d7-244">For more information about Sysprep, see [Sysprep Overview](http://technet.microsoft.com/library/hh825209.aspx) or [Sysprep Technical Reference](http://technet.microsoft.com/library/cc766049.aspx).</span></span>
>
>

1. <span data-ttu-id="198d7-245">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="198d7-245">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="198d7-246">Adja meg a következő parancs tooopen Sysprep hello:</span><span class="sxs-lookup"><span data-stu-id="198d7-246">Enter hello following command tooopen Sysprep:</span></span>

    ```
    %windir%\system32\sysprep\sysprep.exe
    ```

3. <span data-ttu-id="198d7-247">A hello rendszer-előkészítő eszközt, jelölje be adja meg a rendszer Out-of-Box élmény (OOBE), jelölje be hello Generalize jelölőnégyzetet, válassza ki **leállítási**, és kattintson a **OK**, az alábbi hello ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="198d7-247">In hello System Preparation Tool, select Enter System Out-of-Box Experience (OOBE), select hello Generalize check box, select **Shutdown**, and then click **OK**, as shown in hello image below.</span></span> <span data-ttu-id="198d7-248">Sysprep általánosítja hello operációs rendszert, és a hello rendszer leállítása.</span><span class="sxs-lookup"><span data-stu-id="198d7-248">Sysprep will generalize hello operating system and shut down hello system.</span></span>

    ![][1]

<span data-ttu-id="198d7-249">Egy Ubuntu virtuális gép használata a sysprep adatb tooachieve hello azonos.</span><span class="sxs-lookup"><span data-stu-id="198d7-249">For an Ubuntu VM, use virt-sysprep tooachieve hello same.</span></span> <span data-ttu-id="198d7-250">Lásd: [adatb-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="198d7-250">See [virt-sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) for more details.</span></span> <span data-ttu-id="198d7-251">További információ az egyes hello nyílt forráskódú [Linux Server kiépítés szoftver](http://www.cyberciti.biz/tips/server-provisioning-software.html) más Linux operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="198d7-251">See also some of hello open source [Linux Server Provisioning software](http://www.cyberciti.biz/tips/server-provisioning-software.html) for other Linux operating systems.</span></span>

##### <a name="use-a-unique-operating-system-vhd-toocreate-a-single-vm-instance"></a><span data-ttu-id="198d7-252">Egy egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Virtuálisgép-példány használata</span><span class="sxs-lookup"><span data-stu-id="198d7-252">Use a unique Operating System VHD toocreate a single VM instance</span></span>
<span data-ttu-id="198d7-253">Ha hello hello gép adatokat igénylő virtuális gép futó alkalmazást, nem generalize hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="198d7-253">If you have an application running on hello VM which requires hello machine specific data, do not generalize hello VHD.</span></span> <span data-ttu-id="198d7-254">Nem általánosított virtuális Merevlemezt lehet használt toocreate egy egyedi Azure Virtuálisgép-példány.</span><span class="sxs-lookup"><span data-stu-id="198d7-254">A non-generalized VHD can be used toocreate a unique Azure VM instance.</span></span> <span data-ttu-id="198d7-255">Például ha a tartományvezérlő van a VHD-t, sysprep végrehajtása megkönnyítő hatástalan tartományvezérlőként.</span><span class="sxs-lookup"><span data-stu-id="198d7-255">For example, if you have Domain Controller on your VHD, executing sysprep will make it ineffective as a Domain Controller.</span></span> <span data-ttu-id="198d7-256">Tekintse át a virtuális gép és hello hatása rajtuk sysprep futtatása előtt hello VHD normalizálása futó hello alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="198d7-256">Review hello applications running on your VM and hello impact of running sysprep on them before generalizing hello VHD.</span></span>

##### <a name="register-data-disk-vhd"></a><span data-ttu-id="198d7-257">Virtuális merevlemez adatlemeze regisztrálása</span><span class="sxs-lookup"><span data-stu-id="198d7-257">Register data disk VHD</span></span>
<span data-ttu-id="198d7-258">Ha rendelkezik Azure toobe adatlemezek át, győződjön meg arról, hogy hello virtuális gépek, amelyek használják a lemezek állnak le ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="198d7-258">If you have data disks in Azure toobe migrated, you must make sure hello VMs that use these data disks are shut down.</span></span>

<span data-ttu-id="198d7-259">Kövesse az alábbiakban toocopy VHD tooAzure prémium szintű Storage hello lépéseket, és regisztrálja kiosztott adatok lemezként.</span><span class="sxs-lookup"><span data-stu-id="198d7-259">Follow hello steps described below toocopy VHD tooAzure Premium Storage and register it as a provisioned data disk.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="198d7-260">2. lépés</span><span class="sxs-lookup"><span data-stu-id="198d7-260">Step 2.</span></span> <span data-ttu-id="198d7-261">Hello cél a virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d7-261">Create hello destination for your VHD</span></span>
<span data-ttu-id="198d7-262">Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="198d7-262">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="198d7-263">Vegye figyelembe a where megtervezésekor a következő pontok hello toostore merevlemezek:</span><span class="sxs-lookup"><span data-stu-id="198d7-263">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="198d7-264">hello célként prémium szintű storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="198d7-264">hello target Premium storage account.</span></span>
* <span data-ttu-id="198d7-265">hello tárfiók helyének meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat hello utolsó szakaszában.</span><span class="sxs-lookup"><span data-stu-id="198d7-265">hello storage account location must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="198d7-266">Új tárfiók tooa vagy terv toouse hello igényei szerint ugyanazt a tárfiókot nem átmásolni.</span><span class="sxs-lookup"><span data-stu-id="198d7-266">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="198d7-267">Másolja ki és mentse a hello tárfiók kulcsa hello cél tárfiók hello következő szakaszra.</span><span class="sxs-lookup"><span data-stu-id="198d7-267">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="198d7-268">Adatlemezek választhatja ki tookeep néhány adatlemezek egy standard szintű tárfiókot (például lemezek hűtőre tárhellyel rendelkező), de határozottan javasoljuk, hogy áthelyezése éles munkaterhelés toouse prémium szintű storage összes adatát.</span><span class="sxs-lookup"><span data-stu-id="198d7-268">For data disks, you can choose tookeep some data disks in a standard storage account (for example, disks that have cooler storage), but we strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <span data-ttu-id="198d7-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>3. lépés.</span><span class="sxs-lookup"><span data-stu-id="198d7-269"><a name="copy-vhd-with-azcopy-or-powershell"></a>Step 3.</span></span> <span data-ttu-id="198d7-270">Másolja a VHD AzCopy vagy a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="198d7-270">Copy VHD with AzCopy or PowerShell</span></span>
<span data-ttu-id="198d7-271">A tároló elérési útja és a tárolási fiók kulcs tooprocess valamelyik két lehetőséget toofind lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="198d7-271">You will need toofind your container path and storage account key tooprocess either of these two options.</span></span> <span data-ttu-id="198d7-272">Tároló elérési útja és a tároló kulcsa megtalálható **Azure Portal** > **tárolási**.</span><span class="sxs-lookup"><span data-stu-id="198d7-272">Container path and storage account key can be found in **Azure Portal** > **Storage**.</span></span> <span data-ttu-id="198d7-273">hello tároló URL-címe például a "https://myaccount.blob.core.windows.net/mycontainer/" lesz.</span><span class="sxs-lookup"><span data-stu-id="198d7-273">hello container URL will be like "https://myaccount.blob.core.windows.net/mycontainer/".</span></span>

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a><span data-ttu-id="198d7-274">1. lehetőség: Az AzCopy (aszinkron másolhatja azokat) egy virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="198d7-274">Option 1: Copy a VHD with AzCopy (Asynchronous copy)</span></span>
<span data-ttu-id="198d7-275">Használja az AzCopy, egyszerűen feltöltheti hello VHD hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="198d7-275">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="198d7-276">Attól függően, hogy a virtuális merevlemezek hello hello méretét Ez időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="198d7-276">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="198d7-277">Ne felejtse el toocheck hello tárfiókok be-és kilépési korlátai, ha ezt a lehetőséget választja.</span><span class="sxs-lookup"><span data-stu-id="198d7-277">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="198d7-278">Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="198d7-278">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="198d7-279">Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="198d7-279">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="198d7-280">Nyissa meg az Azure PowerShell és amelyen telepítve van-e az AzCopy lépjen toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="198d7-280">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="198d7-281">Használjon hello következő parancsot a toocopy hello VHD-fájlt a "Forrás" túl "Cél".</span><span class="sxs-lookup"><span data-stu-id="198d7-281">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="198d7-282">Példa:</span><span class="sxs-lookup"><span data-stu-id="198d7-282">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd
    ```

    <span data-ttu-id="198d7-283">Az alábbiakban az AzCopy parancs hello használt hello paraméterek leírását:</span><span class="sxs-lookup"><span data-stu-id="198d7-283">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="198d7-284">**/ Forrás:  *&lt;forrás&gt;:***  hello mappa vagy hello VHD-t tartalmazó tároló tárolói URL.</span><span class="sxs-lookup"><span data-stu-id="198d7-284">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="198d7-285">**/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  hello forrás tárfiók Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="198d7-285">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="198d7-286">**/ Dest:  *&lt;cél&gt;:***  tárolási tároló URL-cím toocopy hello VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="198d7-286">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="198d7-287">**/ DestKey:  *&lt;cél fiókkulcs&gt;:***  hello céltárfiókot a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="198d7-287">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="198d7-288">**/ Mintát:  *&lt;Fájlnév&gt;:***  hello hello VHD toocopy a fájl nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="198d7-288">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="198d7-289">AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-289">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a><span data-ttu-id="198d7-290">2. lehetőség: A PowerShell használatával (Synchronized másolás) virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="198d7-290">Option 2: Copy a VHD with PowerShell (Synchronized copy)</span></span>
<span data-ttu-id="198d7-291">PowerShell-parancsmaggal hello Start-AzureStorageBlobCopy hello VHD-fájlt is másolhatja.</span><span class="sxs-lookup"><span data-stu-id="198d7-291">You can also copy hello VHD file using hello PowerShell cmdlet Start-AzureStorageBlobCopy.</span></span> <span data-ttu-id="198d7-292">Használja a következő parancs az Azure PowerShell toocopy VHD hello.</span><span class="sxs-lookup"><span data-stu-id="198d7-292">Use hello following command on Azure PowerShell toocopy VHD.</span></span> <span data-ttu-id="198d7-293">Cserélje le a <> hello értékei a forrás és cél tárfiók megfelelő értékeivel.</span><span class="sxs-lookup"><span data-stu-id="198d7-293">Replace hello values in <> with corresponding values from your source and destination storage account.</span></span> <span data-ttu-id="198d7-294">Ez a parancs, rendelkeznie kell a cél tárfiókkal nevezett VHD-k tárolója toouse.</span><span class="sxs-lookup"><span data-stu-id="198d7-294">toouse this command, you must have a container called vhds in your destination storage account.</span></span> <span data-ttu-id="198d7-295">Ha hello tároló nem létezik, hozzon létre egyet hello parancs futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="198d7-295">If hello container doesn't exist, create one before running hello command.</span></span>

```powershell
$sourceBlobUri = <source-vhd-uri>

$sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

$destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext
```

<span data-ttu-id="198d7-296">Példa:</span><span class="sxs-lookup"><span data-stu-id="198d7-296">Example:</span></span>

```powershell
C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext
```

### <span data-ttu-id="198d7-297"><a name="scenario2"></a>2. forgatókönyv: "I vagyok telepít virtuális gépeket más platformokon tooAzure prémium szintű Storage."</span><span class="sxs-lookup"><span data-stu-id="198d7-297"><a name="scenario2"></a>Scenario 2: "I am migrating VMs from other platforms tooAzure Premium Storage."</span></span>
<span data-ttu-id="198d7-298">Ha a virtuális merevlemez nem Azure felhőalapú tárolást tooAzure telepít, először exportálnia kell hello VHD tooa helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="198d7-298">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="198d7-299">Hello teljes forrás könyvtár elérési útja hello helyi virtuális merevlemez tároló lesz szüksége van, és akkor használja az AzCopy tooupload azt tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="198d7-299">Have hello complete source path of hello local directory where VHD is stored handy, and then using AzCopy tooupload it tooAzure Storage.</span></span>

#### <a name="step-1-export-vhd-tooa-local-directory"></a><span data-ttu-id="198d7-300">1. lépés</span><span class="sxs-lookup"><span data-stu-id="198d7-300">Step 1.</span></span> <span data-ttu-id="198d7-301">Exportálja a virtuális merevlemez tooa helyi könyvtár</span><span class="sxs-lookup"><span data-stu-id="198d7-301">Export VHD tooa local directory</span></span>
##### <a name="copy-a-vhd-from-aws"></a><span data-ttu-id="198d7-302">Másolja a VHD-t AWS</span><span class="sxs-lookup"><span data-stu-id="198d7-302">Copy a VHD from AWS</span></span>
1. <span data-ttu-id="198d7-303">Ha AWS használ, exportálja a hello EC2 példány tooa az Amazon S3 gyűjtő VHD-n.</span><span class="sxs-lookup"><span data-stu-id="198d7-303">If you are using AWS, export hello EC2 instance tooa VHD in an Amazon S3 bucket.</span></span> <span data-ttu-id="198d7-304">Hello exportálása Amazon EC2 példányok tooinstall hello Amazon EC2 parancssori felület (CLI) eszköz Amazon dokumentációjában leírt hello lépésekkel, és futtassa a hello-példány-export-feladat létrehozása parancs tooexport hello EC2 példány tooa VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="198d7-304">Follow hello steps described in hello Amazon documentation for Exporting Amazon EC2 Instances tooinstall hello Amazon EC2 command-line interface (CLI) tool and run hello create-instance-export-task command tooexport hello EC2 instance tooa VHD file.</span></span> <span data-ttu-id="198d7-305">Lehet, hogy toouse **VHD** hello lemez &#95; kép &#95; Hello futtatásakor formátum változó **-példány-export-feladat létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="198d7-305">Be sure toouse **VHD** for hello DISK&#95;IMAGE&#95;FORMAT variable when running hello **create-instance-export-task** command.</span></span> <span data-ttu-id="198d7-306">hello exportált VHD-fájl kerül, a folyamat során megadott hello Amazon S3 gyűjtő.</span><span class="sxs-lookup"><span data-stu-id="198d7-306">hello exported VHD file is saved in hello Amazon S3 bucket you designate during that process.</span></span>

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
      --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2. <span data-ttu-id="198d7-307">Hello S3 gyűjtő hello VHD-fájl letöltését.</span><span class="sxs-lookup"><span data-stu-id="198d7-307">Download hello VHD file from hello S3 bucket.</span></span> <span data-ttu-id="198d7-308">Jelölje be hello VHD-fájlt, majd **műveletek** > **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="198d7-308">Select hello VHD file, then **Actions** > **Download**.</span></span>

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a><span data-ttu-id="198d7-309">Másolja a VHD-t más-Azure felhő</span><span class="sxs-lookup"><span data-stu-id="198d7-309">Copy a VHD from other non-Azure cloud</span></span>
<span data-ttu-id="198d7-310">Ha a virtuális merevlemez nem Azure felhőalapú tárolást tooAzure telepít, először exportálnia kell hello VHD tooa helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="198d7-310">If you are migrating VHD from non-Azure Cloud Storage tooAzure, you must first export hello VHD tooa local directory.</span></span> <span data-ttu-id="198d7-311">Másolja a hello teljes forrás könyvtár elérési útja hello helyi virtuális merevlemez tárolásához.</span><span class="sxs-lookup"><span data-stu-id="198d7-311">Copy hello complete source path of hello local directory where VHD is stored.</span></span>

##### <a name="copy-a-vhd-from-on-premises"></a><span data-ttu-id="198d7-312">Másolja a VHD-t a helyszíni</span><span class="sxs-lookup"><span data-stu-id="198d7-312">Copy a VHD from on-premises</span></span>
<span data-ttu-id="198d7-313">Ha a VHD-t a helyszíni környezetben telepít, szüksége lesz a hello teljes forrás elérési útja a virtuális merevlemez tárolásához.</span><span class="sxs-lookup"><span data-stu-id="198d7-313">If you are migrating VHD from an on-premises environment, you will need hello complete source path where VHD is stored.</span></span> <span data-ttu-id="198d7-314">hello forrás elérési útja egy helyen vagy kiszolgálómegosztás lehet.</span><span class="sxs-lookup"><span data-stu-id="198d7-314">hello source path could be a server location or file share.</span></span>

#### <a name="step-2-create-hello-destination-for-your-vhd"></a><span data-ttu-id="198d7-315">2. lépés</span><span class="sxs-lookup"><span data-stu-id="198d7-315">Step 2.</span></span> <span data-ttu-id="198d7-316">Hello cél a virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d7-316">Create hello destination for your VHD</span></span>
<span data-ttu-id="198d7-317">Hozzon létre egy tárfiókot, a virtuális merevlemezek karbantartásához.</span><span class="sxs-lookup"><span data-stu-id="198d7-317">Create a storage account for maintaining your VHDs.</span></span> <span data-ttu-id="198d7-318">Vegye figyelembe a where megtervezésekor a következő pontok hello toostore merevlemezek:</span><span class="sxs-lookup"><span data-stu-id="198d7-318">Consider hello following points when planning where toostore your VHDs:</span></span>

* <span data-ttu-id="198d7-319">hello cél tárfiók lehet standard vagy prémium szintű storage, attól függően, hogy az alkalmazás követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="198d7-319">hello target storage account could be standard or premium storage depending on your application requirement.</span></span>
* <span data-ttu-id="198d7-320">hello tárfiók régiója meg kell egyeznie a prémium szintű Storage kompatibilis Azure virtuális gépek létrehozhat hello utolsó szakaszában.</span><span class="sxs-lookup"><span data-stu-id="198d7-320">hello storage account region must be same as Premium Storage capable Azure VMs you will create in hello final stage.</span></span> <span data-ttu-id="198d7-321">Új tárfiók tooa vagy terv toouse hello igényei szerint ugyanazt a tárfiókot nem átmásolni.</span><span class="sxs-lookup"><span data-stu-id="198d7-321">You could copy tooa new storage account, or plan toouse hello same storage account based on your needs.</span></span>
* <span data-ttu-id="198d7-322">Másolja ki és mentse a hello tárfiók kulcsa hello cél tárfiók hello következő szakaszra.</span><span class="sxs-lookup"><span data-stu-id="198d7-322">Copy and save hello storage account key of hello destination storage account for hello next stage.</span></span>

<span data-ttu-id="198d7-323">Határozottan javasoljuk, áthelyezése éles munkaterhelés toouse prémium szintű storage összes adatát.</span><span class="sxs-lookup"><span data-stu-id="198d7-323">We strongly recommend you moving all data for production workload toouse premium storage.</span></span>

#### <a name="step-3-upload-hello-vhd-tooazure-storage"></a><span data-ttu-id="198d7-324">3. lépés</span><span class="sxs-lookup"><span data-stu-id="198d7-324">Step 3.</span></span> <span data-ttu-id="198d7-325">Hello VHD tooAzure tárolási feltöltése</span><span class="sxs-lookup"><span data-stu-id="198d7-325">Upload hello VHD tooAzure Storage</span></span>
<span data-ttu-id="198d7-326">Most, hogy a virtuális merevlemez hello helyi könyvtárban, az AzCopy vagy AzurePowerShell tooupload hello .vhd fájl tooAzure tárolási is használhatja.</span><span class="sxs-lookup"><span data-stu-id="198d7-326">Now that you have your VHD in hello local directory, you can use AzCopy or AzurePowerShell tooupload hello .vhd file tooAzure Storage.</span></span> <span data-ttu-id="198d7-327">Mindkét lehetőség itt találhatók:</span><span class="sxs-lookup"><span data-stu-id="198d7-327">Both options are provided here:</span></span>

##### <a name="option-1-using-azure-powershell-add-azurevhd-tooupload-hello-vhd-file"></a><span data-ttu-id="198d7-328">1. lehetőség: Az Azure PowerShell Add-AzureVhd tooupload hello .vhd fájl használata</span><span class="sxs-lookup"><span data-stu-id="198d7-328">Option 1: Using Azure PowerShell Add-AzureVhd tooupload hello .vhd file</span></span>

```powershell
Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>
```

<span data-ttu-id="198d7-329">Példa <Uri> előfordulhat, hogy ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="198d7-329">An example <Uri> might be ***"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"***.</span></span> <span data-ttu-id="198d7-330">Példa <FileInfo> előfordulhat, hogy ***"C:\path\to\upload.vhd"***.</span><span class="sxs-lookup"><span data-stu-id="198d7-330">An example <FileInfo> might be ***"C:\path\to\upload.vhd"***.</span></span>

##### <a name="option-2-using-azcopy-tooupload-hello-vhd-file"></a><span data-ttu-id="198d7-331">2. lehetőség: Az AzCopy tooupload hello .vhd fájl használata</span><span class="sxs-lookup"><span data-stu-id="198d7-331">Option 2: Using AzCopy tooupload hello .vhd file</span></span>
<span data-ttu-id="198d7-332">Használja az AzCopy, egyszerűen feltöltheti hello VHD hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="198d7-332">Using AzCopy, you can easily upload hello VHD over hello Internet.</span></span> <span data-ttu-id="198d7-333">Attól függően, hogy a virtuális merevlemezek hello hello méretét Ez időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="198d7-333">Depending on hello size of hello VHDs, this may take time.</span></span> <span data-ttu-id="198d7-334">Ne felejtse el toocheck hello tárfiókok be-és kilépési korlátai, ha ezt a lehetőséget választja.</span><span class="sxs-lookup"><span data-stu-id="198d7-334">Remember toocheck hello storage account ingress/egress limits when using this option.</span></span> <span data-ttu-id="198d7-335">Lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="198d7-335">See [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md) for details.</span></span>

1. <span data-ttu-id="198d7-336">Töltse le és telepítse az AzCopy innen: [az AzCopy legújabb verzióját](http://aka.ms/downloadazcopy)</span><span class="sxs-lookup"><span data-stu-id="198d7-336">Download and install AzCopy from here: [Latest version of AzCopy](http://aka.ms/downloadazcopy)</span></span>
2. <span data-ttu-id="198d7-337">Nyissa meg az Azure PowerShell és amelyen telepítve van-e az AzCopy lépjen toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="198d7-337">Open Azure PowerShell and go toohello folder where AzCopy is installed.</span></span>
3. <span data-ttu-id="198d7-338">Használjon hello következő parancsot a toocopy hello VHD-fájlt a "Forrás" túl "Cél".</span><span class="sxs-lookup"><span data-stu-id="198d7-338">Use hello following command toocopy hello VHD file from "Source" too"Destination".</span></span>

    ```azcopy
    AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>
    ```

    <span data-ttu-id="198d7-339">Példa:</span><span class="sxs-lookup"><span data-stu-id="198d7-339">Example:</span></span>

    ```azcopy
    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd
    ```

    <span data-ttu-id="198d7-340">Az alábbiakban az AzCopy parancs hello használt hello paraméterek leírását:</span><span class="sxs-lookup"><span data-stu-id="198d7-340">Here are descriptions of hello parameters used in hello AzCopy command:</span></span>

   * <span data-ttu-id="198d7-341">**/ Forrás:  *&lt;forrás&gt;:***  hello mappa vagy hello VHD-t tartalmazó tároló tárolói URL.</span><span class="sxs-lookup"><span data-stu-id="198d7-341">**/Source: *&lt;source&gt;:*** Location of hello folder or storage container URL that contains hello VHD.</span></span>
   * <span data-ttu-id="198d7-342">**/ SourceKey:  *&lt;forrás fiókkulcs&gt;:***  hello forrás tárfiók Tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="198d7-342">**/SourceKey: *&lt;source-account-key&gt;:*** Storage account key of hello source storage account.</span></span>
   * <span data-ttu-id="198d7-343">**/ Dest:  *&lt;cél&gt;:***  tárolási tároló URL-cím toocopy hello VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="198d7-343">**/Dest: *&lt;destination&gt;:*** Storage container URL toocopy hello VHD to.</span></span>
   * <span data-ttu-id="198d7-344">**/ DestKey:  *&lt;cél fiókkulcs&gt;:***  hello céltárfiókot a Tárfiók kulcsára.</span><span class="sxs-lookup"><span data-stu-id="198d7-344">**/DestKey: *&lt;dest-account-key&gt;:*** Storage account key of hello destination storage account.</span></span>
   * <span data-ttu-id="198d7-345">**/ BlobType: lap:** határozza meg, hogy hello cél oldalakra vonatkozó blob.</span><span class="sxs-lookup"><span data-stu-id="198d7-345">**/BlobType: page:** Specifies that hello destination is a page blob.</span></span>
   * <span data-ttu-id="198d7-346">**/ Mintát:  *&lt;Fájlnév&gt;:***  hello hello VHD toocopy a fájl nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="198d7-346">**/Pattern: *&lt;file-name&gt;:*** Specify hello file name of hello VHD toocopy.</span></span>

<span data-ttu-id="198d7-347">AzCopy használatával eszköz, lásd: [adatátvitel az AzCopy parancssori segédprogram hello](storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="198d7-347">For details on using AzCopy tool, see [Transfer data with hello AzCopy Command-Line Utility](storage-use-azcopy.md).</span></span>

##### <a name="other-options-for-uploading-a-vhd"></a><span data-ttu-id="198d7-348">Más beállításokat a virtuális merevlemez feltöltése</span><span class="sxs-lookup"><span data-stu-id="198d7-348">Other options for uploading a VHD</span></span>
<span data-ttu-id="198d7-349">Feltöltheti a virtuális merevlemez tooyour tárfiók azt jelenti, hogy a következő hello egyikének használatával:</span><span class="sxs-lookup"><span data-stu-id="198d7-349">You can also upload a VHD tooyour storage account using one of hello following means:</span></span>

* [<span data-ttu-id="198d7-350">Az Azure Storage másolási Blob API</span><span class="sxs-lookup"><span data-stu-id="198d7-350">Azure Storage Copy Blob API</span></span>](https://msdn.microsoft.com/library/azure/dd894037.aspx)
* [<span data-ttu-id="198d7-351">Az Azure Storage Explorer feltöltése a BLOB</span><span class="sxs-lookup"><span data-stu-id="198d7-351">Azure Storage Explorer Uploading Blobs</span></span>](https://azurestorageexplorer.codeplex.com/)
* [<span data-ttu-id="198d7-352">Storage Import/Export szolgáltatás REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="198d7-352">Storage Import/Export Service REST API Reference</span></span>](https://msdn.microsoft.com/library/dn529096.aspx)

> [!NOTE]
> <span data-ttu-id="198d7-353">Ajánlott Import/Export szolgáltatás használata, ha becsült a 7 napnál hosszabb idő feltöltése.</span><span class="sxs-lookup"><span data-stu-id="198d7-353">We recommend using Import/Export Service if estimated uploading time is longer than 7 days.</span></span> <span data-ttu-id="198d7-354">Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) adatok méretét és átviteli egység tooestimate hello időpontját.</span><span class="sxs-lookup"><span data-stu-id="198d7-354">You can use [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello time from data size and transfer unit.</span></span>
>
> <span data-ttu-id="198d7-355">Importálási/exportálási lehet toocopy tooa standard szintű tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="198d7-355">Import/Export can be used toocopy tooa standard storage account.</span></span> <span data-ttu-id="198d7-356">Standard szintű tárolást toopremium tárkonfigurációt AzCopy hasonló eszköz használatával toocopy kell.</span><span class="sxs-lookup"><span data-stu-id="198d7-356">You will need toocopy from standard storage toopremium storage account using a tool like AzCopy.</span></span>
>
>

## <span data-ttu-id="198d7-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Prémium szintű Storage használata Azure virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d7-357"><a name="create-azure-virtual-machine-using-premium-storage"></a>Create Azure VMs using Premium Storage</span></span>
<span data-ttu-id="198d7-358">Miután hello VHD szükséges feltöltött vagy másolt toohello tárfiókot, ez a szakasz tooregister hello VHD hello utasításait kövesse az operációsrendszer-lemezképek, vagy a forgatókönyvtől függően az operációsrendszer-lemez, és egy Virtuálisgép-példány készítése.</span><span class="sxs-lookup"><span data-stu-id="198d7-358">After hello VHD is uploaded or copied toohello desired storage account, follow hello instructions in this section tooregister hello VHD as an OS image, or OS disk depending on your scenario and then create a VM instance from it.</span></span> <span data-ttu-id="198d7-359">hello adatlemez VHD lehet csatolt toohello virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="198d7-359">hello data disk VHD can be attached toohello VM once it is created.</span></span>
<span data-ttu-id="198d7-360">Ez a szakasz hello végén egy áttelepítési parancsfájlt valósul meg.</span><span class="sxs-lookup"><span data-stu-id="198d7-360">A sample migration script is provided at hello end of this section.</span></span> <span data-ttu-id="198d7-361">Ez egyszerű parancsprogram nem felel meg minden forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="198d7-361">This simple script does not match all scenarios.</span></span> <span data-ttu-id="198d7-362">Az adott forgatókönyv szükség lehet tooupdate hello parancsfájl toomatch.</span><span class="sxs-lookup"><span data-stu-id="198d7-362">You may need tooupdate hello script toomatch with your specific scenario.</span></span> <span data-ttu-id="198d7-363">Ha ezt a parancsfájlt érvényes tooyour esetben toosee lásd az alábbi [A Parancsfájlpéldát áttelepítési](#a-sample-migration-script).</span><span class="sxs-lookup"><span data-stu-id="198d7-363">toosee if this script applies tooyour scenario, see below [A Sample Migration Script](#a-sample-migration-script).</span></span>

### <a name="checklist"></a><span data-ttu-id="198d7-364">Feladatlista</span><span class="sxs-lookup"><span data-stu-id="198d7-364">Checklist</span></span>
1. <span data-ttu-id="198d7-365">Várja meg, amíg az összes hello VHD lemezek másolása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="198d7-365">Wait until all hello VHD disks copying is complete.</span></span>
2. <span data-ttu-id="198d7-366">Ellenőrizze, hogy hello régió áttelepítés prémium szintű Storage áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="198d7-366">Make sure Premium Storage is available in hello region you are migrating to.</span></span>
3. <span data-ttu-id="198d7-367">Döntse el, hello új Virtuálisgép-sorozat fog használni.</span><span class="sxs-lookup"><span data-stu-id="198d7-367">Decide hello new VM series you will be using.</span></span> <span data-ttu-id="198d7-368">Egy prémium szintű Storage képes legyen, és hello mérete attól függően, hello rendelkezésre állási hello régióban és igényei alapján kell.</span><span class="sxs-lookup"><span data-stu-id="198d7-368">It should be a Premium Storage capable, and hello size should be depending on hello availability in hello region and based on your needs.</span></span>
4. <span data-ttu-id="198d7-369">Döntse el, hello pontos Virtuálisgép-méretet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="198d7-369">Decide hello exact VM size you will use.</span></span> <span data-ttu-id="198d7-370">Virtuálisgép-méretet kell toobe elég nagy toosupport hello rendelkezik adatlemezek száma.</span><span class="sxs-lookup"><span data-stu-id="198d7-370">VM size needs toobe large enough toosupport hello number of data disks you have.</span></span> <span data-ttu-id="198d7-371">Például</span><span class="sxs-lookup"><span data-stu-id="198d7-371">E.g.</span></span> <span data-ttu-id="198d7-372">Ha 4 adatlemezek, hello VM 2 vagy több maggal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="198d7-372">if you have 4 data disks, hello VM must have 2 or more cores.</span></span> <span data-ttu-id="198d7-373">Fontolja meg is, a feldolgozási kapacitása, memória, és a hálózati sávszélesség igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="198d7-373">Also, consider processing power, memory and network bandwidth needs.</span></span>
5. <span data-ttu-id="198d7-374">Prémium szintű Storage-fiók létrehozása hello cél régióban.</span><span class="sxs-lookup"><span data-stu-id="198d7-374">Create a Premium Storage account in hello target region.</span></span> <span data-ttu-id="198d7-375">A rendszer a hello fiókot használja-e hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="198d7-375">This is hello account you will use for hello new VM.</span></span>
6. <span data-ttu-id="198d7-376">Hello aktuális virtuális gép adatai lesz szüksége, beleértve a lemezek és a megfelelő VHD-blobok hello listája rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="198d7-376">Have hello current VM details handy, including hello list of disks and corresponding VHD blobs.</span></span>

<span data-ttu-id="198d7-377">Készítse elő az állásidő alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="198d7-377">Prepare your application for downtime.</span></span> <span data-ttu-id="198d7-378">egy tiszta áttelepítési toodo, vannak toostop összes hello feldolgozási hello aktuális rendszer.</span><span class="sxs-lookup"><span data-stu-id="198d7-378">toodo a clean migration, you have toostop all hello processing in hello current system.</span></span> <span data-ttu-id="198d7-379">Csak ezután beszerezheti tooconsistent állapotát, amely áttelepíthető toohello új platformon.</span><span class="sxs-lookup"><span data-stu-id="198d7-379">Only then you can get it tooconsistent state which you can migrate toohello new platform.</span></span> <span data-ttu-id="198d7-380">Állásidő időtartama hello adatmennyiséget a hello lemezek toomigrate függ.</span><span class="sxs-lookup"><span data-stu-id="198d7-380">Downtime duration will depend on hello amount of data in hello disks toomigrate.</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-381">Az Azure Resource Manager virtuális gép speciális VHD lemez létrehozásakor, tekintse meg túl[sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) erőforrás-kezelő virtuális gépet a meglévő lemezt telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="198d7-381">If you are creating an Azure Resource Manager VM from a specialized VHD Disk, please refer too[this template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) for deploying Resource Manager VM using existing disk.</span></span>
>
>

### <a name="register-your-vhd"></a><span data-ttu-id="198d7-382">A virtuális merevlemez regisztrálása</span><span class="sxs-lookup"><span data-stu-id="198d7-382">Register your VHD</span></span>
<span data-ttu-id="198d7-383">a virtuális gép operációs rendszer virtuális Merevlemezt vagy egy adatok lemez tooa tooattach toocreate új virtuális Gépet, először regisztrálnia kell őket.</span><span class="sxs-lookup"><span data-stu-id="198d7-383">toocreate a VM from OS VHD or tooattach a data disk tooa new VM, you must first register them.</span></span> <span data-ttu-id="198d7-384">Kövesse az alábbi lépéseket attól függően, hogy a VHD-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="198d7-384">Follow steps below depending on your VHD's scenario.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="198d7-385">Operációs rendszer virtuális merevlemez toocreate általánosítva több Azure Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="198d7-385">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="198d7-386">Az operációsrendszer-lemezképek általánosított virtuális merevlemez feltöltése toohello tárfiókot, után regisztrálja azt egy **Azure Virtuálisgép-lemezkép** , hogy egy vagy több Virtuálisgép-példányok hozhat létre belőle.</span><span class="sxs-lookup"><span data-stu-id="198d7-386">After generalized OS image VHD is uploaded toohello storage account, register it as an **Azure VM Image** so that you can create one or more VM instances from it.</span></span> <span data-ttu-id="198d7-387">A következő PowerShell-parancsmagok tooregister hello a VHD-t használja egy Azure virtuális gép operációsrendszer-lemezképben.</span><span class="sxs-lookup"><span data-stu-id="198d7-387">Use hello following PowerShell cmdlets tooregister your VHD as an Azure VM OS image.</span></span> <span data-ttu-id="198d7-388">Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="198d7-388">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows
```

<span data-ttu-id="198d7-389">Másolja ki és mentse az új Azure Virtuálisgép-lemezkép hello nevét.</span><span class="sxs-lookup"><span data-stu-id="198d7-389">Copy and save hello name of this new Azure VM Image.</span></span> <span data-ttu-id="198d7-390">Hello a fenti példában az is *OSImageName*.</span><span class="sxs-lookup"><span data-stu-id="198d7-390">In hello example above, it is *OSImageName*.</span></span>

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="198d7-391">Egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Azure Virtuálisgép-példány</span><span class="sxs-lookup"><span data-stu-id="198d7-391">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="198d7-392">Hello után egyedi az operációs rendszer virtuális merevlemez az feltöltött toohello tároló fiókot, regisztrálja őket, mint egy **Azure operációsrendszer-lemez** , hogy egy Virtuálisgép-példányt hozhat létre belőle.</span><span class="sxs-lookup"><span data-stu-id="198d7-392">After hello unique OS VHD is uploaded toohello storage account, register it as an **Azure OS Disk** so that you can create a VM instance from it.</span></span> <span data-ttu-id="198d7-393">A virtuális merevlemez az alábbi PowerShell-parancsmagok tooregister használja Azure operációsrendszer-lemezként.</span><span class="sxs-lookup"><span data-stu-id="198d7-393">Use these PowerShell cmdlets tooregister your VHD as an Azure OS Disk.</span></span> <span data-ttu-id="198d7-394">Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="198d7-394">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"
```

<span data-ttu-id="198d7-395">Másolja ki és mentse az új Azure operációsrendszer-lemez hello nevét.</span><span class="sxs-lookup"><span data-stu-id="198d7-395">Copy and save hello name of this new Azure OS Disk.</span></span> <span data-ttu-id="198d7-396">Hello a fenti példában az is *OSDisk*.</span><span class="sxs-lookup"><span data-stu-id="198d7-396">In hello example above, it is *OSDisk*.</span></span>

#### <a name="data-disk-vhd-toobe-attached-toonew-azure-vm-instances"></a><span data-ttu-id="198d7-397">Adatok lemez VHD toobe csatolt toonew Azure Virtuálisgép-példányokat</span><span class="sxs-lookup"><span data-stu-id="198d7-397">Data disk VHD toobe attached toonew Azure VM instance(s)</span></span>
<span data-ttu-id="198d7-398">Hello virtuális merevlemez adatlemeze feltöltése után toostorage fiók, regisztrálja egy Azure-adatlemez, így azok csatolt tooyour új DS méretek, DSv2-méretek és GS adatsorozat Azure Virtuálisgép-példány.</span><span class="sxs-lookup"><span data-stu-id="198d7-398">After hello data disk VHD is uploaded toostorage account, register it as an Azure Data Disk so that it can be attached tooyour new DS Series, DSv2 series or GS Series Azure VM instance.</span></span>

<span data-ttu-id="198d7-399">A PowerShell-parancsmagok tooregister a VHD-t használja az Azure-adatlemez.</span><span class="sxs-lookup"><span data-stu-id="198d7-399">Use these PowerShell cmdlets tooregister your VHD as an Azure Data Disk.</span></span> <span data-ttu-id="198d7-400">Adja meg a hello teljes tároló URL-CÍMÉT adott VHD lett másolva.</span><span class="sxs-lookup"><span data-stu-id="198d7-400">Provide hello complete container URL where VHD was copied to.</span></span>

```powershell
Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"
```

<span data-ttu-id="198d7-401">Másolja ki és mentse az új Azure-adatlemez hello nevét.</span><span class="sxs-lookup"><span data-stu-id="198d7-401">Copy and save hello name of this new Azure Data Disk.</span></span> <span data-ttu-id="198d7-402">Hello a fenti példában az is *DataDisk*.</span><span class="sxs-lookup"><span data-stu-id="198d7-402">In hello example above, it is *DataDisk*.</span></span>

### <a name="create-a-premium-storage-capable-vm"></a><span data-ttu-id="198d7-403">Prémium szintű Storage képes a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d7-403">Create a Premium Storage capable VM</span></span>
<span data-ttu-id="198d7-404">Az operációsrendszer-lemezképek egyszer hello vagy operációsrendszer-lemez van regisztrálva, hozzon létre egy új DS-méretek, DSv2-sorozat vagy GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="198d7-404">Once hello OS image or OS disk are registered, create a new DS-series, DSv2-series or GS-series VM.</span></span> <span data-ttu-id="198d7-405">T hello operációs rendszeri lemezkép vagy operációs rendszer Lemeznév regisztrált fog használni.</span><span class="sxs-lookup"><span data-stu-id="198d7-405">You will be using hello operating system image or operating system disk name that you registered.</span></span> <span data-ttu-id="198d7-406">Válassza ki a Virtuálisgép-típussá hello hello prémium szintű Storage réteg alapján.</span><span class="sxs-lookup"><span data-stu-id="198d7-406">Select hello VM type from hello Premium Storage tier.</span></span> <span data-ttu-id="198d7-407">Az alábbi példában használjuk hello *Standard_DS2* Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="198d7-407">In example below, we are using hello *Standard_DS2* VM size.</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-408">Frissítés hello lemez mérete toomake meg arról, hogy megegyezzen a kapacitás és a teljesítményre vonatkozó követelmények és a hello Azure rendelkezésre álló lemezterület méretét.</span><span class="sxs-lookup"><span data-stu-id="198d7-408">Update hello disk size toomake sure it matches your capacity and performance requirements and hello available Azure disk sizes.</span></span>
>
>

<span data-ttu-id="198d7-409">Hajtsa végre hello lépésről lépésre PowerShell-parancsmagok alatt toocreate hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="198d7-409">Follow hello step by step PowerShell cmdlets below toocreate hello new VM.</span></span> <span data-ttu-id="198d7-410">Első lépésként állítsa be az általános paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="198d7-410">First, set hello common parameters:</span></span>

```powershell
$serviceName = "yourVM"
$location = "location-name" (e.g., West US)
$vmSize ="Standard_DS2"
$adminUser = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$vmSize = "Standard_DS2"
```

<span data-ttu-id="198d7-411">Először hozzon létre egy felhőalapú szolgáltatás, amelyben, amelyen az új virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="198d7-411">First, create a cloud service in which you will be hosting your new VMs.</span></span>

```powershell
New-AzureService -ServiceName $serviceName -Location $location
```

<span data-ttu-id="198d7-412">Ezután a forgatókönyvtől függően hozzon létre hello Azure Virtuálisgép-példány vagy hello operációsrendszer-lemezképek vagy regisztrált operációsrendszer-lemezt.</span><span class="sxs-lookup"><span data-stu-id="198d7-412">Next, depending on your scenario, create hello Azure VM instance from either hello OS Image or OS Disk that you registered.</span></span>

#### <a name="generalized-operating-system-vhd-toocreate-multiple-azure-vm-instances"></a><span data-ttu-id="198d7-413">Operációs rendszer virtuális merevlemez toocreate általánosítva több Azure Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="198d7-413">Generalized Operating System VHD toocreate multiple Azure VM instances</span></span>
<span data-ttu-id="198d7-414">Hozzon létre egy vagy több új DS adatsorozat Azure Virtuálisgép-példányok hello használata az hello **Azure operációsrendszer-lemezképek** regisztrált.</span><span class="sxs-lookup"><span data-stu-id="198d7-414">Create hello one or more new DS Series Azure VM instances using hello **Azure OS Image** that you registered.</span></span> <span data-ttu-id="198d7-415">Adja meg a operációsrendszer-lemezképek nevét hello Virtuálisgép-konfiguráció, amikor új virtuális gép létrehozása az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="198d7-415">Specify this OS Image name in hello VM configuration when creating new VM as shown below.</span></span>

```powershell
$OSImage = Get-AzureVMImage –ImageName "OSImageName"

$vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

New-AzureVM -ServiceName $serviceName -VM $vm
```

#### <a name="unique-operating-system-vhd-toocreate-a-single-azure-vm-instance"></a><span data-ttu-id="198d7-416">Egyedi operációs rendszer virtuális merevlemez toocreate egyetlen Azure Virtuálisgép-példány</span><span class="sxs-lookup"><span data-stu-id="198d7-416">Unique Operating System VHD toocreate a single Azure VM instance</span></span>
<span data-ttu-id="198d7-417">Hozzon létre új DS adatsorozat Azure virtuális példányt hello segítségével **Azure operációsrendszer-lemez** regisztrált.</span><span class="sxs-lookup"><span data-stu-id="198d7-417">Create a new DS series Azure VM instance using hello **Azure OS Disk** that you registered.</span></span> <span data-ttu-id="198d7-418">Adja meg az operációs rendszer lemezének neve hello Virtuálisgép-konfiguráció, amikor új virtuális gép létrehozása hello alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="198d7-418">Specify this OS Disk name in hello VM configuration when creating hello new VM as shown below.</span></span>

```powershell
$OSDisk = Get-AzureDisk –DiskName "OSDisk"

$vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

New-AzureVM -ServiceName $serviceName –VM $vm
```

<span data-ttu-id="198d7-419">Adjon meg más Azure Virtuálisgép-adatok, például egy felhőalapú szolgáltatás, régió, tárfiókot, a rendelkezésre állási csoport és gyorsítótárazási házirend.</span><span class="sxs-lookup"><span data-stu-id="198d7-419">Specify other Azure VM information, such as a cloud service, region, storage account, availability set, and caching policy.</span></span> <span data-ttu-id="198d7-420">Vegye figyelembe, hogy hello Virtuálisgép-példány közös elhelyezésű társított operációs rendszerrel kell lennie, vagy adatlemezek, így hello kijelölt felhőalapú szolgáltatás, valamint régió és tárolási fiók kell lenniük hello az alapul szolgáló virtuális merevlemezek lemezek hello és ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="198d7-420">Note that hello VM instance must be co-located with associated operating system or data disks, so hello selected cloud service, region and storage account must all be in hello same location as hello underlying VHDs of those disks.</span></span>

### <a name="attach-data-disk"></a><span data-ttu-id="198d7-421">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="198d7-421">Attach data disk</span></span>
<span data-ttu-id="198d7-422">Végül, ha regisztrált adatok lemez VHD-k, csatolja őket toohello új prémium szintű Storage kompatibilis Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="198d7-422">Lastly, if you have registered data disk VHDs, attach them toohello new Premium Storage capable Azure VM.</span></span>

<span data-ttu-id="198d7-423">Használja a következő PowerShell parancsmagot tooattach adatok lemez toohello új virtuális Gépet, és adja meg a gyorsítótárazási házirend hello.</span><span class="sxs-lookup"><span data-stu-id="198d7-423">Use following PowerShell cmdlet tooattach data disk toohello new VM and specify hello caching policy.</span></span> <span data-ttu-id="198d7-424">Hello az alábbi példában a gyorsítótárazási házirend értéke túl*ReadOnly*.</span><span class="sxs-lookup"><span data-stu-id="198d7-424">In example below hello caching policy is set too*ReadOnly*.</span></span>

```powershell
$vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

Update-AzureVM  -VM $vm
```

> [!NOTE]
> <span data-ttu-id="198d7-425">Előfordulhat, hogy további lépéseket szükséges toosupport az alkalmazás, amely nem elegendő az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="198d7-425">There may be additional steps necessary toosupport your application that is not be covered by this guide.</span></span>
>
>

### <a name="checking-and-plan-backup"></a><span data-ttu-id="198d7-426">Ellenőrzése és a biztonsági mentés tervezése</span><span class="sxs-lookup"><span data-stu-id="198d7-426">Checking and plan backup</span></span>
<span data-ttu-id="198d7-427">Egyszer hello új virtuális gép működik-e és fut, a hozzáférés segítségével hello azonos felhasználónév és jelszó megegyezik az eredeti virtuális gép hello, és győződjön meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="198d7-427">Once hello new VM is up and running, access it using hello same login id and password is as hello original VM, and verify that everything is working as expected.</span></span> <span data-ttu-id="198d7-428">Összes hello beállításának, köztük a hello csíkozott kötetek jelen lehet új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="198d7-428">All hello settings, including hello striped volumes, would be present in hello new VM.</span></span>

<span data-ttu-id="198d7-429">hello utolsó lépése tooplan biztonsági mentésének és karbantartásának ütemezése hello hello alkalmazásnak alapuló új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="198d7-429">hello last step is tooplan backup and maintenance schedule for hello new VM based on hello application's needs.</span></span>

### <span data-ttu-id="198d7-430"><a name="a-sample-migration-script"></a>Egy áttelepítési parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="198d7-430"><a name="a-sample-migration-script"></a>A sample migration script</span></span>
<span data-ttu-id="198d7-431">Ha több virtuális gépek toomigrate, automatizálás PowerShell-parancsprogramok hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="198d7-431">If you have multiple VMs toomigrate, automation through PowerShell scripts will be helpful.</span></span> <span data-ttu-id="198d7-432">Az alábbiakban látható egy minta parancsfájlt, amely automatizálja a virtuális gép hello áttelepítését.</span><span class="sxs-lookup"><span data-stu-id="198d7-432">Following is a sample script that automates hello migration of a VM.</span></span> <span data-ttu-id="198d7-433">Megjegyzés: alább parancsfájl, amely csak egy példa, és nincsenek hello aktuális virtuális gép lemezeivel kapcsolatos tett néhány feltételezéseket.</span><span class="sxs-lookup"><span data-stu-id="198d7-433">Note that below script is only an example, and there are few assumptions made about hello current VM disks.</span></span> <span data-ttu-id="198d7-434">Az adott forgatókönyv szükség lehet tooupdate hello parancsfájl toomatch.</span><span class="sxs-lookup"><span data-stu-id="198d7-434">You may need tooupdate hello script toomatch with your specific scenario.</span></span>

<span data-ttu-id="198d7-435">hello Előfeltételek a következők:</span><span class="sxs-lookup"><span data-stu-id="198d7-435">hello assumptions are:</span></span>

* <span data-ttu-id="198d7-436">Klasszikus Azure virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="198d7-436">You are creating classic Azure VMs.</span></span>
* <span data-ttu-id="198d7-437">A forrás operációs rendszer és a forrás adatok lemezek vannak ugyanabban a tárfiókban és az ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="198d7-437">Your source OS Disks and source Data Disks are in same storage account and same container.</span></span> <span data-ttu-id="198d7-438">Ha az operációs rendszer és a lemezek adatok nincsenek az azonos hello helyezze, használhatja a storage-fiókok és a tárolók AzCopy vagy az Azure PowerShell toocopy virtuális merevlemezeket.</span><span class="sxs-lookup"><span data-stu-id="198d7-438">If your OS Disks and Data Disks are not in hello same place, you can use AzCopy or Azure PowerShell toocopy VHDs over storage accounts and containers.</span></span> <span data-ttu-id="198d7-439">Tekintse meg az előző lépésben toohello: [másolási VHD AzCopy vagy a PowerShell használatával](#copy-vhd-with-azcopy-or-powershell).</span><span class="sxs-lookup"><span data-stu-id="198d7-439">Refer toohello previous step: [Copy VHD with AzCopy or PowerShell](#copy-vhd-with-azcopy-or-powershell).</span></span> <span data-ttu-id="198d7-440">A parancsfájl toomeet szerkesztése adott esetben más választási lehetőség, de azt javasoljuk, mert az egyszerűbb és gyorsabb AzCopy vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="198d7-440">Editing this script toomeet your scenario is another choice, but we recommend using AzCopy or PowerShell since it is easier and faster.</span></span>

<span data-ttu-id="198d7-441">hello automatizálási parancsfájl lejjebb tekinthetők meg.</span><span class="sxs-lookup"><span data-stu-id="198d7-441">hello automation script is provided below.</span></span> <span data-ttu-id="198d7-442">Szöveg cseréje az adatait, és frissítse az Ön konkrét forgatókönyvei hello parancsfájl toomatch.</span><span class="sxs-lookup"><span data-stu-id="198d7-442">Replace text with your information and update hello script toomatch with your specific scenario.</span></span>

> [!NOTE]
> <span data-ttu-id="198d7-443">Hello meglévő parancsfájl használatával nem őrzi meg a virtuális gép – forrásként hello hálózati konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="198d7-443">Using hello existing script does not preserve hello network configuration of your source VM.</span></span> <span data-ttu-id="198d7-444">Toore-config hello hálózati beállításokat kell az áttelepített virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="198d7-444">You will need toore-config hello networking settings on your migrated VMs.</span></span>
>
>

```
    <#
    .Synopsis
    This script is provided as an EXAMPLE tooshow how toomigrate a VM from a standard storage account tooa premium storage account. You can customize it according tooyour specific requirements.

    .Description
    hello script will copy hello vhds (page blobs) of hello source VM toohello new storage account.
    And then it will create a new VM from these copied vhds based on hello inputs that you specified for hello new VM.
    You can modify hello script toosatisfy your specific requirement, but please be aware of hello items specified
    in hello Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED toohello IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. hello ENTIRE RISK OF USE, INABILITY tooUSE, OR
    RESULTS FROM hello USE OF THIS CODE REMAINS WITH hello USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location "Southeast Asia"

    .Link
    toofind more information about how tooset up Azure PowerShell, refer toohello following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # hello cloud service name of hello VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # hello VM name toocopy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # hello destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # hello destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # hello destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # hello destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # hello location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not toocopy hello os disk, hello default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently tooreport hello copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # hello name suffix tooadd toonew created disks tooavoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import hello Azure PowerShell module
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


    #Check hello Azure PowerShell module version
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
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer toothis article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ tooconnect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if hello VM is shut down
    #  Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - hello source VM doesn't exist in hello current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping hello VM is a required step so that hello file system is consistent when you do hello copy operation. Azure does not support live migration at this time. If you'd like toocreate a VM from a generalized image, sys-prep hello Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish toostop $SourceVMName now? Input 'N' if you want tooshut down hello VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until hello VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName tooshut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting hello sourve vm tooa configuration file, you can restore hello original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration too$vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy hello vhds of hello source vm
    #  You can choose toocopy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly toobe $false. hello default is toocopy only data disk vhds
    #  and hello new VM will boot from hello original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering hello data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in hello destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need toocopy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting hello source VM so that dest VM can boot
        # from hello same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose toocopy data disks only. Moving VM requires removing hello original VM (hello disks and backing vhd files will NOT be deleted) so that hello new VM can boot from hello same vhd. This is an irreversible action. Do you wish tooproceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing hello original VM (hello vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill hello OS disk is released by source VM. This may take up tooseveral minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy hello os disk vhd
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
        # update hello media link toopoint toohello target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy toocomplete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
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
    #  hello new VM can be created from hello copied disks or hello original os disk.
    #  You can ddd your own logic here toosatisfy your specific requirements of hello vm.
    #######################################################################

    # Create a VM from hello existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached hello copied data disks toohello new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of hello source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want tooadd more custimization toohello new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location
```

#### <span data-ttu-id="198d7-445"><a name="optimization"></a>Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="198d7-445"><a name="optimization"></a>Optimization</span></span>
<span data-ttu-id="198d7-446">Az aktuális Virtuálisgép-konfigurációt testre szabható kifejezetten toowork jól igazodik a Standard lemezek.</span><span class="sxs-lookup"><span data-stu-id="198d7-446">Your current VM configuration may be customized specifically toowork well with Standard disks.</span></span> <span data-ttu-id="198d7-447">Például tooincrease hello teljesítmény sok csíkozott kötetek használatával.</span><span class="sxs-lookup"><span data-stu-id="198d7-447">For instance, tooincrease hello performance by using many disks in a striped volume.</span></span> <span data-ttu-id="198d7-448">Például helyett 4 lemezek külön-külön a prémium szintű Storage, előfordulhat, hogy el tudja toooptimize hello költség azzal, hogy egyetlen lemez.</span><span class="sxs-lookup"><span data-stu-id="198d7-448">For example, instead of using 4 disks separately on Premium Storage, you may be able toooptimize hello cost by having a single disk.</span></span> <span data-ttu-id="198d7-449">Optimalizálás, például a kezelt eseti alapon kell toobe, és egyéni lépéseket igényelnek a hello áttelepítés után.</span><span class="sxs-lookup"><span data-stu-id="198d7-449">Optimizations like this need toobe handled on a case by case basis and require custom steps after hello migration.</span></span> <span data-ttu-id="198d7-450">Emellett vegye figyelembe, hogy ez a folyamat jól nem feltétlenül alkalmas adatbázisok és hello lemez elrendezése hello beállítása meghatározott függő alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="198d7-450">Also, note that this process may not well work for databases and applications that depend on hello disk layout defined in hello setup.</span></span>

##### <a name="preparation"></a><span data-ttu-id="198d7-451">Előkészítése</span><span class="sxs-lookup"><span data-stu-id="198d7-451">Preparation</span></span>
1. <span data-ttu-id="198d7-452">Teljes hello egyszerű áttelepítési hello a korábbi szakaszban.</span><span class="sxs-lookup"><span data-stu-id="198d7-452">Complete hello Simple Migration as described in hello earlier section.</span></span> <span data-ttu-id="198d7-453">Hello történik optimalizálás hello áttelepítés után új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="198d7-453">Optimizations will be performed on hello new VM after hello migration.</span></span>
2. <span data-ttu-id="198d7-454">Adja meg a hello optimalizált hello konfigurálásához szükséges új lemez méretét.</span><span class="sxs-lookup"><span data-stu-id="198d7-454">Define hello new disk sizes needed for hello optimized configuration.</span></span>
3. <span data-ttu-id="198d7-455">Leképezési hello aktuális lemezek vagy kötetek toohello új lemez előírások határozza meg.</span><span class="sxs-lookup"><span data-stu-id="198d7-455">Determine mapping of hello current disks/volumes toohello new disk specifications.</span></span>

##### <a name="execution-steps"></a><span data-ttu-id="198d7-456">Végrehajtási lépések</span><span class="sxs-lookup"><span data-stu-id="198d7-456">Execution steps</span></span>
1. <span data-ttu-id="198d7-457">Hozzon létre hello új lemezek hello megfelelő méretek a prémium szintű Storage VM hello.</span><span class="sxs-lookup"><span data-stu-id="198d7-457">Create hello new disks with hello right sizes on hello Premium Storage VM.</span></span>
2. <span data-ttu-id="198d7-458">Bejelentkezési toohello virtuális gép, és másolja hello adatait hello kötet toohello új lemezhez, amely leképezhető toothat kötet.</span><span class="sxs-lookup"><span data-stu-id="198d7-458">Login toohello VM and copy hello data from hello current volume toohello new disk that maps toothat volume.</span></span> <span data-ttu-id="198d7-459">Ehhez az toomap tooa új lemez igénylő összes hello aktuális kötet.</span><span class="sxs-lookup"><span data-stu-id="198d7-459">Do this for all hello current volumes that need toomap tooa new disk.</span></span>
3. <span data-ttu-id="198d7-460">A következő hello alkalmazás beállítások tooswitch toohello új lemezek módosítsa, majd hello régi kötetet leválasztani.</span><span class="sxs-lookup"><span data-stu-id="198d7-460">Next, change hello application settings tooswitch toohello new disks, and detach hello old volumes.</span></span>

<span data-ttu-id="198d7-461">A lemez teljesítmény növelése érdekében hello alkalmazás hangolása, tekintse meg az túl[alkalmazások teljesítményének optimalizálása](storage-premium-storage-performance.md#optimizing-application-performance).</span><span class="sxs-lookup"><span data-stu-id="198d7-461">For tuning hello application for better disk performance, please refer too[Optimizing Application Performance](storage-premium-storage-performance.md#optimizing-application-performance).</span></span>

### <a name="application-migrations"></a><span data-ttu-id="198d7-462">Alkalmazás-áttelepítések</span><span class="sxs-lookup"><span data-stu-id="198d7-462">Application migrations</span></span>
<span data-ttu-id="198d7-463">Adatbázisok és más összetett alkalmazások lehet szükség különleges lépések hello alkalmazás szolgáltató hello áttelepítéshez NSA.</span><span class="sxs-lookup"><span data-stu-id="198d7-463">Databases and other complex applications may require special steps as defined by hello application provider for hello migration.</span></span> <span data-ttu-id="198d7-464">Toorespective alkalmazás dokumentációjában tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="198d7-464">Please refer toorespective application documentation.</span></span> <span data-ttu-id="198d7-465">Például</span><span class="sxs-lookup"><span data-stu-id="198d7-465">E.g.</span></span> <span data-ttu-id="198d7-466">általában adatbázisok telepíthetők át a biztonsági mentés és visszaállítás.</span><span class="sxs-lookup"><span data-stu-id="198d7-466">typically databases can be migrated through backup and restore.</span></span>

## <a name="next-steps"></a><span data-ttu-id="198d7-467">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="198d7-467">Next steps</span></span>
<span data-ttu-id="198d7-468">Tekintse meg a következő virtuális gépek meghatározott forgatókönyvek erőforrásait hello:</span><span class="sxs-lookup"><span data-stu-id="198d7-468">See hello following resources for specific scenarios for migrating virtual machines:</span></span>

* [<span data-ttu-id="198d7-469">Az Azure virtuális gépek közötti Storage-fiókok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="198d7-469">Migrate Azure Virtual Machines between Storage Accounts</span></span>](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [<span data-ttu-id="198d7-470">Hozzon létre, és töltse fel a Windows Server VHD tooAzure.</span><span class="sxs-lookup"><span data-stu-id="198d7-470">Create and upload a Windows Server VHD tooAzure.</span></span>](../../virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [<span data-ttu-id="198d7-471">Létrehozásával, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="198d7-471">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>](../../virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [<span data-ttu-id="198d7-472">Amazon AWS tooMicrosoft Azure virtuális gépek áttelepítése</span><span class="sxs-lookup"><span data-stu-id="198d7-472">Migrating Virtual Machines from Amazon AWS tooMicrosoft Azure</span></span>](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

<span data-ttu-id="198d7-473">A következő erőforrások toolearn további információk az Azure Storage és az Azure virtuális gépek hello lásd még:</span><span class="sxs-lookup"><span data-stu-id="198d7-473">Also, see hello following resources toolearn more about Azure Storage and Azure Virtual Machines:</span></span>

* [<span data-ttu-id="198d7-474">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="198d7-474">Azure Storage</span></span>](https://azure.microsoft.com/documentation/services/storage/)
* [<span data-ttu-id="198d7-475">Az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="198d7-475">Azure Virtual Machines</span></span>](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [<span data-ttu-id="198d7-476">Premium Storage: Nagy teljesítményű tárolási szolgáltatás Azure-beli virtuális gépek számítási feladataihoz</span><span class="sxs-lookup"><span data-stu-id="198d7-476">Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads</span></span>](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx

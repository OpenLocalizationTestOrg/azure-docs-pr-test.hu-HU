---
title: "Azure IaaS virtuális gép lemezeivel kapcsolatos gyakori kérdések (GYIK) |} Microsoft Docs"
description: "Azure IaaS virtuális gép és a premium lemezek (felügyelt és nem felügyelt) kapcsolatos gyakori kérdések"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="60545-103">Azure IaaS virtuális gép és a kezelt és kezeletlen premium lemezek kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="60545-103">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="60545-104">Ebben a cikkben megválaszolunk néhány Azure felügyelt lemezek és a prémium szintű Azure Storage kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="60545-104">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="60545-105">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="60545-105">Managed Disks</span></span>

<span data-ttu-id="60545-106">**Mi az Azure Managed lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-106">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="60545-107">Felügyelt lemezek olyan szolgáltatás, amely egyszerűbbé teszi a Lemezkezelés Azure IaaS virtuális gépek kezelése a tárolási fiók kezelése az Ön által.</span><span class="sxs-lookup"><span data-stu-id="60545-107">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="60545-108">További információkért lásd: hello [felügyelt lemezekhez – áttekintés](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60545-108">For more information, see hello [Managed Disks overview](storage-managed-disks-overview.md).</span></span>

<span data-ttu-id="60545-109">**Ha egy standard szintű felügyelt lemezes I készíteni egy meglévő VHD-t, amely 80 GB, mennyi lesz, amely költségeket, me?**</span><span class="sxs-lookup"><span data-stu-id="60545-109">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="60545-110">80 GB-os virtuális merevlemezről létrehozott egy standard szintű felügyelt lemezes hello tovább érhető el standard méretű lemez méretet, amelyet egy S10 lemez, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="60545-110">A standard managed disk created from an 80-GB VHD is treated as hello next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="60545-111">Függően toohello S10 lemez árképzési most számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="60545-111">You're charged according toohello S10 disk pricing.</span></span> <span data-ttu-id="60545-112">További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="60545-112">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="60545-113">**Vannak-e a standard szintű felügyelt lemez tranzakciós költségeket?**</span><span class="sxs-lookup"><span data-stu-id="60545-113">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="60545-114">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-114">Yes.</span></span> <span data-ttu-id="60545-115">Minden tranzakció még fizetnie.</span><span class="sxs-lookup"><span data-stu-id="60545-115">You're charged for each transaction.</span></span> <span data-ttu-id="60545-116">További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="60545-116">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="60545-117">**Az egy standard szintű felügyelt lemezes I megterheljük hello hello lemezen tárolt adatok tényleges mérete hello vagy hello lemez kapacitását kiépített hello?**</span><span class="sxs-lookup"><span data-stu-id="60545-117">**For a standard managed disk, will I be charged for hello actual size of hello data on hello disk or for hello provisioned capacity of hello disk?**</span></span>

<span data-ttu-id="60545-118">Most felszámított hello lemez kapacitását kiépített hello alapján.</span><span class="sxs-lookup"><span data-stu-id="60545-118">You're charged based on hello provisioned capacity of hello disk.</span></span> <span data-ttu-id="60545-119">További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="60545-119">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="60545-120">**Hogyan azonos a nem felügyelt lemezek felügyelt premium lemezek árképzési?**</span><span class="sxs-lookup"><span data-stu-id="60545-120">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="60545-121">hello árképzési felügyelt premium lemezek a ugyanaz, mint a nem felügyelt premium lemezek hello.</span><span class="sxs-lookup"><span data-stu-id="60545-121">hello pricing of premium managed disks is hello same as unmanaged premium disks.</span></span>

<span data-ttu-id="60545-122">**Módosíthatja a hello tárfiók típusa (Standard vagy prémium) a kezelt lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-122">**Can I change hello storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="60545-123">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-123">Yes.</span></span> <span data-ttu-id="60545-124">Hello tárfióktípus a felügyelt lemezek hello Azure-portálon, a PowerShell vagy a hello Azure parancssori felület használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="60545-124">You can change hello storage account type of your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="60545-125">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes tooa titkos tárfiók exportálása?**</span><span class="sxs-lookup"><span data-stu-id="60545-125">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="60545-126">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-126">Yes.</span></span> <span data-ttu-id="60545-127">A felügyelt lemezek hello Azure-portálon, a PowerShell vagy a hello Azure CLI segítségével exportálhatja.</span><span class="sxs-lookup"><span data-stu-id="60545-127">You can export your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="60545-128">**Használható az Azure storage-fiók toocreate felügyelt lemezes a VHD-fájl egy másik előfizetést?**</span><span class="sxs-lookup"><span data-stu-id="60545-128">**Can I use a VHD file in an Azure storage account toocreate a managed disk with a different subscription?**</span></span>

<span data-ttu-id="60545-129">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-129">No.</span></span>

<span data-ttu-id="60545-130">**Használható az Azure storage-fiók toocreate felügyelt lemezes a VHD-fájl egy másik régióban?**</span><span class="sxs-lookup"><span data-stu-id="60545-130">**Can I use a VHD file in an Azure storage account toocreate a managed disk in a different region?**</span></span>

<span data-ttu-id="60545-131">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-131">No.</span></span>

<span data-ttu-id="60545-132">**Vannak-e a felügyelt lemezeket használó ügyfelek skálázási korlátozások?**</span><span class="sxs-lookup"><span data-stu-id="60545-132">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="60545-133">Felügyelt lemezek kiküszöböli hello korlátok társított tárfiókokat.</span><span class="sxs-lookup"><span data-stu-id="60545-133">Managed Disks eliminates hello limits associated with storage accounts.</span></span> <span data-ttu-id="60545-134">Előfizetésenként felügyelt lemezek hello száma azonban korlátozott too2, alapértelmezés szerint 000.</span><span class="sxs-lookup"><span data-stu-id="60545-134">However, hello number of managed disks per subscription is limited too2,000 by default.</span></span> <span data-ttu-id="60545-135">Támogatási tooincrease hívhatják ezt a számot.</span><span class="sxs-lookup"><span data-stu-id="60545-135">You can call support tooincrease this number.</span></span>

<span data-ttu-id="60545-136">**Eltarthat egy kezelt lemez növekményes pillanatképet?**</span><span class="sxs-lookup"><span data-stu-id="60545-136">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="60545-137">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-137">No.</span></span> <span data-ttu-id="60545-138">hello aktuális pillanatkép-készítés révén felügyelt lemezes teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="60545-138">hello current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="60545-139">Azonban azt tervezi, toosupport növekményes pillanatképek a jövőbeli hello.</span><span class="sxs-lookup"><span data-stu-id="60545-139">However, we are planning toosupport incremental snapshots in hello future.</span></span>

<span data-ttu-id="60545-140">**Virtuális gépek rendelkezésre állási csoport által felügyelt és nem kezelt lemezek kombinációja állhat?**</span><span class="sxs-lookup"><span data-stu-id="60545-140">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="60545-141">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-141">No.</span></span> <span data-ttu-id="60545-142">hello virtuális gépek rendelkezésre állási csoportba kell használnia, vagy az összes felügyelt lemeznek, vagy a nem felügyelt összes lemez.</span><span class="sxs-lookup"><span data-stu-id="60545-142">hello VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="60545-143">Amikor létrehoz egy rendelkezésre állási csoportot, válassza a lemezek milyen típusú toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="60545-143">When you create an availability set, you can choose which type of disks you want toouse.</span></span>

<span data-ttu-id="60545-144">**Hello Azure-portálon kezelt lemezek hello alapértelmezett beállítás van?**</span><span class="sxs-lookup"><span data-stu-id="60545-144">**Is Managed Disks hello default option in hello Azure portal?**</span></span>

<span data-ttu-id="60545-145">Jelenleg nem de válik a jövőbeli hello hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="60545-145">Not currently, but it will become hello default in hello future.</span></span>

<span data-ttu-id="60545-146">**Hozhat létre egy üres felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="60545-146">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="60545-147">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-147">Yes.</span></span> <span data-ttu-id="60545-148">Üres lemez hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="60545-148">You can create an empty disk.</span></span> <span data-ttu-id="60545-149">Felügyelt lemezes hozhatók létre függetlenül a virtuális gépek például tooa virtuális gép csatlakoztatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="60545-149">A managed disk can be created independently of a VM, for example, without attaching it tooa VM.</span></span>

<span data-ttu-id="60545-150">**Mi az a hello támogatott tartalék tartományainak száma egy rendelkezésre állási csoport által felügyelt lemezeket használó?**</span><span class="sxs-lookup"><span data-stu-id="60545-150">**What is hello supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="60545-151">Attól függően, hogy hello régió, ahol felügyelt lemezeket használó hello rendelkezésre állási csoport a támogatott hello tartalék tartományainak száma 2 vagy 3.</span><span class="sxs-lookup"><span data-stu-id="60545-151">Depending on hello region where hello availability set that uses Managed Disks is located, hello supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="60545-152">**Standard szintű tárfiók hello beállítása diagnosztika hogyan van?**</span><span class="sxs-lookup"><span data-stu-id="60545-152">**How is hello standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="60545-153">A Virtuálisgép-diagnosztika titkos tárolási fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="60545-153">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="60545-154">A jövőbeli hello, hogy terv tooswitch diagnosztika tooManaged lemezeket is.</span><span class="sxs-lookup"><span data-stu-id="60545-154">In hello future, we plan tooswitch diagnostics tooManaged Disks as well.</span></span>

<span data-ttu-id="60545-155">**Milyen típusú szerepköralapú hozzáférés-vezérlés támogatásának felügyelt lemezek érhető el?**</span><span class="sxs-lookup"><span data-stu-id="60545-155">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="60545-156">Felügyelt lemezek által támogatott három fő alapértelmezett szerepkörök:</span><span class="sxs-lookup"><span data-stu-id="60545-156">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="60545-157">Tulajdonos: Mindent felügyelhetnek, beleértve a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="60545-157">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="60545-158">Társszerző: Hozzáférés kivételével mindent felügyelhetnek</span><span class="sxs-lookup"><span data-stu-id="60545-158">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="60545-159">Olvasó: Mindent megtekinthetnek, de nem módosítható</span><span class="sxs-lookup"><span data-stu-id="60545-159">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="60545-160">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes tooa titkos tárfiók exportálása?**</span><span class="sxs-lookup"><span data-stu-id="60545-160">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="60545-161">A csak olvasható közös hozzáférésű jogosultságkód felügyelt hello URI-JÁNAK lemez, és ezzel toocopy hello tartalma tooa titkos tárolási fiók vagy a helyszíni tárolási kérheti le.</span><span class="sxs-lookup"><span data-stu-id="60545-161">You can get a read-only shared access signature URI for hello managed disk and use it toocopy hello contents tooa private storage account or on-premises storage.</span></span>

<span data-ttu-id="60545-162">**A felügyelt lemezes másolatát hozhat létre?**</span><span class="sxs-lookup"><span data-stu-id="60545-162">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="60545-163">Ügyfelek pillanatképet készít a felügyelt lemezeket, és egy másik felügyelt lemezes hello pillanatkép toocreate használhatja.</span><span class="sxs-lookup"><span data-stu-id="60545-163">Customers can take a snapshot of their managed disks and then use hello snapshot toocreate another managed disk.</span></span>

<span data-ttu-id="60545-164">**Nem felügyelt lemezek továbbra is támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="60545-164">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="60545-165">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-165">Yes.</span></span> <span data-ttu-id="60545-166">Nem felügyelt és a felügyelt támogatjuk.</span><span class="sxs-lookup"><span data-stu-id="60545-166">We support unmanaged and managed disks.</span></span> <span data-ttu-id="60545-167">Javasoljuk, hogy új munkaterhelések esetén használja a felügyelt lemezeit, és telepítse át a jelenlegi munkaterhelés toomanaged lemezek.</span><span class="sxs-lookup"><span data-stu-id="60545-167">We recommend that you use managed disks for new workloads and migrate your current workloads toomanaged disks.</span></span>


<span data-ttu-id="60545-168">**Ha megkezdtem 128 GB-os lemez létrehozása, és növelje hello mérete too130 GB, I megterheljük a hello következő lemezméretet (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="60545-168">**If I create a 128-GB disk and then increase hello size too130 GB, will I be charged for hello next disk size (512 GB)?**</span></span>

<span data-ttu-id="60545-169">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-169">Yes.</span></span>

<span data-ttu-id="60545-170">**Létrehozhatok helyileg redundáns tárolás, a georedundáns tárolás és zónaredundáns tárolás által kezelt lemezeken?**</span><span class="sxs-lookup"><span data-stu-id="60545-170">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="60545-171">Azure-lemezeket felügyelt jelenleg csak a helyileg redundáns tárolás felügyelt lemezeit támogatja.</span><span class="sxs-lookup"><span data-stu-id="60545-171">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="60545-172">**Csökkenhessen vagy a felügyelt lemezek downsize?**</span><span class="sxs-lookup"><span data-stu-id="60545-172">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="60545-173">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-173">No.</span></span> <span data-ttu-id="60545-174">Ez a funkció jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="60545-174">This feature is not supported currently.</span></span> 

<span data-ttu-id="60545-175">**Módosítható hello számítógép name tulajdonság esetén egy speciális (nem hello rendszer-előkészítő eszköz használatával létrehozott vagy általánosított) operációs rendszer tárolására használt tooprovision egy virtuális Gépet?**</span><span class="sxs-lookup"><span data-stu-id="60545-175">**Can I change hello computer name property when a specialized (not created by using hello System Preparation tool or generalized) operating system disk is used tooprovision a VM?**</span></span>

<span data-ttu-id="60545-176">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-176">No.</span></span> <span data-ttu-id="60545-177">Hello számítógép name tulajdonság nem frissíthető.</span><span class="sxs-lookup"><span data-stu-id="60545-177">You can't update hello computer name property.</span></span> <span data-ttu-id="60545-178">hello új virtuális gép örökli azt hello szülő virtuális gép, amely használt toocreate hello operációsrendszer-lemez volt.</span><span class="sxs-lookup"><span data-stu-id="60545-178">hello new VM inherits it from hello parent VM, which was used toocreate hello operating system disk.</span></span> 

<span data-ttu-id="60545-179">**Hol találnak a minta Azure Resource Manager sablonok toocreate virtuális gépek felügyelt lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-179">**Where can I find sample Azure Resource Manager templates toocreate VMs with managed disks?**</span></span>
* [<span data-ttu-id="60545-180">Felügyelt lemezekkel sablonok listája</span><span class="sxs-lookup"><span data-stu-id="60545-180">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="60545-181">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="60545-181">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="60545-182">Lemezek és a Storage szolgáltatás titkosítási felügyelt</span><span class="sxs-lookup"><span data-stu-id="60545-182">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="60545-183">**Azure Storage szolgáltatás titkosítási alapértelmezés szerint engedélyezve van egy felügyelt lemezes létrehozásakor?**</span><span class="sxs-lookup"><span data-stu-id="60545-183">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="60545-184">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-184">Yes.</span></span>

<span data-ttu-id="60545-185">**Kezelő hello titkosítási kulcsokat?**</span><span class="sxs-lookup"><span data-stu-id="60545-185">**Who manages hello encryption keys?**</span></span>

<span data-ttu-id="60545-186">A Microsoft kezeli a hello titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="60545-186">Microsoft manages hello encryption keys.</span></span>

<span data-ttu-id="60545-187">**Képes-e letiltása Storage szolgáltatás titkosítási a felügyelt lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-187">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="60545-188">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-188">No.</span></span>

<span data-ttu-id="60545-189">**Érhető Storage szolgáltatás titkosítási csak meghatározott régióiba?**</span><span class="sxs-lookup"><span data-stu-id="60545-189">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="60545-190">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-190">No.</span></span> <span data-ttu-id="60545-191">Érhető el minden, amennyiben rendelkezésre áll-e felügyelt lemezek hello régióban.</span><span class="sxs-lookup"><span data-stu-id="60545-191">It's available in all hello regions where Managed Disks is available.</span></span> <span data-ttu-id="60545-192">Felügyelt lemezek minden nyilvános régiók és Németországban érhető el.</span><span class="sxs-lookup"><span data-stu-id="60545-192">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="60545-193">**Hogyan tudhatom meg titkosítottak, ha a felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="60545-193">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="60545-194">Felügyelt lemezes létrehozásának hello Azure-portálon hello Azure CLI és PowerShell hello időpontja talál.</span><span class="sxs-lookup"><span data-stu-id="60545-194">You can find out hello time when a managed disk was created from hello Azure portal, hello Azure CLI, and PowerShell.</span></span> <span data-ttu-id="60545-195">Ha hello idő után 2017. június 9., a lemez titkosítva.</span><span class="sxs-lookup"><span data-stu-id="60545-195">If hello time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="60545-196">**Hogyan titkosíthatja a meglévő lemezek 2017. június 10. előtt létrehozott?**</span><span class="sxs-lookup"><span data-stu-id="60545-196">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="60545-197">2017. június 10., frissítésétől felügyelt tooexisting lemezek új adatokat automatikusan titkosítva.</span><span class="sxs-lookup"><span data-stu-id="60545-197">As of June 10, 2017, new data written tooexisting managed disks is automatically encrypted.</span></span> <span data-ttu-id="60545-198">Azt is tervez tooencrypt meglévő adatok és hello titkosítási hello háttérben aszinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="60545-198">We are also planning tooencrypt existing data, and hello encryption will happen asynchronously in hello background.</span></span> <span data-ttu-id="60545-199">Ha meglévő adatokat most titkosítani kell, hozzon létre egy másolatot a lemezen.</span><span class="sxs-lookup"><span data-stu-id="60545-199">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="60545-200">Új lemez titkosítva legyen.</span><span class="sxs-lookup"><span data-stu-id="60545-200">New disks will be encrypted.</span></span>

* [<span data-ttu-id="60545-201">Másolja át a felügyelt lemezek hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="60545-201">Copy managed disks by using hello Azure CLI</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="60545-202">Felügyelt lemezeket másolja a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="60545-202">Copy managed disks by using PowerShell</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="60545-203">**Felügyelt pillanatfelvételek és lemezképek titkosítva van?**</span><span class="sxs-lookup"><span data-stu-id="60545-203">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="60545-204">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-204">Yes.</span></span> <span data-ttu-id="60545-205">Az összes kezelt pillanatképeket és lemezképek létrehozása után 2017. június 9., automatikusan titkosítva vannak.</span><span class="sxs-lookup"><span data-stu-id="60545-205">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="60545-206">**Képes virtuális gépek is átalakítása nem felügyelt lemezek tárfiókokban vagy korábban titkosított toomanaged lemez található?**</span><span class="sxs-lookup"><span data-stu-id="60545-206">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted toomanaged disks?**</span></span>

<span data-ttu-id="60545-207">Igen</span><span class="sxs-lookup"><span data-stu-id="60545-207">Yes</span></span>

<span data-ttu-id="60545-208">**A felügyelt lemezes vagy a pillanatképet egy exportált VHD-t is titkosítva lesznek?**</span><span class="sxs-lookup"><span data-stu-id="60545-208">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="60545-209">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-209">No.</span></span> <span data-ttu-id="60545-210">De ha egy virtuális merevlemez tooan exportálnia titkosított tárfiók egy titkosított felügyelt lemezes vagy a pillanatkép, majd titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="60545-210">But if you export a VHD tooan encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="60545-211">Prémium szintű lemezekhez: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="60545-211">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="60545-212">**Egy virtuális gép használ, amely támogatja a prémium szintű Storage, például egy DSv2 mérete több premium és a szabványos adatlemezek csatolható?**</span><span class="sxs-lookup"><span data-stu-id="60545-212">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="60545-213">Igen.</span><span class="sxs-lookup"><span data-stu-id="60545-213">Yes.</span></span>

<span data-ttu-id="60545-214">**Prémium szintű és a standard lemezek tooa mérete adatsorozat, amely nem támogatja a prémium szintű Storage, például a D, Dv2, G vagy F adatsorozat csatolható?**</span><span class="sxs-lookup"><span data-stu-id="60545-214">**Can I attach both premium and standard data disks tooa size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="60545-215">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-215">No.</span></span> <span data-ttu-id="60545-216">Csak szabványos adatok lemezek tooVMs, amelyek nem használják, amely támogatja a prémium szintű Storage mérete több csatolható.</span><span class="sxs-lookup"><span data-stu-id="60545-216">You can attach only standard data disks tooVMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="60545-217">**Ha egy meglévő virtuális merevlemezről, de a 80 GB prémium adatlemezt hozok létre, mennyire, amely ára?**</span><span class="sxs-lookup"><span data-stu-id="60545-217">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="60545-218">80 GB-os virtuális merevlemezről létrehozott prémium adatlemezt P10 lemez hello tovább érhető el prémium szintű lemez méretének kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="60545-218">A premium data disk created from an 80-GB VHD is treated as hello next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="60545-219">Függően toohello P10 lemez árképzési most számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="60545-219">You're charged according toohello P10 disk pricing.</span></span>

<span data-ttu-id="60545-220">**Vannak-e tranzakciós költségek toouse prémium szintű Storage?**</span><span class="sxs-lookup"><span data-stu-id="60545-220">**Are there transaction costs toouse Premium Storage?**</span></span>

<span data-ttu-id="60545-221">Nincs minden lemez méretét, amelyet adott határral kiosztott iops-érték és az átvitel meg egy rögzített költséget.</span><span class="sxs-lookup"><span data-stu-id="60545-221">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="60545-222">hello egyéb költségek olyan kimenő sávszélesség és a pillanatkép-kapacitást, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="60545-222">hello other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="60545-223">További információkért lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="60545-223">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="60545-224">**Az iops-érték és a teljesítményt, amelyek szeretnék hello lemezgyorsítótár lekérheti hello korlátai**</span><span class="sxs-lookup"><span data-stu-id="60545-224">**What are hello limits for IOPS and throughput that I can get from hello disk cache?**</span></span>

<span data-ttu-id="60545-225">gyorsítótár kombinált korlátairól hello és DS azokat a helyi SSD core 4000 iops-értéket és core másodpercenként 33 MB.</span><span class="sxs-lookup"><span data-stu-id="60545-225">hello combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="60545-226">hello GS adatsorozat 5000 iops teljesítményt nyújtanak core és 50 MB / s / core kínál.</span><span class="sxs-lookup"><span data-stu-id="60545-226">hello GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="60545-227">**A lemezek felügyelt virtuális gépek támogatott helyi SSD hello van?**</span><span class="sxs-lookup"><span data-stu-id="60545-227">**Is hello local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="60545-228">hello helyi SSD az ideiglenes tároló, amely megtalálható a felügyelt lemezek virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="60545-228">hello local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="60545-229">Nincs ingyenesen a ideiglenes tárolására.</span><span class="sxs-lookup"><span data-stu-id="60545-229">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="60545-230">Azt javasoljuk, hogy nem használja a helyi SSD toostore az alkalmazásadatok, mert azt az Azure Blob storage nem megőrzött.</span><span class="sxs-lookup"><span data-stu-id="60545-230">We recommend that you do not use this local SSD toostore your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="60545-231">**Nincs hello bármely következményekkel használatban vannak a VÁGÁSHOZ a premium lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-231">**Are there any repercussions for hello use of TRIM on premium disks?**</span></span>

<span data-ttu-id="60545-232">Nincs Nincs vágás vagy prémium szintű Azure-lemezeket vagy a standard lemezek hátránya toohello használatát.</span><span class="sxs-lookup"><span data-stu-id="60545-232">There is no downside toohello use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="60545-233">Új lemezméret: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="60545-233">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="60545-234">**Mi az az hello támogatott operációs rendszer és az adatlemezek legnagyobb lemez méretét?**</span><span class="sxs-lookup"><span data-stu-id="60545-234">**What is hello largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="60545-235">hello partíció, amely Azure támogatja az operációsrendszer-lemez típus hello fő rendszertöltő rekord (MBR).</span><span class="sxs-lookup"><span data-stu-id="60545-235">hello partition type that Azure supports for an operating system disk is hello master boot record (MBR).</span></span> <span data-ttu-id="60545-236">hello MBR formátumhoz too2 TB fel a lemez méretét.</span><span class="sxs-lookup"><span data-stu-id="60545-236">hello MBR format supports a disk size up too2 TB.</span></span> <span data-ttu-id="60545-237">hello legnagyobb Azure támogatja az operációsrendszer-lemez mérete 2 TB.</span><span class="sxs-lookup"><span data-stu-id="60545-237">hello largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="60545-238">Azure legfeljebb too4 TB adatlemezek támogat.</span><span class="sxs-lookup"><span data-stu-id="60545-238">Azure supports up too4 TB for data disks.</span></span> 

<span data-ttu-id="60545-239">**Mi az az hello legnagyobb blob oldalméret támogatott?**</span><span class="sxs-lookup"><span data-stu-id="60545-239">**What is hello largest page blob size that's supported?**</span></span>

<span data-ttu-id="60545-240">hello legnagyobb lap blob, amely támogatja az Azure mérete 8 TB (8,191 GB).</span><span class="sxs-lookup"><span data-stu-id="60545-240">hello largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="60545-241">Nagyobb, mint 4 TB-os (4095 GB) kapcsolódó tooa VM adatok vagy az operációs rendszer lemezek lapblobokat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="60545-241">We don't support page blobs larger than 4 TB (4,095 GB) attached tooa VM as data or operating system disks.</span></span>

<span data-ttu-id="60545-242">**Toouse újabb verzióra kell az Azure-eszközök toocreate, csatolja, méretezze át, és töltse fel az 1 TB-nál nagyobb lemezek?**</span><span class="sxs-lookup"><span data-stu-id="60545-242">**Do I need toouse a new version of Azure tools toocreate, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="60545-243">Nem kell tooupgrade a meglévő Azure-eszközök toocreate, csatolja vagy 1 TB-nál nagyobb lemezek átméretezése.</span><span class="sxs-lookup"><span data-stu-id="60545-243">You don't need tooupgrade your existing Azure tools toocreate, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="60545-244">a VHD fájlt tooupload helyszíni közvetlenül tooAzure oldalakra vonatkozó blob vagy nem felügyelt lemez, toouse hello legújabb eszköz beállítása szükséges:</span><span class="sxs-lookup"><span data-stu-id="60545-244">tooupload your VHD file from on-premises directly tooAzure as a page blob or unmanaged disk, you need toouse hello latest tool sets:</span></span>

|<span data-ttu-id="60545-245">Azure-eszközök</span><span class="sxs-lookup"><span data-stu-id="60545-245">Azure tools</span></span>      | <span data-ttu-id="60545-246">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="60545-246">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="60545-247">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="60545-247">Azure PowerShell</span></span> | <span data-ttu-id="60545-248">4.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="60545-248">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="60545-249">Az Azure CLI 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="60545-249">Azure CLI v1</span></span>     | <span data-ttu-id="60545-250">Verziószám 0.10.13: Előfordulhat, hogy 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="60545-250">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="60545-251">AzCopy</span><span class="sxs-lookup"><span data-stu-id="60545-251">AzCopy</span></span>           | <span data-ttu-id="60545-252">6.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="60545-252">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="60545-253">az Azure parancssori felület v2 és Azure Tártallózó hello támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="60545-253">hello support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="60545-254">**Nem felügyelt lemezek vagy lapblobokat támogatott P4 és P6 mérete?**</span><span class="sxs-lookup"><span data-stu-id="60545-254">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="60545-255">Nem.</span><span class="sxs-lookup"><span data-stu-id="60545-255">No.</span></span> <span data-ttu-id="60545-256">P4 (32 GB) és P6 (64 GB-os) lemezméret csak felügyelt lemezek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="60545-256">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="60545-257">Nem felügyelt lemezek és a lapblobokat támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="60545-257">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="60545-258">**Ha a meglévő prémium szintű felügyelt lemez kevesebb, mint 64 GB-os hozták létre, mielőtt hello kisebb lemezt engedélyezése (körülbelül 2017. június 15.), hogyan azt számlázása?**</span><span class="sxs-lookup"><span data-stu-id="60545-258">**If my existing premium managed disk less than 64 GB was created before hello small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="60545-259">64 GB-nál kisebb meglévő kis premium lemezek továbbra is számlázva toobe függően toohello P10 tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="60545-259">Existing small premium disks less than 64 GB continue toobe billed according toohello P10 pricing tier.</span></span> 

<span data-ttu-id="60545-260">**Hogyan átválthatja hello lemez réteg 64 GB-nál kisebb kis premium lemezek P10 tooP4 vagy P6?**</span><span class="sxs-lookup"><span data-stu-id="60545-260">**How can I switch hello disk tier of small premium disks less than 64 GB from P10 tooP4 or P6?**</span></span>

<span data-ttu-id="60545-261">Pillanatkép készítése a kis kapacitású lemezeket, és hozzon létre egy lemez tooautomatically kapcsoló hello árképzési szint tooP4 vagy P6 kiépített hello mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="60545-261">You can take a snapshot of your small disks and then create a disk tooautomatically switch hello pricing tier tooP4 or P6 based on hello provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="60545-262">Mi történik, ha a fentiekben itt nem választ?</span><span class="sxs-lookup"><span data-stu-id="60545-262">What if my question isn't answered here?</span></span>

<span data-ttu-id="60545-263">Ha kérdését itt nem látható, ossza meg velünk, és segítünk talált választ.</span><span class="sxs-lookup"><span data-stu-id="60545-263">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="60545-264">Ez a cikk végén hello kérdés beküldheti hello megjegyzésekben.</span><span class="sxs-lookup"><span data-stu-id="60545-264">You can post a question at hello end of this article in hello comments.</span></span> <span data-ttu-id="60545-265">tooengage hello Azure Storage-csapattal és a Közösség más tagjaival kapcsolatos ebben a cikkben használja hello MSDN [Azure Storage fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="60545-265">tooengage with hello Azure Storage team and other community members about this article, use hello MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="60545-266">toorequest szolgáltatásokat nyújt a kérelmek és ötleteket toohello [Azure Storage-visszajelzési fórumon](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="60545-266">toorequest features, submit your requests and ideas toohello [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>

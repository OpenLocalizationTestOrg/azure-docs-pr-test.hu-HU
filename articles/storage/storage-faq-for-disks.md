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
ms.openlocfilehash: 9e5eed35334f1b95441b8181c8e90645be78b389
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="36232-103">Azure IaaS virtuális gép és a kezelt és kezeletlen premium lemezek kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="36232-103">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="36232-104">Ebben a cikkben megválaszolunk néhány Azure felügyelt lemezek és a prémium szintű Azure Storage kapcsolatos gyakori kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="36232-104">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="36232-105">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="36232-105">Managed Disks</span></span>

<span data-ttu-id="36232-106">**Mi az Azure Managed lemezek?**</span><span class="sxs-lookup"><span data-stu-id="36232-106">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="36232-107">Felügyelt lemezek olyan szolgáltatás, amely egyszerűbbé teszi a Lemezkezelés Azure IaaS virtuális gépek kezelése a tárolási fiók kezelése az Ön által.</span><span class="sxs-lookup"><span data-stu-id="36232-107">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="36232-108">További információkért lásd: a [felügyelt lemezekhez – áttekintés](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="36232-108">For more information, see the [Managed Disks overview](storage-managed-disks-overview.md).</span></span>

<span data-ttu-id="36232-109">**Ha egy standard szintű felügyelt lemezes I készíteni egy meglévő VHD-t, amely 80 GB, mennyi lesz, amely költségeket, me?**</span><span class="sxs-lookup"><span data-stu-id="36232-109">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="36232-110">80 GB-os virtuális merevlemezről létrehozott egy standard szintű felügyelt lemezes a rendszer a következő elérhető standard méretű lemez méretet, amelyet egy S10 lemez.</span><span class="sxs-lookup"><span data-stu-id="36232-110">A standard managed disk created from an 80-GB VHD is treated as the next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="36232-111">Most a S10 lemez díjszabás alapján számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="36232-111">You're charged according to the S10 disk pricing.</span></span> <span data-ttu-id="36232-112">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="36232-112">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="36232-113">**Vannak-e a standard szintű felügyelt lemez tranzakciós költségeket?**</span><span class="sxs-lookup"><span data-stu-id="36232-113">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="36232-114">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-114">Yes.</span></span> <span data-ttu-id="36232-115">Minden tranzakció még fizetnie.</span><span class="sxs-lookup"><span data-stu-id="36232-115">You're charged for each transaction.</span></span> <span data-ttu-id="36232-116">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="36232-116">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="36232-117">**Az egy standard szintű felügyelt lemezes I megterheljük a lemezen lévő adatok tényleges mérete, vagy a lemez kiosztott kapacitást?**</span><span class="sxs-lookup"><span data-stu-id="36232-117">**For a standard managed disk, will I be charged for the actual size of the data on the disk or for the provisioned capacity of the disk?**</span></span>

<span data-ttu-id="36232-118">A lemez kiosztott kapacitást alapján még fel.</span><span class="sxs-lookup"><span data-stu-id="36232-118">You're charged based on the provisioned capacity of the disk.</span></span> <span data-ttu-id="36232-119">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="36232-119">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="36232-120">**Hogyan azonos a nem felügyelt lemezek felügyelt premium lemezek árképzési?**</span><span class="sxs-lookup"><span data-stu-id="36232-120">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="36232-121">A felügyelt premium lemezek árképzési megegyezik a nem felügyelt premium lemezek.</span><span class="sxs-lookup"><span data-stu-id="36232-121">The pricing of premium managed disks is the same as unmanaged premium disks.</span></span>

<span data-ttu-id="36232-122">**A tárfiók típusa (Standard vagy prémium) a kezelt lemezek is megváltozik?**</span><span class="sxs-lookup"><span data-stu-id="36232-122">**Can I change the storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="36232-123">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-123">Yes.</span></span> <span data-ttu-id="36232-124">Az Azure-portálon, a PowerShell vagy az Azure parancssori felület használatával módosíthatja a tárfiók típusa a felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="36232-124">You can change the storage account type of your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="36232-125">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes exportálása a titkos storage-fiók?**</span><span class="sxs-lookup"><span data-stu-id="36232-125">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="36232-126">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-126">Yes.</span></span> <span data-ttu-id="36232-127">A felügyelt lemezek exportálhatja az Azure-portálon, a PowerShell vagy az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="36232-127">You can export your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="36232-128">**Az Azure-tárfiók VHD-fájl használatával hozzon létre egy felügyelt lemezes egy másik előfizetést?**</span><span class="sxs-lookup"><span data-stu-id="36232-128">**Can I use a VHD file in an Azure storage account to create a managed disk with a different subscription?**</span></span>

<span data-ttu-id="36232-129">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-129">No.</span></span>

<span data-ttu-id="36232-130">**Az Azure-tárfiók VHD-fájl használatával hozzon létre egy felügyelt lemezes egy másik régióban?**</span><span class="sxs-lookup"><span data-stu-id="36232-130">**Can I use a VHD file in an Azure storage account to create a managed disk in a different region?**</span></span>

<span data-ttu-id="36232-131">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-131">No.</span></span>

<span data-ttu-id="36232-132">**Vannak-e a felügyelt lemezeket használó ügyfelek skálázási korlátozások?**</span><span class="sxs-lookup"><span data-stu-id="36232-132">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="36232-133">Kezelt lemezek megszünteti a társított tárfiókok korlátok.</span><span class="sxs-lookup"><span data-stu-id="36232-133">Managed Disks eliminates the limits associated with storage accounts.</span></span> <span data-ttu-id="36232-134">Azonban a előfizetésenként felügyelt lemezek számát korlátozódik 2000 alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="36232-134">However, the number of managed disks per subscription is limited to 2,000 by default.</span></span> <span data-ttu-id="36232-135">Ezt a számot növelheti a támogatási hívása.</span><span class="sxs-lookup"><span data-stu-id="36232-135">You can call support to increase this number.</span></span>

<span data-ttu-id="36232-136">**Eltarthat egy kezelt lemez növekményes pillanatképet?**</span><span class="sxs-lookup"><span data-stu-id="36232-136">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="36232-137">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-137">No.</span></span> <span data-ttu-id="36232-138">Az aktuális pillanatkép-készítés révén felügyelt lemezes teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="36232-138">The current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="36232-139">Azonban azt tervezi, hogy a jövőben támogatja a növekményes pillanatképek.</span><span class="sxs-lookup"><span data-stu-id="36232-139">However, we are planning to support incremental snapshots in the future.</span></span>

<span data-ttu-id="36232-140">**Virtuális gépek rendelkezésre állási csoport által felügyelt és nem kezelt lemezek kombinációja állhat?**</span><span class="sxs-lookup"><span data-stu-id="36232-140">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="36232-141">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-141">No.</span></span> <span data-ttu-id="36232-142">A virtuális gépek rendelkezésre állási csoportba kell használnia, vagy az összes felügyelt lemeznek, vagy a nem felügyelt összes lemez.</span><span class="sxs-lookup"><span data-stu-id="36232-142">The VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="36232-143">Amikor létrehoz egy rendelkezésre állási csoportot, kiválaszthatja a használni kívánt lemezek milyen típusú.</span><span class="sxs-lookup"><span data-stu-id="36232-143">When you create an availability set, you can choose which type of disks you want to use.</span></span>

<span data-ttu-id="36232-144">**Felügyelt lemezek van az alapértelmezett beállítás, az Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="36232-144">**Is Managed Disks the default option in the Azure portal?**</span></span>

<span data-ttu-id="36232-145">Jelenleg nem de ez a jövőben lesz az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="36232-145">Not currently, but it will become the default in the future.</span></span>

<span data-ttu-id="36232-146">**Hozhat létre egy üres felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="36232-146">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="36232-147">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-147">Yes.</span></span> <span data-ttu-id="36232-148">Üres lemez hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="36232-148">You can create an empty disk.</span></span> <span data-ttu-id="36232-149">Felügyelt lemezes hozhatók létre függetlenül a virtuális gépek, például egy virtuális Géphez való csatlakoztatás nélkül.</span><span class="sxs-lookup"><span data-stu-id="36232-149">A managed disk can be created independently of a VM, for example, without attaching it to a VM.</span></span>

<span data-ttu-id="36232-150">**Mi a támogatott tartalék tartományok számának a rendelkezésre állási beállítása felügyelt lemezeket használó?**</span><span class="sxs-lookup"><span data-stu-id="36232-150">**What is the supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="36232-151">Attól függően, hogy a régió, ahol a rendelkezésre állási csoport által felügyelt lemezeket használó a támogatott tartalék tartományok számának, 2 vagy 3.</span><span class="sxs-lookup"><span data-stu-id="36232-151">Depending on the region where the availability set that uses Managed Disks is located, the supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="36232-152">**Hogyan van a standard szintű tárfiók beállítása diagnosztikai?**</span><span class="sxs-lookup"><span data-stu-id="36232-152">**How is the standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="36232-153">A Virtuálisgép-diagnosztika titkos tárolási fiók beállítása.</span><span class="sxs-lookup"><span data-stu-id="36232-153">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="36232-154">A jövőben tervezzük diagnosztika váltani kezelt lemezeken is.</span><span class="sxs-lookup"><span data-stu-id="36232-154">In the future, we plan to switch diagnostics to Managed Disks as well.</span></span>

<span data-ttu-id="36232-155">**Milyen típusú szerepköralapú hozzáférés-vezérlés támogatásának felügyelt lemezek érhető el?**</span><span class="sxs-lookup"><span data-stu-id="36232-155">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="36232-156">Felügyelt lemezek által támogatott három fő alapértelmezett szerepkörök:</span><span class="sxs-lookup"><span data-stu-id="36232-156">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="36232-157">Tulajdonos: Mindent felügyelhetnek, beleértve a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="36232-157">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="36232-158">Társszerző: Hozzáférés kivételével mindent felügyelhetnek</span><span class="sxs-lookup"><span data-stu-id="36232-158">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="36232-159">Olvasó: Mindent megtekinthetnek, de nem módosítható</span><span class="sxs-lookup"><span data-stu-id="36232-159">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="36232-160">**Van mód, hogy lehet másolni, vagy egy felügyelt lemezes exportálása a titkos storage-fiók?**</span><span class="sxs-lookup"><span data-stu-id="36232-160">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="36232-161">A csak olvasható közös hozzáférésű jogosultságkód URI lekérése a felügyelt lemezes és a segítségével a saját tárolási fiók vagy a helyszíni tárolási másolja.</span><span class="sxs-lookup"><span data-stu-id="36232-161">You can get a read-only shared access signature URI for the managed disk and use it to copy the contents to a private storage account or on-premises storage.</span></span>

<span data-ttu-id="36232-162">**A felügyelt lemezes másolatát hozhat létre?**</span><span class="sxs-lookup"><span data-stu-id="36232-162">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="36232-163">Ügyfelek pillanatkép készítése a felügyelt lemezek és a pillanatkép segítségével hozzon létre egy másik kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="36232-163">Customers can take a snapshot of their managed disks and then use the snapshot to create another managed disk.</span></span>

<span data-ttu-id="36232-164">**Nem felügyelt lemezek továbbra is támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="36232-164">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="36232-165">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-165">Yes.</span></span> <span data-ttu-id="36232-166">Nem felügyelt és a felügyelt támogatjuk.</span><span class="sxs-lookup"><span data-stu-id="36232-166">We support unmanaged and managed disks.</span></span> <span data-ttu-id="36232-167">Javasoljuk, hogy új munkaterhelések esetén használja a felügyelt lemezeit, és az aktuális munkaterheléseket telepít át kezelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="36232-167">We recommend that you use managed disks for new workloads and migrate your current workloads to managed disks.</span></span>


<span data-ttu-id="36232-168">**Ha megkezdtem 128 GB-os lemez létrehozása, és növelje a 130 GB-os méret, I megterheljük a következő lemez méretét (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="36232-168">**If I create a 128-GB disk and then increase the size to 130 GB, will I be charged for the next disk size (512 GB)?**</span></span>

<span data-ttu-id="36232-169">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-169">Yes.</span></span>

<span data-ttu-id="36232-170">**Létrehozhatok helyileg redundáns tárolás, a georedundáns tárolás és zónaredundáns tárolás által kezelt lemezeken?**</span><span class="sxs-lookup"><span data-stu-id="36232-170">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="36232-171">Azure-lemezeket felügyelt jelenleg csak a helyileg redundáns tárolás felügyelt lemezeit támogatja.</span><span class="sxs-lookup"><span data-stu-id="36232-171">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="36232-172">**Csökkenhessen vagy a felügyelt lemezek downsize?**</span><span class="sxs-lookup"><span data-stu-id="36232-172">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="36232-173">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-173">No.</span></span> <span data-ttu-id="36232-174">Ez a funkció jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="36232-174">This feature is not supported currently.</span></span> 

<span data-ttu-id="36232-175">**Módosítható a számítógép neve tulajdonság amikor egy speciális (nem hozta létre a rendszer-előkészítő eszközzel vagy általánosított) operációsrendszer-lemez a virtuális gép létrehozásához használt?**</span><span class="sxs-lookup"><span data-stu-id="36232-175">**Can I change the computer name property when a specialized (not created by using the System Preparation tool or generalized) operating system disk is used to provision a VM?**</span></span>

<span data-ttu-id="36232-176">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-176">No.</span></span> <span data-ttu-id="36232-177">A számítógép name tulajdonság nem frissíthető.</span><span class="sxs-lookup"><span data-stu-id="36232-177">You can't update the computer name property.</span></span> <span data-ttu-id="36232-178">Az új virtuális gép örökli a szülő virtuális gép, amelyen az operációsrendszer-lemez létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="36232-178">The new VM inherits it from the parent VM, which was used to create the operating system disk.</span></span> 

<span data-ttu-id="36232-179">**Hol találnak a minta Azure Resource Manager-sablonok felügyelt lemezzel rendelkező virtuális gépek létrehozásához?**</span><span class="sxs-lookup"><span data-stu-id="36232-179">**Where can I find sample Azure Resource Manager templates to create VMs with managed disks?**</span></span>
* [<span data-ttu-id="36232-180">Felügyelt lemezekkel sablonok listája</span><span class="sxs-lookup"><span data-stu-id="36232-180">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="36232-181">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="36232-181">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="36232-182">Lemezek és a Storage szolgáltatás titkosítási felügyelt</span><span class="sxs-lookup"><span data-stu-id="36232-182">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="36232-183">**Azure Storage szolgáltatás titkosítási alapértelmezés szerint engedélyezve van egy felügyelt lemezes létrehozásakor?**</span><span class="sxs-lookup"><span data-stu-id="36232-183">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="36232-184">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-184">Yes.</span></span>

<span data-ttu-id="36232-185">**A titkosítási kulcsok kezelő?**</span><span class="sxs-lookup"><span data-stu-id="36232-185">**Who manages the encryption keys?**</span></span>

<span data-ttu-id="36232-186">A Microsoft kezeli a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="36232-186">Microsoft manages the encryption keys.</span></span>

<span data-ttu-id="36232-187">**Képes-e letiltása Storage szolgáltatás titkosítási a felügyelt lemezek?**</span><span class="sxs-lookup"><span data-stu-id="36232-187">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="36232-188">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-188">No.</span></span>

<span data-ttu-id="36232-189">**Érhető Storage szolgáltatás titkosítási csak meghatározott régióiba?**</span><span class="sxs-lookup"><span data-stu-id="36232-189">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="36232-190">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-190">No.</span></span> <span data-ttu-id="36232-191">Érhető el minden olyan régióban, amennyiben rendelkezésre áll-e felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="36232-191">It's available in all the regions where Managed Disks is available.</span></span> <span data-ttu-id="36232-192">Felügyelt lemezek minden nyilvános régiók és Németországban érhető el.</span><span class="sxs-lookup"><span data-stu-id="36232-192">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="36232-193">**Hogyan tudhatom meg titkosítottak, ha a felügyelt lemezes?**</span><span class="sxs-lookup"><span data-stu-id="36232-193">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="36232-194">A felügyelt lemezes az Azure-portálon az Azure CLI és PowerShell létrehozásának ideje talál.</span><span class="sxs-lookup"><span data-stu-id="36232-194">You can find out the time when a managed disk was created from the Azure portal, the Azure CLI, and PowerShell.</span></span> <span data-ttu-id="36232-195">Ha az időpont után 2017. június 9., a lemez titkosítva.</span><span class="sxs-lookup"><span data-stu-id="36232-195">If the time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="36232-196">**Hogyan titkosíthatja a meglévő lemezek 2017. június 10. előtt létrehozott?**</span><span class="sxs-lookup"><span data-stu-id="36232-196">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="36232-197">Től 2017. június 10. a meglévő felügyelt lemezekre írt új adatokat automatikusan titkosítva.</span><span class="sxs-lookup"><span data-stu-id="36232-197">As of June 10, 2017, new data written to existing managed disks is automatically encrypted.</span></span> <span data-ttu-id="36232-198">Azt is tervezi, hogy a meglévő adatok titkosítása, és a titkosítás aszinkron módon történik a háttérben.</span><span class="sxs-lookup"><span data-stu-id="36232-198">We are also planning to encrypt existing data, and the encryption will happen asynchronously in the background.</span></span> <span data-ttu-id="36232-199">Ha meglévő adatokat most titkosítani kell, hozzon létre egy másolatot a lemezen.</span><span class="sxs-lookup"><span data-stu-id="36232-199">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="36232-200">Új lemez titkosítva legyen.</span><span class="sxs-lookup"><span data-stu-id="36232-200">New disks will be encrypted.</span></span>

* [<span data-ttu-id="36232-201">Felügyelt lemezeket másolja az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="36232-201">Copy managed disks by using the Azure CLI</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="36232-202">Felügyelt lemezeket másolja a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="36232-202">Copy managed disks by using PowerShell</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="36232-203">**Felügyelt pillanatfelvételek és lemezképek titkosítva van?**</span><span class="sxs-lookup"><span data-stu-id="36232-203">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="36232-204">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-204">Yes.</span></span> <span data-ttu-id="36232-205">Az összes kezelt pillanatképeket és lemezképek létrehozása után 2017. június 9., automatikusan titkosítva vannak.</span><span class="sxs-lookup"><span data-stu-id="36232-205">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="36232-206">**Is virtuális gépek is átalakítása nem felügyelt a storage-fiókok vagy kezelt lemezek korábban titkosított lévő lemezek?**</span><span class="sxs-lookup"><span data-stu-id="36232-206">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted to managed disks?**</span></span>

<span data-ttu-id="36232-207">Igen</span><span class="sxs-lookup"><span data-stu-id="36232-207">Yes</span></span>

<span data-ttu-id="36232-208">**A felügyelt lemezes vagy a pillanatképet egy exportált VHD-t is titkosítva lesznek?**</span><span class="sxs-lookup"><span data-stu-id="36232-208">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="36232-209">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-209">No.</span></span> <span data-ttu-id="36232-210">De ha egy virtuális Merevlemezt egy titkosított tárfiókba történő exportálását egy titkosított kezelt lemez vagy a pillanatképet, majd titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="36232-210">But if you export a VHD to an encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="36232-211">Prémium szintű lemezekhez: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="36232-211">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="36232-212">**Egy virtuális gép használ, amely támogatja a prémium szintű Storage, például egy DSv2 mérete több premium és a szabványos adatlemezek csatolható?**</span><span class="sxs-lookup"><span data-stu-id="36232-212">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="36232-213">Igen.</span><span class="sxs-lookup"><span data-stu-id="36232-213">Yes.</span></span>

<span data-ttu-id="36232-214">**Csatolható prémium és standard adatlemezek is, amely nem támogatja a prémium szintű Storage, például a D, Dv2, G vagy F adatsorozat mérete több?**</span><span class="sxs-lookup"><span data-stu-id="36232-214">**Can I attach both premium and standard data disks to a size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="36232-215">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-215">No.</span></span> <span data-ttu-id="36232-216">Csak a standard adatlemezek csatolhat a virtuális gépek, amelyek nem használják, amely támogatja a prémium szintű Storage mérete több.</span><span class="sxs-lookup"><span data-stu-id="36232-216">You can attach only standard data disks to VMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="36232-217">**Ha egy meglévő virtuális merevlemezről, de a 80 GB prémium adatlemezt hozok létre, mennyire, amely ára?**</span><span class="sxs-lookup"><span data-stu-id="36232-217">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="36232-218">80 GB-os virtuális merevlemezről létrehozott prémium adatlemezt a rendszer a következő érhető el prémium szintű lemez méretet, amelyet P10 lemez.</span><span class="sxs-lookup"><span data-stu-id="36232-218">A premium data disk created from an 80-GB VHD is treated as the next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="36232-219">Most a P10 lemez díjszabás alapján számítjuk fel.</span><span class="sxs-lookup"><span data-stu-id="36232-219">You're charged according to the P10 disk pricing.</span></span>

<span data-ttu-id="36232-220">**Vannak-e a prémium szintű Storage tranzakciós költségek?**</span><span class="sxs-lookup"><span data-stu-id="36232-220">**Are there transaction costs to use Premium Storage?**</span></span>

<span data-ttu-id="36232-221">Nincs minden lemez méretét, amelyet adott határral kiosztott iops-érték és az átvitel meg egy rögzített költséget.</span><span class="sxs-lookup"><span data-stu-id="36232-221">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="36232-222">Az egyéb költségekkel kimenő sávszélesség és a pillanatkép-kapacitást, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="36232-222">The other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="36232-223">További tájékoztatás a [díjszabási lapon](https://azure.microsoft.com/pricing/details/storage) olvasható.</span><span class="sxs-lookup"><span data-stu-id="36232-223">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="36232-224">**Az iops-érték és a teljesítményt, amelyek szeretnék lekérheti a lemezgyorsítótár korlátai**</span><span class="sxs-lookup"><span data-stu-id="36232-224">**What are the limits for IOPS and throughput that I can get from the disk cache?**</span></span>

<span data-ttu-id="36232-225">A kombinált korlátozhatja a gyorsítótár és a DS-ben több helyi SSD is core 4000 iops-értéket core másodpercenként 33 MB.</span><span class="sxs-lookup"><span data-stu-id="36232-225">The combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="36232-226">A GS adatsorozat 5000 iops teljesítményt nyújtanak core és 50 MB / s / core kínál.</span><span class="sxs-lookup"><span data-stu-id="36232-226">The GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="36232-227">**A lemezek felügyelt virtuális gépek támogatja a helyi SSD?**</span><span class="sxs-lookup"><span data-stu-id="36232-227">**Is the local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="36232-228">A helyi SSD az ideiglenes tároló, a lemezek felügyelt virtuális gépek részét képező.</span><span class="sxs-lookup"><span data-stu-id="36232-228">The local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="36232-229">Nincs ingyenesen a ideiglenes tárolására.</span><span class="sxs-lookup"><span data-stu-id="36232-229">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="36232-230">Azt javasoljuk, hogy nem használja a helyi SSD az alkalmazásadatok tárolására, mert azt az Azure Blob storage nem megőrzött.</span><span class="sxs-lookup"><span data-stu-id="36232-230">We recommend that you do not use this local SSD to store your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="36232-231">**Vannak-e bármilyen következményekkel VÁGÁSHOZ a premium lemezek használatát?**</span><span class="sxs-lookup"><span data-stu-id="36232-231">**Are there any repercussions for the use of TRIM on premium disks?**</span></span>

<span data-ttu-id="36232-232">Nincs vágás használatához Azure-lemezeket vagy Premium vagy a standard lemezek hátránya van.</span><span class="sxs-lookup"><span data-stu-id="36232-232">There is no downside to the use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="36232-233">Új lemezméret: felügyelt és nem felügyelt</span><span class="sxs-lookup"><span data-stu-id="36232-233">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="36232-234">**Mi az a legnagyobb lemez méretét a támogatott operációs rendszer és az adatlemezek?**</span><span class="sxs-lookup"><span data-stu-id="36232-234">**What is the largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="36232-235">A partíció típusa, amely Azure támogatja az operációsrendszer-lemez a fő rendszerindító rekord (MBR).</span><span class="sxs-lookup"><span data-stu-id="36232-235">The partition type that Azure supports for an operating system disk is the master boot record (MBR).</span></span> <span data-ttu-id="36232-236">Egy lemezt a fő rendszertöltő rekord formátum támogatja legfeljebb 2 TB-os méret.</span><span class="sxs-lookup"><span data-stu-id="36232-236">The MBR format supports a disk size up to 2 TB.</span></span> <span data-ttu-id="36232-237">A legnagyobb, amely Azure támogatja az operációsrendszer-lemez mérete 2 TB.</span><span class="sxs-lookup"><span data-stu-id="36232-237">The largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="36232-238">Azure akár 4 TB adatlemezek támogatja.</span><span class="sxs-lookup"><span data-stu-id="36232-238">Azure supports up to 4 TB for data disks.</span></span> 

<span data-ttu-id="36232-239">**Mi az a legnagyobb blob mérete támogatott?**</span><span class="sxs-lookup"><span data-stu-id="36232-239">**What is the largest page blob size that's supported?**</span></span>

<span data-ttu-id="36232-240">A legnagyobb blob mérete, amely támogatja az Azure rendszeren 8 TB (8,191 GB).</span><span class="sxs-lookup"><span data-stu-id="36232-240">The largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="36232-241">Nagyobb, mint 4 TB-os (4095 GB) egy virtuális Géphez csatlakozik, adatok vagy az operációs rendszer lemezek lapblobokat nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="36232-241">We don't support page blobs larger than 4 TB (4,095 GB) attached to a VM as data or operating system disks.</span></span>

<span data-ttu-id="36232-242">**Kell létrehozni, csatolja, átméretezése és 1 TB-nál nagyobb lemezek feltöltése Azure eszközök új verziójának segítségével?**</span><span class="sxs-lookup"><span data-stu-id="36232-242">**Do I need to use a new version of Azure tools to create, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="36232-243">Nem kell frissíteni a meglévő Azure-eszközök létrehozására, csatolja, és 1 TB-nál nagyobb lemezek átméretezése.</span><span class="sxs-lookup"><span data-stu-id="36232-243">You don't need to upgrade your existing Azure tools to create, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="36232-244">Töltse fel a VHD-fájl a helyszíni közvetlenül az Azure oldalakra vonatkozó blob vagy nem felügyelt lemezt, kell használnia az eszköz legújabb beállítása:</span><span class="sxs-lookup"><span data-stu-id="36232-244">To upload your VHD file from on-premises directly to Azure as a page blob or unmanaged disk, you need to use the latest tool sets:</span></span>

|<span data-ttu-id="36232-245">Azure-eszközök</span><span class="sxs-lookup"><span data-stu-id="36232-245">Azure tools</span></span>      | <span data-ttu-id="36232-246">Támogatott verziók</span><span class="sxs-lookup"><span data-stu-id="36232-246">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="36232-247">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="36232-247">Azure PowerShell</span></span> | <span data-ttu-id="36232-248">4.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="36232-248">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="36232-249">Az Azure CLI 1-es verzió</span><span class="sxs-lookup"><span data-stu-id="36232-249">Azure CLI v1</span></span>     | <span data-ttu-id="36232-250">Verziószám 0.10.13: Előfordulhat, hogy 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="36232-250">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="36232-251">AzCopy</span><span class="sxs-lookup"><span data-stu-id="36232-251">AzCopy</span></span>           | <span data-ttu-id="36232-252">6.1.0 verziószám: június 2017 kiadás vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="36232-252">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="36232-253">Az Azure parancssori felület v2 és Azure Tártallózó támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="36232-253">The support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="36232-254">**Nem felügyelt lemezek vagy lapblobokat támogatott P4 és P6 mérete?**</span><span class="sxs-lookup"><span data-stu-id="36232-254">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="36232-255">Nem.</span><span class="sxs-lookup"><span data-stu-id="36232-255">No.</span></span> <span data-ttu-id="36232-256">P4 (32 GB) és P6 (64 GB-os) lemezméret csak felügyelt lemezek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="36232-256">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="36232-257">Nem felügyelt lemezek és a lapblobokat támogatása hamarosan elérhető.</span><span class="sxs-lookup"><span data-stu-id="36232-257">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="36232-258">**Ha a meglévő prémium szintű felügyelt lemez kevesebb, mint 64 GB-os jött létre a kisebb lemezt (körülbelül 2017. június 15.) engedélyezése előtt, hogyan azt számlázása?**</span><span class="sxs-lookup"><span data-stu-id="36232-258">**If my existing premium managed disk less than 64 GB was created before the small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="36232-259">Meglévő kis premium lemezek kisebb, mint 64 GB-os továbbra is fizetnie kell ezért a P10 tarifacsomag alapján.</span><span class="sxs-lookup"><span data-stu-id="36232-259">Existing small premium disks less than 64 GB continue to be billed according to the P10 pricing tier.</span></span> 

<span data-ttu-id="36232-260">**Hogyan válthat a lemez réteg P4 vagy P6 P10 a 64 GB-nál kisebb kis premium lemezek?**</span><span class="sxs-lookup"><span data-stu-id="36232-260">**How can I switch the disk tier of small premium disks less than 64 GB from P10 to P4 or P6?**</span></span>

<span data-ttu-id="36232-261">Pillanatkép készítése a kis kapacitású lemezeket, és ezután automatikusan Váltás az árképzési szint P4 vagy a kiosztott mérete alapján P6 lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="36232-261">You can take a snapshot of your small disks and then create a disk to automatically switch the pricing tier to P4 or P6 based on the provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="36232-262">Mi történik, ha a fentiekben itt nem választ?</span><span class="sxs-lookup"><span data-stu-id="36232-262">What if my question isn't answered here?</span></span>

<span data-ttu-id="36232-263">Ha kérdését itt nem látható, ossza meg velünk, és segítünk talált választ.</span><span class="sxs-lookup"><span data-stu-id="36232-263">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="36232-264">A megjegyzések is közzétesz egy kérdést, ez a cikk végén.</span><span class="sxs-lookup"><span data-stu-id="36232-264">You can post a question at the end of this article in the comments.</span></span> <span data-ttu-id="36232-265">Az Azure Storage csapat és a Közösség más tagjaival kapcsolatos cikkben bevonásához, használja az MSDN [Azure Storage fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="36232-265">To engage with the Azure Storage team and other community members about this article, use the MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="36232-266">Kérelem szolgáltatások, a kérelmek és ötleteket a a [Azure Storage-visszajelzési fórumon](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="36232-266">To request features, submit your requests and ideas to the [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>

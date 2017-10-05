---
title: "Hozzon létre egy Azure felügyelt lemezt biztonsági másolatát mentése |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Azure felügyelt lemezt biztonsági vagy a lemez problémák elhárításához."
documentationcenter: 
author: cwatson-cat
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 2/9/2017
ms.author: cwatson
ms.openlocfilehash: a7527b12f4f0d2b45713a0c0109d81ff51293fd8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="468c1-103">Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása</span><span class="sxs-lookup"><span data-stu-id="468c1-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="468c1-104">Pillanatkép készítése a biztonsági mentéshez kezelt lemez vagy hozhat létre egy felügyelt lemezt a pillanatkép, és csatlakoztassa azt a teszt virtuális gép a hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="468c1-104">Take a snapshot of a Managed Disk for backup or create a Managed Disk from the snapshot and attach it to a test virtual machine to troubleshoot.</span></span> <span data-ttu-id="468c1-105">Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát.</span><span class="sxs-lookup"><span data-stu-id="468c1-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="468c1-106">Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="468c1-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> <span data-ttu-id="468c1-107">Felügyelt lemezek kapcsolatos további információkért lásd: [Azure felügyelt lemezekhez – áttekintés](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="468c1-107">For more information about Managed Disks, see [Azure Managed Disks overview](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="468c1-108">További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="468c1-108">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="468c1-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="468c1-109">Before you begin</span></span>
<span data-ttu-id="468c1-110">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult.</span><span class="sxs-lookup"><span data-stu-id="468c1-110">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="468c1-111">A következő parancsot a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="468c1-111">Run the following command to install it.</span></span>

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="468c1-112">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="468c1-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>

## <a name="copy-the-vhd-with-a-snapshot"></a><span data-ttu-id="468c1-113">Pillanatkép virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="468c1-113">Copy the VHD with a snapshot</span></span>
<span data-ttu-id="468c1-114">Az Azure portálon vagy a PowerShell segítségével készítsen pillanatképet a kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="468c1-114">Use either the Azure portal or PowerShell to take a snapshot of the Managed Disk.</span></span>

### <a name="use-azure-portal-to-take-a-snapshot"></a><span data-ttu-id="468c1-115">Készítsen pillanatképet az Azure portál használatával</span><span class="sxs-lookup"><span data-stu-id="468c1-115">Use Azure portal to take a snapshot</span></span> 

1. <span data-ttu-id="468c1-116">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="468c1-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="468c1-117">Kezdési bal felső sarokban, kattintson a **új** keresse meg a **pillanatkép**.</span><span class="sxs-lookup"><span data-stu-id="468c1-117">Starting in the upper left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="468c1-118">A pillanatkép paneljén kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="468c1-118">In the Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="468c1-119">Adjon meg egy **neve** a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="468c1-119">Enter a **Name** for the snapshot.</span></span>
5. <span data-ttu-id="468c1-120">Válasszon ki egy létező [Erőforráscsoportot](../../azure-resource-manager/resource-group-overview.md#resource-groups), vagy adja meg egy új csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="468c1-120">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span> 
6. <span data-ttu-id="468c1-121">Válasszon egy Azure-adatközpontban helyét.</span><span class="sxs-lookup"><span data-stu-id="468c1-121">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="468c1-122">A **forráslemez**, válassza ki a felügyelt lemezt pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="468c1-122">For **Source disk**, select the Managed Disk to snapshot.</span></span>
8. <span data-ttu-id="468c1-123">Válassza ki a **fiók típusa** használni a pillanatkép tárolására.</span><span class="sxs-lookup"><span data-stu-id="468c1-123">Select the **Account type** to use to store the snapshot.</span></span> <span data-ttu-id="468c1-124">Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.</span><span class="sxs-lookup"><span data-stu-id="468c1-124">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="468c1-125">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="468c1-125">Click **Create**.</span></span>

### <a name="use-powershell-to-take-a-snapshot"></a><span data-ttu-id="468c1-126">Pillanatkép készítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="468c1-126">Use PowerShell to take a snapshot</span></span>
<span data-ttu-id="468c1-127">A következő lépések bemutatják a lekérése a VHD lemez másolni, hozzon létre a pillanatkép-konfigurációkat és pillanatképet készít, a lemez a New-AzureRmSnapshot parancsmaggal<!--Add link to cmdlet when available-->.</span><span class="sxs-lookup"><span data-stu-id="468c1-127">The following steps show you how to get the VHD disk to be copied, create the snapshot configurations, and take a snapshot of the disk by using the New-AzureRmSnapshot cmdlet<!--Add link to cmdlet when available-->.</span></span> 

1. <span data-ttu-id="468c1-128">Egyes paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="468c1-128">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  <span data-ttu-id="468c1-129">Cserélje le a paraméterértékeket:</span><span class="sxs-lookup"><span data-stu-id="468c1-129">Replace the parameter values:</span></span>
  -  <span data-ttu-id="468c1-130">a virtuális gép erőforráscsoporttal "contoso.com".</span><span class="sxs-lookup"><span data-stu-id="468c1-130">"myResourceGroup" with the VM's resource group.</span></span>
  -  <span data-ttu-id="468c1-131">az a földrajzi hely, ahol azt szeretné, hogy a felügyelt pillanatkép tárolt "southeastasia".</span><span class="sxs-lookup"><span data-stu-id="468c1-131">"southeastasia" with the geographic location where you want your Managed Snapshot stored.</span></span> <!---How do you look these up? -->
  -  <span data-ttu-id="468c1-132">"ContosoMD_datadisk1" nevű, a VHD lemez, amelyeket másolni kíván.</span><span class="sxs-lookup"><span data-stu-id="468c1-132">"ContosoMD_datadisk1" with the name of the VHD disk that you want to copy.</span></span>
  -  <span data-ttu-id="468c1-133">Az új pillanatkép használni kívánt "ContosoMD_datadisk1_snapshot1" néven.</span><span class="sxs-lookup"><span data-stu-id="468c1-133">"ContosoMD_datadisk1_snapshot1" with the name you want to use for the new snapshot.</span></span>

2. <span data-ttu-id="468c1-134">A VHD lemez másolandó beolvasása.</span><span class="sxs-lookup"><span data-stu-id="468c1-134">Get the VHD disk to be copied.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. <span data-ttu-id="468c1-135">A pillanatkép-konfigurációk létrehozása.</span><span class="sxs-lookup"><span data-stu-id="468c1-135">Create the snapshot configurations.</span></span> 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. <span data-ttu-id="468c1-136">A pillanatkép elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="468c1-136">Take the snapshot.</span></span>

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
<span data-ttu-id="468c1-137">Ha azt tervezi, a pillanatkép segítségével kezelt lemez létrehozása, és csatlakoztassa a virtuális gépek magas végrehajtása szükséges, használja a paraméterrel `-AccountType Premium_LRS` a New-AzureRmSnapshot paranccsal.</span><span class="sxs-lookup"><span data-stu-id="468c1-137">If you plan to use the snapshot to create a Managed Disk and attach it a VM that needs to be high performing, use the parameter `-AccountType Premium_LRS` with the New-AzureRmSnapshot command.</span></span> <span data-ttu-id="468c1-138">A paraméter a pillanatkép hoz, hogy prémium felügyelt lemezként tárolja.</span><span class="sxs-lookup"><span data-stu-id="468c1-138">The parameter creates the snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="468c1-139">Prémium szintű kezelt lemezek drágább szabvány.</span><span class="sxs-lookup"><span data-stu-id="468c1-139">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="468c1-140">Ezért mindenképpen valóban szükség van prémium, hogy a paraméter használata előtt.</span><span class="sxs-lookup"><span data-stu-id="468c1-140">So be sure you really need Premium before using that parameter.</span></span>



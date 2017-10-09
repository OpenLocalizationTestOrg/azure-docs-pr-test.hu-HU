---
title: "egy-egy példányát az Azure kezelt lemez a biztonságimásolat-aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy példányát az Azure Managed lemez toouse vissza felfelé vagy hibaelhárítási lemez ad ki."
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
ms.openlocfilehash: 2f33dbbee624bcd813f3c7c3e3401072d0933714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-vhd-stored-as-an-azure-managed-disk-by-using-managed-snapshots"></a><span data-ttu-id="f332d-103">Felügyelt pillanatképek használatával Azure felügyelt lemezként tárolt virtuális merevlemez másolatának létrehozása</span><span class="sxs-lookup"><span data-stu-id="f332d-103">Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots</span></span>
<span data-ttu-id="f332d-104">Pillanatképet készít, egy felügyelt lemezt a biztonsági mentéshez vagy hello pillanatképből kezelt lemez létrehozása, és mellékelje tooa teszt virtuális gép tootroubleshoot.</span><span class="sxs-lookup"><span data-stu-id="f332d-104">Take a snapshot of a Managed Disk for backup or create a Managed Disk from hello snapshot and attach it tooa test virtual machine tootroubleshoot.</span></span> <span data-ttu-id="f332d-105">Felügyelt pillanatkép egy virtuális gép kezelt lemez teljes-időpontban másolatát.</span><span class="sxs-lookup"><span data-stu-id="f332d-105">A Managed Snapshot is a full point-in-time copy of a VM Managed Disk.</span></span> <span data-ttu-id="f332d-106">Azt a virtuális merevlemez csak olvasható másolatát hozza létre, és alapértelmezés szerint tárolja azt, Standard szintű felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="f332d-106">It creates a read-only copy of your VHD and, by default, stores it as a Standard Managed Disk.</span></span> <span data-ttu-id="f332d-107">Felügyelt lemezek kapcsolatos további információkért lásd: [Azure felügyelt lemezekhez – áttekintés](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f332d-107">For more information about Managed Disks, see [Azure Managed Disks overview](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="f332d-108">További információk a díjszabásról: [Azure Storage szolgáltatás díjszabása](https://azure.microsoft.com/pricing/details/managed-disks/).</span><span class="sxs-lookup"><span data-stu-id="f332d-108">For information about pricing, see [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/managed-disks/).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="f332d-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="f332d-109">Before you begin</span></span>
<span data-ttu-id="f332d-110">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="f332d-110">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="f332d-111">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="f332d-111">Run hello following command tooinstall it.</span></span>

```
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="f332d-112">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f332d-112">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>

## <a name="copy-hello-vhd-with-a-snapshot"></a><span data-ttu-id="f332d-113">Másolja a VHD hello egy pillanatképet</span><span class="sxs-lookup"><span data-stu-id="f332d-113">Copy hello VHD with a snapshot</span></span>
<span data-ttu-id="f332d-114">Használja a hello Azure-portálon vagy a PowerShell tootake hello kezelt lemez pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="f332d-114">Use either hello Azure portal or PowerShell tootake a snapshot of hello Managed Disk.</span></span>

### <a name="use-azure-portal-tootake-a-snapshot"></a><span data-ttu-id="f332d-115">Az Azure portál tootake pillanatkép használata</span><span class="sxs-lookup"><span data-stu-id="f332d-115">Use Azure portal tootake a snapshot</span></span> 

1. <span data-ttu-id="f332d-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f332d-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f332d-117">Kattintson a bal felső hello kezdődően **új** keresse meg a **pillanatkép**.</span><span class="sxs-lookup"><span data-stu-id="f332d-117">Starting in hello upper left, click **New** and search for **snapshot**.</span></span>
3. <span data-ttu-id="f332d-118">Hello pillanatkép paneljén kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f332d-118">In hello Snapshot blade, click **Create**.</span></span>
4. <span data-ttu-id="f332d-119">Adjon meg egy **neve** hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="f332d-119">Enter a **Name** for hello snapshot.</span></span>
5. <span data-ttu-id="f332d-120">Válasszon ki egy létező [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="f332d-120">Select an existing [Resource group](../../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span> 
6. <span data-ttu-id="f332d-121">Válasszon egy Azure-adatközpontban helyét.</span><span class="sxs-lookup"><span data-stu-id="f332d-121">Select an Azure datacenter Location.</span></span>  
7. <span data-ttu-id="f332d-122">A **forráslemez**, válassza ki a kezelt lemez toosnapshot hello.</span><span class="sxs-lookup"><span data-stu-id="f332d-122">For **Source disk**, select hello Managed Disk toosnapshot.</span></span>
8. <span data-ttu-id="f332d-123">Jelölje be hello **fiók típusa** toouse toostore hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="f332d-123">Select hello **Account type** toouse toostore hello snapshot.</span></span> <span data-ttu-id="f332d-124">Ajánlott **Standard_LRS** kivéve, ha esetleg szükség lenne rá egy nagy teljesítményű lemezen tárolja.</span><span class="sxs-lookup"><span data-stu-id="f332d-124">We recommend **Standard_LRS** unless you need it stored on a high performing disk.</span></span>
9. <span data-ttu-id="f332d-125">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f332d-125">Click **Create**.</span></span>

### <a name="use-powershell-tootake-a-snapshot"></a><span data-ttu-id="f332d-126">PowerShell tootake pillanatkép használata</span><span class="sxs-lookup"><span data-stu-id="f332d-126">Use PowerShell tootake a snapshot</span></span>
<span data-ttu-id="f332d-127">hello következő lépések bemutatják, hogyan tooget hello VHD lemezre másolt toobe hello pillanatkép-konfigurációk létrehozása és pillanatképet hello lemez hello New-AzureRmSnapshot parancsmaggal<!--Add link toocmdlet when available-->.</span><span class="sxs-lookup"><span data-stu-id="f332d-127">hello following steps show you how tooget hello VHD disk toobe copied, create hello snapshot configurations, and take a snapshot of hello disk by using hello New-AzureRmSnapshot cmdlet<!--Add link toocmdlet when available-->.</span></span> 

1. <span data-ttu-id="f332d-128">Egyes paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="f332d-128">Set some parameters.</span></span> 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$location = 'southeastasia' 
$dataDiskName = 'ContosoMD_datadisk1' 
$snapshotName = 'ContosoMD_datadisk1_snapshot1'  
```
  <span data-ttu-id="f332d-129">Cserélje le a paraméterértékeket hello:</span><span class="sxs-lookup"><span data-stu-id="f332d-129">Replace hello parameter values:</span></span>
  -  <span data-ttu-id="f332d-130">"contoso.com" hello VM erőforrás csoporttal.</span><span class="sxs-lookup"><span data-stu-id="f332d-130">"myResourceGroup" with hello VM's resource group.</span></span>
  -  <span data-ttu-id="f332d-131">"southeastasia" hello földrajzi helyét, a felügyelt pillanatkép tárolása az.</span><span class="sxs-lookup"><span data-stu-id="f332d-131">"southeastasia" with hello geographic location where you want your Managed Snapshot stored.</span></span> <!---How do you look these up? -->
  -  <span data-ttu-id="f332d-132">"ContosoMD_datadisk1" hello nevű hello VHD lemez, amelyet az toocopy.</span><span class="sxs-lookup"><span data-stu-id="f332d-132">"ContosoMD_datadisk1" with hello name of hello VHD disk that you want toocopy.</span></span>
  -  <span data-ttu-id="f332d-133">"ContosoMD_datadisk1_snapshot1" hello a nevét, toouse szánt hello új pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="f332d-133">"ContosoMD_datadisk1_snapshot1" with hello name you want toouse for hello new snapshot.</span></span>

2. <span data-ttu-id="f332d-134">Hello VHD lemez toobe másolt beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f332d-134">Get hello VHD disk toobe copied.</span></span>

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $dataDiskName 
```
3. <span data-ttu-id="f332d-135">Hozzon létre hello pillanatkép-konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="f332d-135">Create hello snapshot configurations.</span></span> 

 ```powershell
$snapshot =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location $location 
```
4. <span data-ttu-id="f332d-136">Hello pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="f332d-136">Take hello snapshot.</span></span>

 ```powershell
New-AzureRmSnapshot -Snapshot $snapshot -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName 
```
<span data-ttu-id="f332d-137">Ha toouse hello pillanatkép toocreate tervezze meg a felügyelt lemez, és csatlakoztassa a virtuális gép, amelyet a toobe magas hajt végre, használja a hello paramétert `-AccountType Premium_LRS` a New-AzureRmSnapshot hello paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f332d-137">If you plan toouse hello snapshot toocreate a Managed Disk and attach it a VM that needs toobe high performing, use hello parameter `-AccountType Premium_LRS` with hello New-AzureRmSnapshot command.</span></span> <span data-ttu-id="f332d-138">hello paraméter hello pillanatképet hoz, hogy a prémium felügyelt lemezként tárolja.</span><span class="sxs-lookup"><span data-stu-id="f332d-138">hello parameter creates hello snapshot so that it's stored as a Premium Managed Disk.</span></span> <span data-ttu-id="f332d-139">Prémium szintű kezelt lemezek drágább szabvány.</span><span class="sxs-lookup"><span data-stu-id="f332d-139">Premium Managed Disks are more expensive than Standard.</span></span> <span data-ttu-id="f332d-140">Ezért mindenképpen valóban szükség van prémium, hogy a paraméter használata előtt.</span><span class="sxs-lookup"><span data-stu-id="f332d-140">So be sure you really need Premium before using that parameter.</span></span>



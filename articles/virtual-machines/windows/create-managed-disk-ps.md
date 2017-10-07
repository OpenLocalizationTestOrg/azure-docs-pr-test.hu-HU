---
title: "az egy VHD-ről az Azure-ban felügyelt lemezes aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy felügyelt lemezes virtuális merevlemez jelenleg egy Azure storage-fiók használatával hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: cynthn
ms.openlocfilehash: 77adaac5419186ff85039fe2c4752f021aa5e448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="e369a-103">Felügyelt lemezek tárfiókokban nem felügyelt lemezekből létrehozása</span><span class="sxs-lookup"><span data-stu-id="e369a-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="e369a-104">Felügyelt lemezes egy meglévő adatlemez vagy egy operációsrendszer-lemez, amely jelenleg egy Azure storage-fiókok hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="e369a-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="e369a-105">Üres lemez, amely használható a virtuális gépek új adatlemezt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="e369a-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="e369a-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e369a-106">Before you begin</span></span>
<span data-ttu-id="e369a-107">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="e369a-107">If you use PowerShell, make sure that you have hello latest version of hello AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="e369a-108">Futtatás hello parancs tooinstall követően.</span><span class="sxs-lookup"><span data-stu-id="e369a-108">Run hello following command tooinstall it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="e369a-109">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e369a-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="e369a-110">Az Azure-tárfiók olyan virtuális merevlemezről kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="e369a-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="e369a-111">Hello példában azt hozhat létre egy felügyelt lemezes virtuális Merevlemezt egy lemezt, és rendelje hozzá toohello paraméter **$ 1. lemez** toouse később.</span><span class="sxs-lookup"><span data-stu-id="e369a-111">In hello example we create a disk from a VHD as managed disk and assign it toohello parameter **$disk1** toouse later.</span></span> 

<span data-ttu-id="e369a-112">hello felügyelt lemezes létrejön hello **nyugati-US** helyét, nevű erőforráscsoport **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="e369a-112">hello managed disk will be created in hello **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="e369a-113">hello lemez neve lehet **myDisk** és a rendszer automatikusan létrehozza a VHD-fájl nevű **myDisk.vhd** azt korábban feltöltött nevű tooa tárfiók **mystorageaccount**.</span><span class="sxs-lookup"><span data-stu-id="e369a-113">hello disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded tooa storage account named **mystorageaccount**.</span></span> <span data-ttu-id="e369a-114">prémium szintű helyileg redundáns tárolás (LRS) hello felügyelt lemezes létrejön.</span><span class="sxs-lookup"><span data-stu-id="e369a-114">hello managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="e369a-115">StandardLRS és PremiumLRS is csak hello **- AccountType** lehetőségeit által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="e369a-115">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="e369a-116">Egyes paraméterek beállítása</span><span class="sxs-lookup"><span data-stu-id="e369a-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="e369a-117">Hello adatok lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e369a-117">Create hello data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="e369a-118">Hozzon létre egy üres adatlemez felügyelt lemezes</span><span class="sxs-lookup"><span data-stu-id="e369a-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="e369a-119">Hello példában azt hozzon létre egy üres adatlemez felügyelt lemezként, és rendelje hozzá toohello paraméter **$dataDisk2** toouse később.</span><span class="sxs-lookup"><span data-stu-id="e369a-119">In hello example we create an empty data disk as managed disk and assign it toohello parameter **$dataDisk2** toouse later.</span></span> <span data-ttu-id="e369a-120">Egy üres adatlemez kell inicializálni toobe toohello VM-naplózás és diskmgmt.msc használatával vagy [Rendszerfelügyeleti webszolgáltatások és egy parancsfájl segítségével távolról](attach-disk-ps.md#initialize-the-disk), ha fut a virtuális gép csatlakoztatott tooa.</span><span class="sxs-lookup"><span data-stu-id="e369a-120">An empty data disk will need toobe initialized logging in toohello VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached tooa running VM.</span></span>

<span data-ttu-id="e369a-121">hello üres adatlemez létrejön hello **nyugati középső Régiójában** helyét, nevű erőforráscsoport **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="e369a-121">hello empty data disk will be created in hello **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="e369a-122">hello lemez neve lehet **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="e369a-122">hello disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="e369a-123">prémium szintű helyileg redundáns tárolás (LRS) hello üres lemez létrejön.</span><span class="sxs-lookup"><span data-stu-id="e369a-123">hello empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="e369a-124">StandardLRS és PremiumLRS is csak hello **- AccountType** lehetőségeit által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="e369a-124">StandardLRS and PremiumLRS are hello only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="e369a-125">Ebben a példában hello lemez mérete 128 GB-os, de kell kiválasztani, amely megfelel a virtuális gépen futó alkalmazások hello igényeinek méretet.</span><span class="sxs-lookup"><span data-stu-id="e369a-125">hello disk size in this example is 128GB, but you should choose a size that meets hello needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="e369a-126">Egyes paraméterek beállítása</span><span class="sxs-lookup"><span data-stu-id="e369a-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="e369a-127">Hello adatok lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e369a-127">Create hello data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="e369a-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e369a-128">Next Steps</span></span>   
- <span data-ttu-id="e369a-129">Ha már van egy virtuális Gépet, akkor [adatlemezzel](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e369a-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>

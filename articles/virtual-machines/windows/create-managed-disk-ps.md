---
title: "Hozzon létre egy felügyelt lemezes az egy VHD-ről az Azure-ban |} Microsoft Docs"
description: "Hozzon létre egy felügyelt lemezes virtuális Merevlemezt, amely jelenleg egy Azure storage-fiók használata a Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: c03ebf73f1090b595149daf2eb3e274b05822f4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-managed-disks-from-unmanaged-disks-in-a-storage-account"></a><span data-ttu-id="b513e-103">Felügyelt lemezek tárfiókokban nem felügyelt lemezekből létrehozása</span><span class="sxs-lookup"><span data-stu-id="b513e-103">Create managed disks from unmanaged disks in a storage account</span></span>

<span data-ttu-id="b513e-104">Felügyelt lemezes egy meglévő adatlemez vagy egy operációsrendszer-lemez, amely jelenleg egy Azure storage-fiókok hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="b513e-104">A managed disk can be created from an existing data disk or an OS disk that is currently in an Azure storage account.</span></span> <span data-ttu-id="b513e-105">Üres lemez, amely használható a virtuális gépek új adatlemezt is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="b513e-105">You can also create an empty disk that can be used as a new data disk for a VM.</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="b513e-106">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b513e-106">Before you begin</span></span>
<span data-ttu-id="b513e-107">Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e a legújabb verzióját a AzureRM.Compute PowerShell-modult.</span><span class="sxs-lookup"><span data-stu-id="b513e-107">If you use PowerShell, make sure that you have the latest version of the AzureRM.Compute PowerShell module.</span></span> <span data-ttu-id="b513e-108">A következő parancsot a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="b513e-108">Run the following command to install it.</span></span>

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
<span data-ttu-id="b513e-109">További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b513e-109">For more information, see [Azure PowerShell Versioning](/powershell/azure/overview).</span></span>


## <a name="create-a-managed-disk-from-a-vhd-in-an-azure-storage-account"></a><span data-ttu-id="b513e-110">Az Azure-tárfiók olyan virtuális merevlemezről kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="b513e-110">Create a managed disk from a VHD in an Azure storage account</span></span>

<span data-ttu-id="b513e-111">A példa azt hozhat létre egy felügyelt lemezes virtuális Merevlemezt egy lemezt, és rendelje hozzá a paraméter **$ 1. lemez** későbbi használat céljából.</span><span class="sxs-lookup"><span data-stu-id="b513e-111">In the example we create a disk from a VHD as managed disk and assign it to the parameter **$disk1** to use later.</span></span> 

<span data-ttu-id="b513e-112">A felügyelt lemezes létrejön a **nyugati-US** helyét, nevű erőforráscsoport **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b513e-112">The managed disk will be created in the **West-US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="b513e-113">A lemez neve lehet **myDisk** és a rendszer automatikusan létrehozza a VHD-fájl nevű **myDisk.vhd** azt korábban feltöltött nevű tárfiók **mystorageaccount**.</span><span class="sxs-lookup"><span data-stu-id="b513e-113">The disk will be named **myDisk** and it will be created from a VHD file named **myDisk.vhd** we previously uploaded to a storage account named **mystorageaccount**.</span></span> <span data-ttu-id="b513e-114">A felügyelt lemezes prémium helyileg redundáns tárolás (LRS) létrejön.</span><span class="sxs-lookup"><span data-stu-id="b513e-114">The managed disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="b513e-115">Az egyetlen StandardLRS és PremiumLRS **- AccountType** lehetőségeit által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="b513e-115">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span> 

1.  <span data-ttu-id="b513e-116">Egyes paraméterek beállítása</span><span class="sxs-lookup"><span data-stu-id="b513e-116">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $diskName = "myDisk"
    $vhdUri = "https://mystorageaccount.blob.core.windows.net/vhds/myDisk.vhd"
    ```

2. <span data-ttu-id="b513e-117">Az adatok lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b513e-117">Create the data disk.</span></span> 
    ```powershell
    $disk1 = New-AzureRmDisk -DiskName $diskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Import -SourceUri $vhdUri) -ResourceGroupName $rgName
    ```
    
    

## <a name="create-an-empty-data-disk-as-a-managed-disk"></a><span data-ttu-id="b513e-118">Hozzon létre egy üres adatlemez felügyelt lemezes</span><span class="sxs-lookup"><span data-stu-id="b513e-118">Create an empty data disk as a managed disk</span></span>

<span data-ttu-id="b513e-119">A példa azt hozzon létre egy üres adatlemez felügyelt lemezként, és rendelje hozzá a paraméter **$dataDisk2** későbbi használat céljából.</span><span class="sxs-lookup"><span data-stu-id="b513e-119">In the example we create an empty data disk as managed disk and assign it to the parameter **$dataDisk2** to use later.</span></span> <span data-ttu-id="b513e-120">Egy üres adatlemez inicializált jelentkezik be a virtuális gép és diskmgmt.msc használatával kell vagy [Rendszerfelügyeleti webszolgáltatások és egy parancsfájl segítségével távolról](attach-disk-ps.md#initialize-the-disk), miután a virtuális gép csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b513e-120">An empty data disk will need to be initialized logging in to the VM and using diskmgmt.msc or [remotely using WinRM and a script](attach-disk-ps.md#initialize-the-disk), once it is attached to a running VM.</span></span>

<span data-ttu-id="b513e-121">Az üres adatlemez létrejön a **nyugati középső Régiójában** helyét, nevű erőforráscsoport **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="b513e-121">The empty data disk will be created in the **West Central US** location, in a resource group named **myResourceGroup**.</span></span> <span data-ttu-id="b513e-122">A lemez neve lehet **myEmptyDataDisk**.</span><span class="sxs-lookup"><span data-stu-id="b513e-122">The disk will be named **myEmptyDataDisk**.</span></span> <span data-ttu-id="b513e-123">Az üres lemez prémium helyileg redundáns tárolás (LRS) létrejön.</span><span class="sxs-lookup"><span data-stu-id="b513e-123">The empty disk will be created in premium locally-redundant storage (LRS).</span></span> <span data-ttu-id="b513e-124">Az egyetlen StandardLRS és PremiumLRS **- AccountType** lehetőségeit által kezelt lemezeken.</span><span class="sxs-lookup"><span data-stu-id="b513e-124">StandardLRS and PremiumLRS are the only **-AccountType** options available for managed disks.</span></span>

<span data-ttu-id="b513e-125">Ebben a példában a lemez mérete 128 GB-os, de a méretet, amely megfelel a virtuális gépen futó alkalmazások igényeinek megfelelően kell kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="b513e-125">The disk size in this example is 128GB, but you should choose a size that meets the needs of any applications running on your VM.</span></span>

1.  <span data-ttu-id="b513e-126">Egyes paraméterek beállítása</span><span class="sxs-lookup"><span data-stu-id="b513e-126">Set some parameters</span></span>

    ```powershell
    $rgName = "myResourceGroup"
    $location = "West Central US"
    $dataDiskName = "myEmptyDataDisk"
    ```

2. <span data-ttu-id="b513e-127">Az adatok lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b513e-127">Create the data disk.</span></span>
    ```powershell
    $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk (New-AzureRmDiskConfig -AccountType PremiumLRS -Location $location -CreateOption Empty -DiskSizeGB 128) -ResourceGroupName $rgName
    ```
    
## <a name="next-steps"></a><span data-ttu-id="b513e-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b513e-128">Next Steps</span></span>   
- <span data-ttu-id="b513e-129">Ha már van egy virtuális Gépet, akkor [adatlemezzel](attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b513e-129">If you already have a VM, you can [attach a data disk](attach-disk-portal.md).</span></span>

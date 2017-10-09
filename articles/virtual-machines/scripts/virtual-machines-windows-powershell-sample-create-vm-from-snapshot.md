---
title: "PowerShell parancsfájl minta - aaaAzure virtuális gép létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – hozzon létre egy virtuális Gépet egy pillanatképből"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 89c65171b55bff0582c4a26df0b0f29f556845fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a><span data-ttu-id="475ef-103">Hozzon létre egy virtuális gépet egy pillanatképből, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="475ef-103">Create a virtual machine from a snapshot with PowerShell</span></span>

<span data-ttu-id="475ef-104">Ez a parancsfájl létrehoz egy virtuális gépet egy operációsrendszer-lemez egy pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="475ef-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="475ef-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="475ef-105">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="475ef-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="475ef-106">Clean up deployment</span></span> 

<span data-ttu-id="475ef-107">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="475ef-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="475ef-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="475ef-108">Script explanation</span></span>

<span data-ttu-id="475ef-109">A parancsfájl a következő parancsok tooget pillanatkép tulajdonságok, hozzon létre egy felügyelt lemezes pillanatképből, és hozzon létre egy virtuális Gépet hello.</span><span class="sxs-lookup"><span data-stu-id="475ef-109">This script uses hello following commands tooget snapshot properties, create a managed disk from snapshot and create a VM.</span></span> <span data-ttu-id="475ef-110">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="475ef-110">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="475ef-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="475ef-111">Command</span></span> | <span data-ttu-id="475ef-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="475ef-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="475ef-113">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="475ef-113">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/get-azurermsnapshot) | <span data-ttu-id="475ef-114">Lekérdezi a pillanatkép-név használatával pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="475ef-114">Gets a snapshot using snapshot name.</span></span> |
| [<span data-ttu-id="475ef-115">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="475ef-115">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/new-azurermdiskconfig) | <span data-ttu-id="475ef-116">Létrehozza a lemezkonfigurációt.</span><span class="sxs-lookup"><span data-stu-id="475ef-116">Creates a disk configuration.</span></span> <span data-ttu-id="475ef-117">Ebben a konfigurációban használatos hello lemez létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="475ef-117">This configuration is used with hello disk creation process.</span></span> |
| [<span data-ttu-id="475ef-118">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="475ef-118">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/new-azurermdisk) | <span data-ttu-id="475ef-119">Egy felügyelt lemezt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="475ef-119">Creates a managed disk.</span></span> |
| [<span data-ttu-id="475ef-120">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="475ef-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="475ef-121">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="475ef-121">Creates a VM configuration.</span></span> <span data-ttu-id="475ef-122">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="475ef-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="475ef-123">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="475ef-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="475ef-124">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="475ef-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="475ef-125">Hello felügyelt lemezes csatolja az operációs rendszer lemez toohello virtuális gépként</span><span class="sxs-lookup"><span data-stu-id="475ef-125">Attaches hello managed disk as OS disk toohello virtual machine</span></span> |
| [<span data-ttu-id="475ef-126">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="475ef-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="475ef-127">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="475ef-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="475ef-128">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="475ef-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="475ef-129">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="475ef-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="475ef-130">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="475ef-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="475ef-131">Létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="475ef-131">Creates a virtual machine.</span></span> |
|[<span data-ttu-id="475ef-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="475ef-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="475ef-133">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="475ef-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="475ef-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="475ef-134">Next steps</span></span>

<span data-ttu-id="475ef-135">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="475ef-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="475ef-136">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="475ef-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

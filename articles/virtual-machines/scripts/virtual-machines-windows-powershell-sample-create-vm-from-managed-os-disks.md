---
title: "Az Azure PowerShell-parancsfájl minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával"
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
ms.openlocfilehash: ec532811e94647c8a04b9faf9474f6749969f83e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a><span data-ttu-id="62f2a-103">Hozzon létre egy virtuális gép egy meglévő felügyelt operációsrendszer-lemez a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="62f2a-103">Create a virtual machine using an existing managed OS disk with PowerShell</span></span>

<span data-ttu-id="62f2a-104">Ez a parancsfájl által az operációs rendszer lemezeként felügyelt meglévő lemez csatolása létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="62f2a-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="62f2a-105">Használja ezt a parancsfájlt előző forgatókönyvek:</span><span class="sxs-lookup"><span data-stu-id="62f2a-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="62f2a-106">Egy meglévő felügyelt operációsrendszer-lemez, amely egy másik előfizetésben található felügyelt lemezes másolta a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="62f2a-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="62f2a-107">Lemezről egy meglévő felügyelt speciális VHD-fájl alapján létrehozott virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="62f2a-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="62f2a-108">Egy meglévő felügyelt operációsrendszer-lemez, amely egy pillanatképből hozták létre a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="62f2a-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="62f2a-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="62f2a-109">Sample script</span></span>

<span data-ttu-id="62f2a-110">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "pillanatképből hozzon létre VM")]</span><span class="sxs-lookup"><span data-stu-id="62f2a-110">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="62f2a-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="62f2a-111">Clean up deployment</span></span> 

<span data-ttu-id="62f2a-112">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="62f2a-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="62f2a-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="62f2a-113">Script explanation</span></span>

<span data-ttu-id="62f2a-114">A parancsfájl a következő parancsokat a felügyelt lemezes tulajdonságait, egy felügyelt lemezt csatolni a új virtuális gép, és hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="62f2a-114">This script uses the following commands to get managed disk properties, attach a managed disk to a new VM and create a VM.</span></span> <span data-ttu-id="62f2a-115">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="62f2a-115">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="62f2a-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="62f2a-116">Command</span></span> | <span data-ttu-id="62f2a-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="62f2a-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="62f2a-118">Get-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="62f2a-118">Get-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Get-AzureRmDisk) | <span data-ttu-id="62f2a-119">Lekérdezi a neve és az erőforráscsoport egy lemez alapú objektumát.</span><span class="sxs-lookup"><span data-stu-id="62f2a-119">Gets disk object based on the name and the resource group of a disk.</span></span> <span data-ttu-id="62f2a-120">A visszaadott objektumát tulajdonsága használt csatolni a lemezt egy új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="62f2a-120">Id property of the returned disk object is used to attach the disk to a new VM</span></span> |
| [<span data-ttu-id="62f2a-121">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="62f2a-121">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="62f2a-122">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="62f2a-122">Creates a VM configuration.</span></span> <span data-ttu-id="62f2a-123">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="62f2a-123">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="62f2a-124">A konfiguráció a Virtuálisgép-létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="62f2a-124">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="62f2a-125">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="62f2a-125">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="62f2a-126">Csatolja az Id tulajdonság, a lemez használatával új virtuális gép operációsrendszer-lemez felügyelt lemezes</span><span class="sxs-lookup"><span data-stu-id="62f2a-126">Attaches a managed disk using the Id property of the disk as OS disk to a new virtual machine</span></span> |
| [<span data-ttu-id="62f2a-127">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="62f2a-127">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="62f2a-128">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="62f2a-128">Creates a public IP address.</span></span> |
| [<span data-ttu-id="62f2a-129">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="62f2a-129">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="62f2a-130">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="62f2a-130">Creates a network interface.</span></span> |
| [<span data-ttu-id="62f2a-131">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="62f2a-131">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="62f2a-132">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="62f2a-132">Create a virtual machine.</span></span> |
|[<span data-ttu-id="62f2a-133">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="62f2a-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="62f2a-134">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="62f2a-134">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="62f2a-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62f2a-135">Next steps</span></span>

<span data-ttu-id="62f2a-136">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="62f2a-136">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="62f2a-137">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62f2a-137">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
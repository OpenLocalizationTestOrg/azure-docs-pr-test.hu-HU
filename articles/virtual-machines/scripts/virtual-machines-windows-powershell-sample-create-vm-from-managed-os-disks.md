---
title: "PowerShell parancsfájl minta - aaaAzure virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával |} Microsoft Docs"
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
ms.openlocfilehash: 8ae5b14df3977a4af91b92692cb925199cfb8058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-powershell"></a><span data-ttu-id="f5d4d-103">Hozzon létre egy virtuális gép egy meglévő felügyelt operációsrendszer-lemez a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f5d4d-103">Create a virtual machine using an existing managed OS disk with PowerShell</span></span>

<span data-ttu-id="f5d4d-104">Ez a parancsfájl által az operációs rendszer lemezeként felügyelt meglévő lemez csatolása létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="f5d4d-105">Használja ezt a parancsfájlt előző forgatókönyvek:</span><span class="sxs-lookup"><span data-stu-id="f5d4d-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="f5d4d-106">Egy meglévő felügyelt operációsrendszer-lemez, amely egy másik előfizetésben található felügyelt lemezes másolta a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5d4d-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="f5d4d-107">Lemezről egy meglévő felügyelt speciális VHD-fájl alapján létrehozott virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5d4d-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="f5d4d-108">Egy meglévő felügyelt operációsrendszer-lemez, amely egy pillanatképből hozták létre a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5d4d-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f5d4d-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f5d4d-109">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="f5d4d-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f5d4d-110">Clean up deployment</span></span> 

<span data-ttu-id="f5d4d-111">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f5d4d-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f5d4d-112">Script explanation</span></span>

<span data-ttu-id="f5d4d-113">A parancsfájl a következő parancsok felügyelt tooget lemez tulajdonságai hello, egy felügyelt lemezes tooa csatolása új virtuális Gépet, és hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="f5d4d-114">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f5d4d-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="f5d4d-115">Command</span></span> | <span data-ttu-id="f5d4d-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f5d4d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f5d4d-117">Get-AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="f5d4d-117">Get-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/Get-AzureRmDisk) | <span data-ttu-id="f5d4d-118">Lekérdezi a hello nevét és a lemez hello erőforráscsoport alapján objektumát.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-118">Gets disk object based on hello name and hello resource group of a disk.</span></span> <span data-ttu-id="f5d4d-119">Hello tulajdonsága visszaadott objektumát használt tooattach hello lemez tooa új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f5d4d-119">Id property of hello returned disk object is used tooattach hello disk tooa new VM</span></span> |
| [<span data-ttu-id="f5d4d-120">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="f5d4d-120">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="f5d4d-121">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-121">Creates a VM configuration.</span></span> <span data-ttu-id="f5d4d-122">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-122">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="f5d4d-123">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-123">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="f5d4d-124">Set-AzureRmVMOSDisk</span><span class="sxs-lookup"><span data-stu-id="f5d4d-124">Set-AzureRmVMOSDisk</span></span>](/powershell/module/azurerm.compute/set-azurermvmosdisk) | <span data-ttu-id="f5d4d-125">Csatolja a felügyelt lemezes tulajdonsággal hello azonosító hello lemez az operációs rendszer lemez tooa új virtuális gépként</span><span class="sxs-lookup"><span data-stu-id="f5d4d-125">Attaches a managed disk using hello Id property of hello disk as OS disk tooa new virtual machine</span></span> |
| [<span data-ttu-id="f5d4d-126">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="f5d4d-126">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="f5d4d-127">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-127">Creates a public IP address.</span></span> |
| [<span data-ttu-id="f5d4d-128">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="f5d4d-128">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="f5d4d-129">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-129">Creates a network interface.</span></span> |
| [<span data-ttu-id="f5d4d-130">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="f5d4d-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="f5d4d-131">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-131">Create a virtual machine.</span></span> |
|[<span data-ttu-id="f5d4d-132">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f5d4d-132">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="f5d4d-133">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="f5d4d-133">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f5d4d-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f5d4d-134">Next steps</span></span>

<span data-ttu-id="f5d4d-135">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f5d4d-135">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f5d4d-136">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f5d4d-136">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

---
title: "Az Azure PowerShell-parancsfájl a minta - OMS |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl a minta - OMS"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: cd23f71221efb62d2547b2b683ca8e2218403a2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="3c362-103">Hozzon létre egy Operations Management Suite figyelt VM a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="3c362-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="3c362-104">Ezt a parancsfájlt hoz létre egy Azure virtuális gépet, telepíti az Operations Management Suite (OMS) ügynök, és regisztrálja a rendszer az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="3c362-104">This script creates an Azure Virtual Machine, installs the Operations Management Suite (OMS) agent, and enrolls the system with an OMS workspace.</span></span> <span data-ttu-id="3c362-105">Miután a parancsfájl lefutott, a virtuális gép lesz látható az OMS-konzolon.</span><span class="sxs-lookup"><span data-stu-id="3c362-105">Once the script has run, the virtual machine will be visible in the OMS console.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3c362-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="3c362-106">Sample script</span></span>

<span data-ttu-id="3c362-107">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.ps1 "VM OMS létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="3c362-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.ps1 "Create VM OMS")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="3c362-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3c362-108">Clean up deployment</span></span> 

<span data-ttu-id="3c362-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="3c362-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="3c362-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3c362-110">Script explanation</span></span>

<span data-ttu-id="3c362-111">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3c362-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="3c362-112">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="3c362-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3c362-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="3c362-113">Command</span></span> | <span data-ttu-id="3c362-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3c362-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3c362-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3c362-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="3c362-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3c362-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3c362-117">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="3c362-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="3c362-118">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="3c362-118">Creates a subnet configuration.</span></span> <span data-ttu-id="3c362-119">Ezt a konfigurációt használja a virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="3c362-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="3c362-120">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="3c362-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="3c362-121">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3c362-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="3c362-122">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="3c362-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="3c362-123">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="3c362-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="3c362-124">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="3c362-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="3c362-125">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="3c362-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="3c362-126">Ez a konfiguráció segítségével egy NSG-szabály létrehozása az NSG létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3c362-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="3c362-127">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="3c362-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="3c362-128">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3c362-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="3c362-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="3c362-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="3c362-130">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="3c362-130">Gets subnet information.</span></span> <span data-ttu-id="3c362-131">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="3c362-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="3c362-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="3c362-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="3c362-133">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="3c362-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="3c362-134">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="3c362-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="3c362-135">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3c362-135">Creates a VM configuration.</span></span> <span data-ttu-id="3c362-136">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="3c362-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="3c362-137">A konfiguráció a Virtuálisgép-létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="3c362-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="3c362-138">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="3c362-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="3c362-139">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="3c362-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="3c362-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="3c362-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="3c362-141">A virtuális gépet egy Virtuálisgép-bővítmény hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3c362-141">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="3c362-142">Ebben az esetben az Operations Management Suite ügynök kiterjesztést az OMS-ügynököt telepíteni és regisztrálni az OMS-munkaterület virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="3c362-142">In this case, the Operations Management Suite agent extension is used to install the OMS agent and enroll the VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="3c362-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="3c362-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="3c362-144">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3c362-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c362-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c362-145">Next steps</span></span>

<span data-ttu-id="3c362-146">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c362-146">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3c362-147">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Linux virtuális dokumentációját](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c362-147">Additional virtual machine PowerShell script samples can be found in the [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

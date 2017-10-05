---
title: "Az Azure PowerShell-parancsfájl a minta - Docker |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl a minta - Docker"
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
ms.date: 03/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c8b700d13e4645d408e4e752a541e521ef93a6e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-docker-host-with-powershell"></a><span data-ttu-id="7eb5f-103">Hozzon létre egy Docker-állomás a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7eb5f-103">Create a Docker host with PowerShell</span></span>

<span data-ttu-id="7eb5f-104">Ezt a parancsfájlt hoz létre egy virtuális gép Docker engedélyezve van, és elindítja egy futó NGINX tároló.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-104">This script creates a virtual machine with Docker enabled and starts a container running NGINX.</span></span> <span data-ttu-id="7eb5f-105">A parancsfájl futtatása után az Azure virtuális gép teljes Tartományneve keresztül érheti el az NGINX-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-105">After running the script, you can access the NGINX web server through the FQDN of the Azure virtual machine.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7eb5f-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7eb5f-106">Sample script</span></span>

<span data-ttu-id="7eb5f-107">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-docker-host/create-docker-host.ps1 "létrehozása Docker-állomás")]</span><span class="sxs-lookup"><span data-stu-id="7eb5f-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-docker-host/create-docker-host.ps1 "Create Docker host")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="7eb5f-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7eb5f-108">Clean up deployment</span></span> 

<span data-ttu-id="7eb5f-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="7eb5f-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7eb5f-110">Script explanation</span></span>

<span data-ttu-id="7eb5f-111">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="7eb5f-112">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7eb5f-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="7eb5f-113">Command</span></span> | <span data-ttu-id="7eb5f-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7eb5f-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7eb5f-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7eb5f-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="7eb5f-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="7eb5f-117">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="7eb5f-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="7eb5f-118">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-118">Creates a subnet configuration.</span></span> <span data-ttu-id="7eb5f-119">Ezt a konfigurációt használja a virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="7eb5f-120">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="7eb5f-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="7eb5f-121">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="7eb5f-122">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="7eb5f-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="7eb5f-123">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="7eb5f-124">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="7eb5f-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="7eb5f-125">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="7eb5f-126">Ez a konfiguráció segítségével egy NSG-szabály létrehozása az NSG létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="7eb5f-127">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="7eb5f-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="7eb5f-128">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="7eb5f-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="7eb5f-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="7eb5f-130">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-130">Gets subnet information.</span></span> <span data-ttu-id="7eb5f-131">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="7eb5f-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="7eb5f-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="7eb5f-133">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="7eb5f-134">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="7eb5f-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="7eb5f-135">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-135">Creates a VM configuration.</span></span> <span data-ttu-id="7eb5f-136">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="7eb5f-137">A konfiguráció a Virtuálisgép-létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="7eb5f-138">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="7eb5f-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="7eb5f-139">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="7eb5f-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="7eb5f-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="7eb5f-141">A virtuális gépet egy Virtuálisgép-bővítmény hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-141">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="7eb5f-142">Ez a példa a Docker kiterjesztését Docker konfigurálására és futtatására egy NGINX Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-142">In this sample, the Docker extension is used to configure Docker and run an NGINX Docker container.</span></span> |
|[<span data-ttu-id="7eb5f-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="7eb5f-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="7eb5f-144">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="7eb5f-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7eb5f-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7eb5f-145">Next steps</span></span>

<span data-ttu-id="7eb5f-146">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7eb5f-146">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7eb5f-147">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Linux virtuális dokumentációját](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7eb5f-147">Additional virtual machine PowerShell script samples can be found in the [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

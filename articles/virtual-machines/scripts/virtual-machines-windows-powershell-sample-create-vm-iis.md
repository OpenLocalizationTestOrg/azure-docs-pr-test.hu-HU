---
title: "Az Azure PowerShell-parancsfájl a minta - IIS |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl a minta - IIS"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1f2f6301267c43919efc298573b6239cacf39239
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-iis-vm-with-powershell"></a><span data-ttu-id="fc90c-103">Az IIS virtuális gép létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="fc90c-103">Create an IIS VM with PowerShell</span></span>

<span data-ttu-id="fc90c-104">Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket, és az Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény használatával telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="fc90c-104">This script creates an Azure Virtual Machine running Windows Server 2016, and then uses the Azure Virtual Machine Custom Script Extension to install IIS.</span></span> <span data-ttu-id="fc90c-105">A parancsfájl futtatása után érheti el az alapértelmezett IIS webhelyet a nyilvános IP-címhez, a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fc90c-105">After running the script, you can access the default IIS website on the public IP address of the virtual machine.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="fc90c-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="fc90c-106">Sample script</span></span>

<span data-ttu-id="fc90c-107">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "VM IIS létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="fc90c-107">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="fc90c-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="fc90c-108">Clean up deployment</span></span> 

<span data-ttu-id="fc90c-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="fc90c-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="fc90c-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="fc90c-110">Script explanation</span></span>

<span data-ttu-id="fc90c-111">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fc90c-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="fc90c-112">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="fc90c-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="fc90c-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="fc90c-113">Command</span></span> | <span data-ttu-id="fc90c-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="fc90c-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="fc90c-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fc90c-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="fc90c-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fc90c-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="fc90c-117">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="fc90c-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="fc90c-118">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="fc90c-118">Creates a subnet configuration.</span></span> <span data-ttu-id="fc90c-119">Ezt a konfigurációt használja a virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="fc90c-119">This configuration is used with the virtual network creation process.</span></span> |
| [<span data-ttu-id="fc90c-120">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="fc90c-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="fc90c-121">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fc90c-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="fc90c-122">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="fc90c-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="fc90c-123">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="fc90c-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="fc90c-124">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="fc90c-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="fc90c-125">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="fc90c-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="fc90c-126">Ez a konfiguráció segítségével egy NSG-szabály létrehozása az NSG létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="fc90c-126">This configuration is used to create an NSG rule when the NSG is created.</span></span> |
| [<span data-ttu-id="fc90c-127">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="fc90c-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="fc90c-128">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fc90c-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="fc90c-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="fc90c-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="fc90c-130">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="fc90c-130">Gets subnet information.</span></span> <span data-ttu-id="fc90c-131">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="fc90c-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="fc90c-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="fc90c-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="fc90c-133">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="fc90c-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="fc90c-134">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="fc90c-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="fc90c-135">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fc90c-135">Creates a VM configuration.</span></span> <span data-ttu-id="fc90c-136">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="fc90c-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="fc90c-137">A konfiguráció a Virtuálisgép-létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="fc90c-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="fc90c-138">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="fc90c-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="fc90c-139">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fc90c-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="fc90c-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="fc90c-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="fc90c-141">A virtuális gépet egy Virtuálisgép-bővítmény hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="fc90c-141">Add a VM extension to the virtual machine.</span></span> <span data-ttu-id="fc90c-142">Ez a példa az egyéni parancsprogramok futtatására szolgáló bővítmény segítségével telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="fc90c-142">In this sample, the custom script extension is used to install IIS.</span></span> |
|[<span data-ttu-id="fc90c-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="fc90c-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="fc90c-144">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="fc90c-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="fc90c-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fc90c-145">Next steps</span></span>

<span data-ttu-id="fc90c-146">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fc90c-146">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="fc90c-147">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fc90c-147">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

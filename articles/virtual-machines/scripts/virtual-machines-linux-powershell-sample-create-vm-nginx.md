---
title: "PowerShell parancsfájl minta - aaaAzure NGINX |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl a minta - NGINX"
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
ms.openlocfilehash: 017a4853e400f8f544a3f92771adaf468b5dee76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-nginx-vm-with-powershell"></a><span data-ttu-id="d89ee-103">Egy NGINX virtuális gép létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d89ee-103">Create an NGINX VM with PowerShell</span></span>

<span data-ttu-id="d89ee-104">Ez a parancsfájl létrehoz egy Azure virtuális gépet, és ezután hello Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="d89ee-104">This script creates an Azure Virtual Machine and then uses hello Azure Virtual Machine Custom Script Extension tooinstall NGINX.</span></span> <span data-ttu-id="d89ee-105">Hello parancsprogram futtatása után a nyilvános IP-címe hello hello virtuális gép egy bemutató webhelyen érheti el.</span><span class="sxs-lookup"><span data-stu-id="d89ee-105">After running hello script, you can access a demo website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d89ee-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d89ee-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.ps1 "Create VM NGINX")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d89ee-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d89ee-107">Clean up deployment</span></span> 

<span data-ttu-id="d89ee-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="d89ee-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d89ee-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d89ee-109">Script explanation</span></span>

<span data-ttu-id="d89ee-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="d89ee-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="d89ee-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="d89ee-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="d89ee-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="d89ee-112">Command</span></span> | <span data-ttu-id="d89ee-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d89ee-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d89ee-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d89ee-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d89ee-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d89ee-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d89ee-116">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="d89ee-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="d89ee-117">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="d89ee-117">Creates a subnet configuration.</span></span> <span data-ttu-id="d89ee-118">Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="d89ee-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="d89ee-119">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="d89ee-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="d89ee-120">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d89ee-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="d89ee-121">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="d89ee-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="d89ee-122">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d89ee-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="d89ee-123">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="d89ee-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="d89ee-124">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="d89ee-124">Creates a network security group rule configuration.</span></span> <span data-ttu-id="d89ee-125">Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="d89ee-125">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="d89ee-126">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="d89ee-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="d89ee-127">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d89ee-127">Creates a network security group.</span></span> |
| [<span data-ttu-id="d89ee-128">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="d89ee-128">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="d89ee-129">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="d89ee-129">Gets subnet information.</span></span> <span data-ttu-id="d89ee-130">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="d89ee-130">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="d89ee-131">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="d89ee-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="d89ee-132">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="d89ee-132">Creates a network interface.</span></span> |
| [<span data-ttu-id="d89ee-133">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="d89ee-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="d89ee-134">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d89ee-134">Creates a VM configuration.</span></span> <span data-ttu-id="d89ee-135">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="d89ee-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="d89ee-136">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="d89ee-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="d89ee-137">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="d89ee-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="d89ee-138">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d89ee-138">Create a virtual machine.</span></span> |
| [<span data-ttu-id="d89ee-139">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="d89ee-139">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="d89ee-140">Adja hozzá a virtuális gép bővítmény toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d89ee-140">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="d89ee-141">Ez a példa hello egyéni parancsprogramok futtatására szolgáló bővítmény használt tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="d89ee-141">In this sample, hello custom script extension is used tooinstall NGINX.</span></span> |
|[<span data-ttu-id="d89ee-142">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d89ee-142">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="d89ee-143">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d89ee-143">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d89ee-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d89ee-144">Next steps</span></span>

<span data-ttu-id="d89ee-145">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d89ee-145">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d89ee-146">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Linux virtuális dokumentációját](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d89ee-146">Additional virtual machine PowerShell script samples can be found in hello [Azure Linux VM documentation](../linux/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

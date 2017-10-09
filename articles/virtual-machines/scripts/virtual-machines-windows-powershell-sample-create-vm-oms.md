---
title: "PowerShell parancsfájl minta - aaaAzure OMS |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl a minta - OMS"
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
ms.date: 03/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1eeafbe743013e97bf3fcefb5ce87f72cb503a4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-operations-management-suite-monitored-vm-with-powershell"></a><span data-ttu-id="289f9-103">Hozzon létre egy Operations Management Suite figyelt VM a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="289f9-103">Create an Operations Management Suite monitored VM with PowerShell</span></span>

<span data-ttu-id="289f9-104">Ezt a parancsfájlt hoz létre egy Azure virtuális gép, hello Operations Management Suite (OMS) agent szolgáltatást telepíti, és regisztrálja az OMS-munkaterület hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="289f9-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="289f9-105">Miután hello parancsfájl lefutott, hello virtuális gép hello OMS konzolban látható lesz.</span><span class="sxs-lookup"><span data-stu-id="289f9-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span> <span data-ttu-id="289f9-106">Emellett kell tooupdate hello OMS munkaterület azonosítója és a munkaterület kulcsot.</span><span class="sxs-lookup"><span data-stu-id="289f9-106">Also, you need tooupdate hello OMS workspace ID and workspace key.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="289f9-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="289f9-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-detailed-oms.ps1 "Create VM OMS")]

## <a name="clean-up-deployment"></a><span data-ttu-id="289f9-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="289f9-108">Clean up deployment</span></span> 

<span data-ttu-id="289f9-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="289f9-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="289f9-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="289f9-110">Script explanation</span></span>

<span data-ttu-id="289f9-111">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="289f9-111">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="289f9-112">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="289f9-112">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="289f9-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="289f9-113">Command</span></span> | <span data-ttu-id="289f9-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="289f9-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="289f9-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="289f9-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="289f9-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="289f9-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="289f9-117">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="289f9-117">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="289f9-118">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="289f9-118">Creates a subnet configuration.</span></span> <span data-ttu-id="289f9-119">Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="289f9-119">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="289f9-120">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="289f9-120">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="289f9-121">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="289f9-121">Creates a virtual network.</span></span> |
| [<span data-ttu-id="289f9-122">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="289f9-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="289f9-123">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="289f9-123">Creates a public IP address.</span></span> |
| [<span data-ttu-id="289f9-124">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="289f9-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="289f9-125">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="289f9-125">Creates a network security group rule configuration.</span></span> <span data-ttu-id="289f9-126">Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="289f9-126">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="289f9-127">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="289f9-127">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="289f9-128">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="289f9-128">Creates a network security group.</span></span> |
| [<span data-ttu-id="289f9-129">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="289f9-129">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="289f9-130">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="289f9-130">Gets subnet information.</span></span> <span data-ttu-id="289f9-131">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="289f9-131">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="289f9-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="289f9-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="289f9-133">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="289f9-133">Creates a network interface.</span></span> |
| [<span data-ttu-id="289f9-134">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="289f9-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="289f9-135">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="289f9-135">Creates a VM configuration.</span></span> <span data-ttu-id="289f9-136">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="289f9-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="289f9-137">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="289f9-137">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="289f9-138">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="289f9-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="289f9-139">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="289f9-139">Create a virtual machine.</span></span> |
| [<span data-ttu-id="289f9-140">Set-AzureRmVMExtension</span><span class="sxs-lookup"><span data-stu-id="289f9-140">Set-AzureRmVMExtension</span></span>](/powershell/module/azurerm.compute/set-azurermvmextension) | <span data-ttu-id="289f9-141">Adja hozzá a virtuális gép bővítmény toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="289f9-141">Add a VM extension toohello virtual machine.</span></span> <span data-ttu-id="289f9-142">Hello Operations Management Suite ügynök bővítmény ebben az esetben használt tooinstall hello OMS-ügynököt, és az OMS-munkaterület VM hello regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="289f9-142">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
|[<span data-ttu-id="289f9-143">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="289f9-143">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="289f9-144">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="289f9-144">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="289f9-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="289f9-145">Next steps</span></span>

<span data-ttu-id="289f9-146">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="289f9-146">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="289f9-147">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="289f9-147">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

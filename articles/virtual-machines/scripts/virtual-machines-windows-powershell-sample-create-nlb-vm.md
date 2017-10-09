---
title: "PowerShell parancsfájl minta - aaaAzure hozzon létre egy Windows virtuális gép hálózati Terheléselosztás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – hozzon létre egy Windows virtuális gép hálózati Terheléselosztás"
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
ms.date: 06/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 60a48c5f243c8ff3ab886437ae45b21ce069ea4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a><span data-ttu-id="9fdb4-103">Betöltési oszthatja el a forgalmat magas rendelkezésre állású virtuális gépek között</span><span class="sxs-lookup"><span data-stu-id="9fdb4-103">Load balance traffic between highly available virtual machines</span></span>

<span data-ttu-id="9fdb4-104">Ez a minta parancsfájlt hoz létre minden szükséges toorun több Windows Server 2016-os virtuális gépek magas rendelkezésre állású konfigurálva és betölteni a konfigurációs elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-104">This script sample creates everything needed toorun several Windows Server 2016 virtual machines configured in a highly available and load balanced configuration.</span></span> <span data-ttu-id="9fdb4-105">Három virtuális gépet, a csatlakoztatott tooan Azure rendelkezésre állási csoportban, hogy után hello a parancsfájl futtatását, és az Azure Load Balancer keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-105">After running hello script, you will have three virtual machines, joined tooan Azure Availability Set, and accessible through an Azure Load Balancer.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9fdb4-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9fdb4-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a><span data-ttu-id="9fdb4-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9fdb4-107">Clean up deployment</span></span> 

<span data-ttu-id="9fdb4-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9fdb4-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9fdb4-109">Script explanation</span></span>

<span data-ttu-id="9fdb4-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="9fdb4-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9fdb4-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="9fdb4-112">Command</span></span> | <span data-ttu-id="9fdb4-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9fdb4-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9fdb4-114">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9fdb4-114">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="9fdb4-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="9fdb4-116">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-116">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="9fdb4-117">Létrehoz egy alhálózati konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-117">Creates a subnet configuration.</span></span> <span data-ttu-id="9fdb4-118">Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-118">This configuration is used with hello virtual network creation process.</span></span> |
| [<span data-ttu-id="9fdb4-119">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="9fdb4-119">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="9fdb4-120">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-120">Creates a virtual network.</span></span> |
| [<span data-ttu-id="9fdb4-121">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="9fdb4-121">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="9fdb4-122">Létrehoz egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-122">Creates a public IP address.</span></span> |
| [<span data-ttu-id="9fdb4-123">Új AzureRmLoadBalancerFrontendIpConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-123">New-AzureRmLoadBalancerFrontendIpConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | <span data-ttu-id="9fdb4-124">Létrehoz egy terheléselosztó előtér-IP-konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-124">Creates a front-end IP configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-125">Új AzureRmLoadBalancerBackendAddressPoolConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-125">New-AzureRmLoadBalancerBackendAddressPoolConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | <span data-ttu-id="9fdb4-126">Létrehoz egy háttér címkészlet terheléselosztó beállítása.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-126">Creates a backend address pool configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-127">Új AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-127">New-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | <span data-ttu-id="9fdb4-128">Létrehoz egy terheléselosztó mintavételi konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-128">Creates a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-129">Új AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-129">New-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | <span data-ttu-id="9fdb4-130">Létrehoz egy terheléselosztó szabály konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-130">Creates a rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-131">Új AzureRmLoadBalancerInboundNatRuleConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-131">New-AzureRmLoadBalancerInboundNatRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | <span data-ttu-id="9fdb4-132">Egy bejövő NAT-szabály konfigurációjának terheléselosztót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-132">Creates an inbound NAT rule configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-133">Új AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="9fdb4-133">New-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/new-azurermloadbalancer) | <span data-ttu-id="9fdb4-134">Létrehoz egy terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-134">Creates a load balancer.</span></span> |
| [<span data-ttu-id="9fdb4-135">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-135">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="9fdb4-136">Létrehoz egy hálózati biztonsági csoport szabály konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-136">Creates a network security group rule configuration.</span></span> <span data-ttu-id="9fdb4-137">Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-137">This configuration is used toocreate an NSG rule when hello NSG is created.</span></span> |
| [<span data-ttu-id="9fdb4-138">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="9fdb4-138">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="9fdb4-139">Hálózati biztonsági csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-139">Creates a network security group.</span></span> |
| [<span data-ttu-id="9fdb4-140">Get-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-140">Get-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="9fdb4-141">Lekérdezi az alhálózati adatokat.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-141">Gets subnet information.</span></span> <span data-ttu-id="9fdb4-142">Ezt az információt a hálózati illesztő létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-142">This information is used when creating a network interface.</span></span> |
| [<span data-ttu-id="9fdb4-143">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="9fdb4-143">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="9fdb4-144">Létrehoz egy adott hálózati csatoló.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-144">Creates a network interface.</span></span> |
| [<span data-ttu-id="9fdb4-145">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="9fdb4-145">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="9fdb4-146">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-146">Creates a VM configuration.</span></span> <span data-ttu-id="9fdb4-147">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-147">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="9fdb4-148">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-148">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="9fdb4-149">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="9fdb4-149">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="9fdb4-150">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-150">Create a virtual machine.</span></span> |
|[<span data-ttu-id="9fdb4-151">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="9fdb4-151">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="9fdb4-152">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9fdb4-152">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9fdb4-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9fdb4-153">Next steps</span></span>

<span data-ttu-id="9fdb4-154">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9fdb4-154">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9fdb4-155">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9fdb4-155">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

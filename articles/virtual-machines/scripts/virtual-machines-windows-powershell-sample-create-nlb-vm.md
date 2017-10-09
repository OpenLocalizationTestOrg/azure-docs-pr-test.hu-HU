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
# <a name="load-balance-traffic-between-highly-available-virtual-machines"></a>Betöltési oszthatja el a forgalmat magas rendelkezésre állású virtuális gépek között

Ez a minta parancsfájlt hoz létre minden szükséges toorun több Windows Server 2016-os virtuális gépek magas rendelkezésre állású konfigurálva és betölteni a konfigurációs elosztott terhelésű. Három virtuális gépet, a csatlakoztatott tooan Azure rendelkezésre állási csoportban, hogy után hello a parancsfájl futtatását, és az Azure Load Balancer keresztül érhető el.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Create VM NLB")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello telepítési hello. Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Létrehoz egy alhálózati konfigurációt. Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Virtuális hálózat létrehozása. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Létrehoz egy nyilvános IP-címet. |
| [Új AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Létrehoz egy terheléselosztó előtér-IP-konfigurációja. |
| [Új AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Létrehoz egy háttér címkészlet terheléselosztó beállítása. |
| [Új AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Létrehoz egy terheléselosztó mintavételi konfigurációt. |
| [Új AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Létrehoz egy terheléselosztó szabály konfigurációját. |
| [Új AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Egy bejövő NAT-szabály konfigurációjának terheléselosztót hoz létre. |
| [Új AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Létrehoz egy terheléselosztót. |
| [Új AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Létrehoz egy hálózati biztonsági csoport szabály konfigurációt. Ez a konfiguráció használt toocreate egy NSG-szabály esetén hello NSG jön létre. |
| [Új AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hálózati biztonsági csoportot hoz létre. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Lekérdezi az alhálózati adatokat. Ezt az információt a hálózati illesztő létrehozása során használatos. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehoz egy adott hálózati csatoló. |
| [Új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Létrehoz egy Virtuálisgép-konfiguráció. Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal. hello konfigurációs szolgál a virtuális gép létrehozása során. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hozzon létre egy virtuális gépet. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Eltávolítja az erőforráscsoportot és belül található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

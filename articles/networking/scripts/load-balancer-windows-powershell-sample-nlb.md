---
title: "PowerShell parancsfájl minta - Load balance forgalom tooVMs a magas rendelkezésre állású aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Load balance forgalom tooVMs a magas rendelkezésre állású"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 332c39e1aad071ca6f7ce420ccc1f423ef647d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-traffic-toovms-for-high-availability"></a>Egyenleg forgalom tooVMs a magas rendelkezésre állású betöltése

Ez a minta parancsfájlt hoz létre minden szükséges toorun több Windows virtuális gépek magas rendelkezésre állású konfigurálva és betölteni a konfigurációs elosztott terhelésű. Három virtuális gépet, a csatlakoztatott tooan Azure rendelkezésre állási csoportban, hogy után hello a parancsfájl futtatását, és az Azure Load Balancer keresztül érhető el.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Létrehoz egy alhálózati konfigurációt. Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel. |
| [Új AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Egy Azure terheléselosztót hoz létre. |
| [Új AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Terheléselosztói mintavétel létrehozása. Terheléselosztói mintavétel használt toomonitor hello load balancer készlet minden egyes virtuális gép van. Ha minden virtuális gép elérhetetlenné válik, akkor kimenő forgalomról nem útválasztásos toohello VM. |
| [Új AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Létrehoz egy terheléselosztó szabályhoz. Ez a példa létrejön egy szabály 80-as porton. HTTP-forgalom érkezik hello terheléselosztót, mert már irányított tooport 80 hello load balancer set hello virtuális gépeinek egyikét. |
| [Új AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Létrehoz egy terheléselosztó hálózati címfordítás (NAT) szabály.  NAT-szabályok hello load balancer tooa port a virtuális gép port hozzárendelését. Ez a példa egy NAT-szabály jön létre SSH forgalom tooeach VM hello load balancer készletében.  |
| [Új AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hálózati biztonsági csoport (NSG), amely a biztonsági határ hello internet és hello virtuális gépek közötti hoz létre. |
| [Új AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Létrehoz egy NSG-szabály tooallow érkező bejövő forgalmat. Ez a példa 22-es portot SSH forgalom van megnyitva. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati alhálózat és NSG. |
| [Új AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Létrehoz egy rendelkezésre állási csoportot. Rendelkezésre állási készletek elosztásával hello virtuális gépek a fizikai erőforrások között, úgy, hogy hiba esetén nem történik teljes készletét megtisztítja hello alkalmazás hasznos üzemidő biztosítása. |
| [Új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Létrehoz egy Virtuálisgép-konfiguráció. Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal. hello konfigurációs szolgál a virtuális gép létrehozása során. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).

További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

---
title: "PowerShell parancsfájl minta - aaaAzure terhelésének elosztása több webhely, az Azure PowerShell |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - terhelésének elosztása több webhelyek toohello ugyanahhoz a virtuális géphez"
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
ms.openlocfilehash: 316818964eb6928fe4163ef69eb7f05da2dc9636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-multiple-websites"></a>Terhelésének elosztása több webhely

Parancsfájl a következő példában két virtuális gépekkel (VM), amelyek egy rendelkezésre állási csoport tagjai hoz létre egy virtuális hálózatot. A terheléselosztó irányítja a forgalmat a két külön IP-címek toohello két virtuális gépeket. Hello a parancsfájl futtatását, miután sikerült web server szoftver toohello virtuális gépek telepítése, és mindegyiket a saját IP-cím több webhely üzemeltetésére.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, virtuális hálózati terheléselosztó, és minden kapcsolódó erőforrások betöltése hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Létrehoz egy Azure rendelkezésre állási készlet tooprovide magas rendelkezésre állású. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Létrehoz egy alhálózati konfigurációt. Ebben a konfigurációban használatos hello virtuális hálózat létrehozásának folyamatát. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Virtuális hálózat létrehozása. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Létrehoz egy nyilvános IP-címet. |
| [Új AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Létrehoz egy terheléselosztó előtér-IP config. |
| [Új AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Létrehoz egy háttér címkészlet terheléselosztó beállítása. |
| [Új AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Létrehoz egy hálózati Terheléselosztási mintavételt. Az NLB mintavételi használt toomonitor az egyes virtuális gépek hello hálózati Terheléselosztási készletben. Ha minden virtuális gép elérhetetlenné válik, akkor kimenő forgalomról nem útválasztásos toohello VM. |
| [Új AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Egy hálózati Terheléselosztási szabályt hoz létre. Ez a példa létrejön egy szabály 80-as porton. HTTP-forgalom érkezik hello NLB, mivel irányított tooport 80 egy hello virtuális gépek hello hálózati Terheléselosztási készletben. |
| [Új AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Létrehoz egy terheléselosztót. |
| [Új AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Határozza meg a virtuális hálózati adapter speciális szolgáltatásai. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehoz egy adott hálózati csatoló. |
| [Új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Létrehoz egy Virtuálisgép-konfiguráció. Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal. hello konfigurációs szolgál a virtuális gép létrehozása során. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hozzon létre egy virtuális gépet. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Eltávolítja az erőforráscsoportot és belül található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).

További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

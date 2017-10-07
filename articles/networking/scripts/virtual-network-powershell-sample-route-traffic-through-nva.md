---
title: "aaaAzure PowerShell parancsfájl minta - útvonal forgalmát a hálózati virtuális készülék |} Microsoft Docs"
description: "Az Azure PowerShell parancsfájl minta - hálózat virtuális készülékként egy tűzfalat-útvonal forgalmát."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 3b999f3289d654c00d5becb973e2883896457d52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>A hálózati virtuális készülék-útvonal forgalmát

Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat. Is létrehoz egy virtuális gép IP-enabled tooroute forgalmat hello két alhálózat között. Hálózati szoftverek, például egy tűzfal alkalmazást, a virtuális gép toohello telepítése hello parancsfájl futtatása után.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl


[!code-powershell[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```
## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok. Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Háttér- és DMZ-alhálózatot hoz létre. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | A nyilvános IP cím tooaccess hello VM hello Internet jön létre. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehoz egy virtuális hálózati adapter és az IP-továbbítás engedélyezése. |
| [Új AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | A hálózati biztonsági csoport (NSG) hoz. |
| [Új AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | NSG-szabályok, amelyek lehetővé teszik a HTTP és HTTPS-portok toohello VM bejövő hoz létre. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Társult hello NSG-ket és útválasztási táblázatot toosubnets. |
| [Új AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Az összes útvonal egy útválasztási táblázatot hoz létre. |
| [Új AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Útvonalak tooroute forgalmi alhálózatok között, valamint hello interneten keresztül hello VM hoz létre. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Létrehoz egy virtuális gépet, és csatolja a hello NIC tooit. Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Törli az erőforráscsoportot és a benne található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).

További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

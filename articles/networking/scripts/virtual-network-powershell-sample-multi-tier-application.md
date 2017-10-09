---
title: "PowerShell parancsfájl minta - aaaAzure hálózat Többrétegű alkalmazások létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Többrétegű alkalmazások virtuális hálózat létrehozása."
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
ms.openlocfilehash: 46d6d16dc5dbc8be467359f31346f017727b1abe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>A többrétegű alkalmazások hálózat létrehozása

Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat. Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306. Után hello a parancsfájl futtatását hogy két virtuális gép, egy-egy mindegyik alhálózat, amely központilag telepíthető a webkiszolgáló és szoftverrel a MySQL.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl


[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok. Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Háttér-alhálózatot hoz létre. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | A nyilvános IP cím tooaccess hello VM hello Internet jön létre. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok. |
| [Új AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre. |
| [Új AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach. Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Törli az erőforráscsoportot és a benne található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).

További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).

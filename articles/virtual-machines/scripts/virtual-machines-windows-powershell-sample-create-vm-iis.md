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
# <a name="create-an-iis-vm-with-powershell"></a>Az IIS virtuális gép létrehozása a PowerShell használatával

Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket, és az Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény használatával telepítse az IIS. A parancsfájl futtatása után érheti el az alapértelmezett IIS webhelyet a nyilvános IP-címhez, a virtuális gép.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "VM IIS létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat a központi telepítés létrehozásához. A parancs adott dokumentáció tábla mutató összes elemére.

| Parancs | Megjegyzések |
|---|---|
| [Új-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [Új AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Létrehoz egy alhálózati konfigurációt. Ezt a konfigurációt használja a virtuális hálózat létrehozásának folyamatát. |
| [Új-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Virtuális hálózat létrehozása. |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Létrehoz egy nyilvános IP-címet. |
| [Új AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Létrehoz egy hálózati biztonsági csoport szabály konfigurációt. Ez a konfiguráció segítségével egy NSG-szabály létrehozása az NSG létrehozásakor. |
| [Új AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Hálózati biztonsági csoportot hoz létre. |
| [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) | Lekérdezi az alhálózati adatokat. Ezt az információt a hálózati illesztő létrehozása során használatos. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehoz egy adott hálózati csatoló. |
| [Új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Létrehoz egy Virtuálisgép-konfiguráció. Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal. A konfiguráció a Virtuálisgép-létrehozása során használatos. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Hozzon létre egy virtuális gépet. |
| [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) | A virtuális gépet egy Virtuálisgép-bővítmény hozzáadása. Ez a példa az egyéni parancsprogramok futtatására szolgáló bővítmény segítségével telepítse az IIS. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Eltávolítja az erőforráscsoportot és belül található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

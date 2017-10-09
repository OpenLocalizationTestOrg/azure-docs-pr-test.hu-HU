---
title: "PowerShell parancsfájl minta - aaaAzure virtuális gép létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – hozzon létre egy virtuális Gépet egy pillanatképből"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 89c65171b55bff0582c4a26df0b0f29f556845fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell"></a>Hozzon létre egy virtuális gépet egy pillanatképből, a PowerShell használatával

Ez a parancsfájl létrehoz egy virtuális gépet egy operációsrendszer-lemez egy pillanatképből. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok tooget pillanatkép tulajdonságok, hozzon létre egy felügyelt lemezes pillanatképből, és hozzon létre egy virtuális Gépet hello. Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.

| Parancs | Megjegyzések |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/get-azurermsnapshot) | Lekérdezi a pillanatkép-név használatával pillanatképet. |
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) | Létrehozza a lemezkonfigurációt. Ebben a konfigurációban használatos hello lemez létrehozási folyamata. |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk) | Egy felügyelt lemezt hoz létre. |
| [Új AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Létrehoz egy Virtuálisgép-konfiguráció. Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal. hello konfigurációs szolgál a virtuális gép létrehozása során. |
| [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) | Hello felügyelt lemezes csatolja az operációs rendszer lemez toohello virtuális gépként |
| [Új AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Létrehoz egy nyilvános IP-címet. |
| [Új AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Létrehoz egy adott hálózati csatoló. |
| [Új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Létrehoz egy virtuális gépet. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Eltávolítja az erőforráscsoportot és belül található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

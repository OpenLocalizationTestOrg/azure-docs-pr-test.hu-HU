---
title: "PowerShell parancsfájl minta - aaaAzure kezelt lemez létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - pillanatképből kezelt lemez létrehozása"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: b99413b4c3e9a662a5fef74b7e4aeb5ecbb26af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Hozzon létre egy felügyelt lemezes egy pillanatképből, a PowerShell használatával

Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre. A virtuális gép operációsrendszer- és lemezek pillanatkép-készítési toorestore használni. Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása. Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate felügyelt lemezes egy pillanatképből. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Pillanatkép tulajdonságainak lekérdezi.  |
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | A lemez létrehozásához használt lemezkonfiguráció hoz létre. Ez magában foglalja a hello erőforrás-azonosítót hello szülő pillanatkép, a helyre, amely ugyanaz, mint a szülő pillanatkép és hello tárolótípus hello helyét.  |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre. |


## <a name="next-steps"></a>Következő lépések

[A felügyelt lemezes virtuális gép létrehozása](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

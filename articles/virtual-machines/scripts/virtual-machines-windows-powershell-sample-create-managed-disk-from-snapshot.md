---
title: "Az Azure PowerShell-parancsfájl minták – hozzon létre egy felügyelt lemezes egy pillanatképből |} Microsoft Docs"
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
ms.openlocfilehash: b475516694d120b7ea05d0892b6789710eec171e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Hozzon létre egy felügyelt lemezes egy pillanatképből, a PowerShell használatával

Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre. Segítségével szeretné visszaállítani a virtuális gép operációsrendszer- és lemezek pillanatkép-készítési. Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása. Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "létrehozás felügyelt lemezes pillanatképből")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy pillanatképből kezelt lemez létrehozása. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Pillanatkép tulajdonságainak lekérdezi.  |
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | A lemez létrehozásához használt lemezkonfiguráció hoz létre. Ez magában foglalja az erőforrás-azonosítót a szülő pillanatkép, a helyre, amely ugyanaz, mint a szülő pillanatkép és a tárolási helyét.  |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre. |


## <a name="next-steps"></a>Következő lépések

[A felügyelt lemezes virtuális gép létrehozása](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
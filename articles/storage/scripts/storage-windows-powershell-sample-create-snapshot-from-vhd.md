---
title: "Az Azure PowerShell-parancsfájl minta - pillanatképet készíteni virtuális Merevlemezét kis időn belül több azonos felügyelt lemezek létrehozásához |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - pillanatkép létrehozása a virtuális merevlemez létrehozása több, azonos felügyelt lemezek kis időn belül"
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
ms.openlocfilehash: 8588a33a1f0b4cce0059a40239301587cdc28920
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Pillanatkép létrehozása olyan virtuális merevlemezről több azonos kezelt lemez létrehozása a PowerShell-lel kis időn belül

Ez a parancsfájl pillanatképet hoz létre a VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban. Ezen parancsfájl segítségével importálja a speciális (nem általánosított/Sysprep használatával előkészített) VHD-fájlt egy pillanatképet, majd használja a pillanatkép létrehozásához több azonos által kezelt lemezeken kis időn belül. Használja továbbá azt is adatimportálás egy virtuális Merevlemezt egy pillanatképre, majd a pillanatkép létrehozása több felügyelt lemezek kis időn belül. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "létrehozás pillanatkép virtuális merevlemezből")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hozhat létre egy felügyelt lemezt egy másik előfizetésben található virtuális merevlemez. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | A lemez létrehozásához használt lemezkonfiguráció hoz létre. Ez magában foglalja a tárolási típus, hely, erőforrás-azonosítót a szülő virtuális Merevlemezt tároló tárfióknak és a szülő virtuális Merevlemezt a virtuális merevlemez URI. |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre. |

## <a name="next-steps"></a>Következő lépések

[Hozzon létre egy felügyelt lemezes pillanatképből](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
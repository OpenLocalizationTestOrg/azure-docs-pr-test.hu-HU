---
title: "PowerShell parancsfájl minta - aaaAzure pillanatkép létrehozása egy virtuális merevlemez toocreate a több azonos felügyelt lemezeket kis időn belül |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – létrehozása pillanatképet egy virtuális merevlemez toocreate több azonos felügyelt lemezek kis időn belül"
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
ms.openlocfilehash: 0a13e399b692f32b3772add39fe5b5c023808c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-snapshot-from-a-vhd-toocreate-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Pillanatkép létrehozása a VHD-toocreate több azonos felügyelt lemezeket a PowerShell-lel kis időn belül

Ez a parancsfájl pillanatképet hoz létre a VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban. Használja a parancsfájl tooimport speciális (nem általánosított/Sysprep használatával előkészített) VHD tooa pillanatképet, majd hello pillanatkép toocreate több azonos felügyelt lemezek kis időn belül. Használja továbbá azt tooimport adatok VHD tooa pillanatképet, majd hello pillanatkép toocreate több felügyelt lemezek kis időn belül. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate felügyelt lemezes egy másik előfizetésben található virtuális merevlemezről. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | A lemez létrehozásához használt lemezkonfiguráció hoz létre. Ez magában foglalja a tárolási típus, hely, erőforrás-azonosítót hello tárfiók hello szülő virtuális merevlemez tárolásához és hello szülő virtuális Merevlemezt a virtuális merevlemez URI. |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre. |

## <a name="next-steps"></a>Következő lépések

[Hozzon létre egy felügyelt lemezes pillanatképből](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

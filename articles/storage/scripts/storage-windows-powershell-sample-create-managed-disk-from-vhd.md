---
title: "PowerShell parancsfájl minta - aaaAzure hozhat létre felügyelt lemezt egy VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt olyan tárfiókban ugyanazon vagy másik előfizetésben található"
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
ms.openlocfilehash: 93157823eb3b8cddba5e0af455d16bff1d42ce00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a>Felügyelt lemezes egy tárfiókot, a PowerShell segítségével az ugyanazon vagy másik előfizetést a VHD-fájl létrehozása

Ezt a parancsfájlt egy felügyelt lemezt egy VHD-fájlt olyan tárfiókban ugyanazon vagy másik előfizetést hoz létre. A parancsfájl tooimport egy speciális (nem általánosított/Sysprep használatával előkészített) VHD toomanaged OS lemez toocreate egy virtuális gép használja. Használja továbbá azt tooimport VHD toomanaged adatlemezt. 

Ne hozzon létre több azonos felügyelt lemezeket a VHD-fájl kis időn belül. a vhd-fájl toocreate által kezelt lemezeken, blob pillanatképe hello vhd-fájl jön létre és majd felügyelt használt toocreate lemezek. Csak egy blob pillanatkép is létrehozható egy létrehozása lemezhiba okozó percen belül esedékes toothrottling. tooavoid a szabályozás, hozzon létre egy [hello vhd-fájlt a felügyelt pillanatkép](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) és majd használata hello kezelt pillanatkép toocreate több felügyelt lemezre rövid időn belül. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate felügyelt lemezes egy másik előfizetésben található virtuális merevlemezről. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | A lemez létrehozásához használt lemezkonfiguráció hoz létre. Ez magában foglalja a tárolási típus, hely, erőforrás hello tárfiók hello szülő virtuális merevlemez tárolásához, hello szülő virtuális Merevlemezt a virtuális merevlemez URI azonosítója. |
| [Új AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre. |

## <a name="next-steps"></a>Következő lépések

[Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

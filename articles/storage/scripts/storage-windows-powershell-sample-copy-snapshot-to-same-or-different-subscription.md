---
title: "PowerShell parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy egy másik előfizetésben található aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy egy másik előfizetésben található"
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
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 7a3565356f13cb93759dec7ef9d0357e04c3410b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Másolja át a felügyelt lemezes pillanatképet ugyanahhoz az előfizetéshez vagy másik előfizetést, a PowerShell használatával

Ez a parancsfájl egy pillanatkép másolatot készít a hello ugyanahhoz az előfizetéshez ugyanazon vagy másik előfizetést. A parancsfájl toomove pillanatkép toodifferent előfizetés használja az adatok megőrzésének. Tárolni pillanatképek másik előfizetésben található a pillanatképek a fő előfizetésben véletlen törlés elleni védelme. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával pillanatkép hello hello forrás pillanatkép azonosítója. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | A pillanatkép létrehozásához használt pillanatkép-konfiguráció létrehozása Ez magában foglalja a hello erőforrás-azonosítót hello szülő pillanatkép és helyre, amely ugyanaz, mint a hello szülő pillanatkép.  |
| [Új AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Pillanatkép készítése a pillanatkép-beállítása, pillanatfelvétel neve és paraméterként erőforráscsoport-név használatával. |


## <a name="next-steps"></a>Következő lépések

[Hozzon létre egy virtuális gépet egy pillanatképből](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

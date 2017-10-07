---
title: "PowerShell parancsfájl minta - exportálási vagy másolási pillanatkép, a virtuális merevlemez tooa tárfiók más régióban aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - exportálási vagy másolási pillanatkép VHD tooa tárfiókkal azonos másik régióban található"
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
ms.openlocfilehash: 3b3e38c6b06bfa1e117f4e913dfc09443a795196
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a>Exportálás vagy másolási felügyelt pillanatfelvételek VHD tooa tárfiókkal másik régióban található a PowerShell használatával

Ez a parancsfájl egy felügyelt pillanatkép tooa tárfiók más régióban exportálja. Először hoz létre a hello hello pillanatkép SAS URI-t, és a toocopy használja azt tooa tárfiók más régióban. A parancsfájl toomaintain biztonsági másolat, a kezelt lemezek használható különböző régióban vész-helyreállítási.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toogenerate felügyelt pillanatkép és másolat hello SAS URI-JÁNAK pillanatkép-tooa tárfiók SAS URI-JÁNAK használatával. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Támogatás-AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | SAS URI-t hoz létre egy pillanatképet, amely használt toocopy azt tooa tárfiók. |
| [Új AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Létrehoz egy tárfiók környezetét hello fióknevet és kulcsot használ. Ebben a környezetben használt tooperform olvasási/írási műveletek hello storage-fiók is lehet. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Másolja a pillanatkép tooa tárfiók alapul szolgáló virtuális merevlemez hello |

## <a name="next-steps"></a>Következő lépések

[Felügyelt lemezes virtuális merevlemez létrehozása](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[A felügyelt lemezes virtuális gép létrehozása](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

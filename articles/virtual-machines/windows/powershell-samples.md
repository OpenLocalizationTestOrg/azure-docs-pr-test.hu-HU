---
title: "Virtuális gép PowerShell-példák aaaAzure |} Microsoft Docs"
description: "Virtuális gép az Azure PowerShell-példák"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/05/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 89fd26aa979687cdcdf9ae4e60be7d0d2eaeae91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure virtuális gép PowerShell-minták

hello következő táblázat tartalmazza a hivatkozások tooPowerShell parancsfájlok mintákat, amelyek Windows virtuális gépek létrehozására és kezelésére.

| | |
|---|---|
|**Virtuális gépek létrehozása**||
| [A teljesen konfigurált virtuális gép létrehozása](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.|
| [Magas rendelkezésre állású virtuális gépek létrehozása](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre.|
| [Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet, és hello Azure Custom Script bővítmény tooinstall IIS használ. |
| [Hozzon létre egy virtuális Gépet, és futtassa a DSC-konfiguráció](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet, és hello Azure kívánt állapot konfigurációs szolgáltatása (DSC) bővítmény tooinstall IIS használ. |
| [A virtuális merevlemez feltöltéséhez és a virtuális gépek létrehozása](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Uplaods helyi virtuális merevlemez tooAzure fájlt, létrehoz és kép hello VHD és erről a képről létrehoz egy virtuális Gépet. |
| [A felügyelt operációsrendszer-lemez a virtuális gép létrehozása](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy meglévő kezelt lemez az operációs rendszer lemezeként csatolásával. |
| [Hozzon létre egy virtuális Gépet egy pillanatképből](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy pillanatképből, először hoz létre egy felügyelt lemezes pillanatképből, és majd lemezcsatlakoztatás hello új felügyelt az operációs rendszer lemezeként. |
|**Tárterület kezelése**||
| [Felügyelt lemezes ugyanazon vagy másik előfizetésben található virtuális merevlemez létrehozása](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Egy felügyelt lemezt hoz létre, egy speciális olyan operációs rendszert tartalmazó virtuális Merevlemezt vagy egy adatlemez ugyanazon vagy másik előfizetésben található virtuális Merevlemezt a.  |
| [Hozzon létre egy felügyelt lemezes egy pillanatképből](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Felügyelt lemezes pillanatképet hoz létre. |
| [Másolja a felügyelt lemezes toosame vagy másik előfizetésben található](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Felügyelt másolatok toosame vagy másik előfizetést, de a hello azonos régióban hello szülőként kezelt lemezre. 
| [Pillanatkép virtuális merevlemez tooa tárfiókkal exportálása](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Virtuális merevlemez tooa tárfiók más régióban található, exportálja a felügyelt pillanatképet. |
| [Pillanatkép virtuális merevlemez létrehozása](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hoz pillanatkép egy virtuális merevlemez toocreate több azonos felügyelt lemezek pillanatképből rövid időn belül.  |
| [Másolja a pillanatkép toosame vagy másik előfizetésben található](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Másolatot pillanatkép toosame vagy másik előfizetésben található de hello azonos régióban hello szülő pillanatképként. |
|**Virtuális gépek védelme**||
| [A Virtuálisgép- és adatlemezek titkosítása](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Létrehoz egy Azure Key Vault, a titkosítási kulcs és a szolgáltatás egyszerű, majd a virtuális gépek titkosítja. |
|**Virtuális gépek figyelése**||
| [Virtuális gép és az Operations Management Suite figyelése](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet, hello Operations Management Suite ügynököt telepíti és regisztrálja az OMS-munkaterület VM hello.  |
| | |


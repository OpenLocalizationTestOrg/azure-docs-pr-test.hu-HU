---
title: "parancssori felület minták aaaAzure |} Microsoft Docs"
description: "Az Azure CLI-minták"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 776c947e6daca564242fc77b0527dcb124fa057d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>A Linux virtuális gépek Azure CLI-minták

hello alábbi táblázat tartalmaz hivatkozásokat toobash parancsfájlokat hello Azure parancssori felület használatával készített.

| | |
|---|---|
|**Virtuális gépek létrehozása**||
| [Virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | A Linux virtuális gép minimális konfigurációs hoz létre. |
| [A teljesen konfigurált virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások.|
| [Magas rendelkezésre állású virtuális gépek létrehozása](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Több magas rendelkezésre állású virtuális gépek és az elosztott terhelésű konfigurációs hoz létre. |
| [A Docker engedélyezve van a virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és konfigurálja a virtuális gép egy Docker-gazdagépként egy NGINX tároló futtatja. |
| [Hozzon létre egy virtuális Gépet, és futtassa a konfigurációs parancsfájl](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és hello Azure Custom Script bővítmény tooinstall NGINX használja. |
| [Hozzon létre egy virtuális gép WordPress telepítése](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, és hello Azure Custom Script bővítmény tooinstall WordPress használja. |
| [A felügyelt operációsrendszer-lemez a virtuális gép létrehozása](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy meglévő kezelt lemez az operációs rendszer lemezeként csatolásával. |
| [Hozzon létre egy virtuális Gépet egy pillanatképből](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Létrehoz egy virtuális gépet egy pillanatképből, először hoz létre egy felügyelt lemezes pillanatképből, és majd lemezcsatlakoztatás hello új felügyelt az operációs rendszer lemezeként. |
|**Tárterület kezelése**||
| [Felügyelt lemezes virtuális merevlemez létrehozása](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Egy felügyelt lemezt hoz létre, egy speciális olyan operációs rendszert tartalmazó virtuális Merevlemezt vagy egy virtuális Merevlemezt adatlemez a.  |
| [Hozzon létre egy felügyelt lemezes egy pillanatképből](../scripts/virtual-machines-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Felügyelt lemezes pillanatképet hoz létre. |
| [Másolja a felügyelt lemezes toosame vagy másik előfizetésben található](../scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Felügyelt másolatok toosame vagy másik előfizetést, de a hello azonos régióban hello szülőként kezelt lemezre. 
| [Pillanatkép virtuális merevlemez tooa tárfiókkal exportálása](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Virtuális merevlemez tooa tárfiók más régióban található, exportálja a felügyelt pillanatképet. |
| [Másolja a pillanatkép toosame vagy másik előfizetésben található](../scripts/virtual-machines-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Másolatot pillanatkép toosame vagy másik előfizetésben található de hello azonos régióban hello szülő pillanatképként. |
|**Virtuális gépek hálózati**||
| [Virtuális gépek közötti hálózati forgalmának biztonságossá tétele](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Két virtuális gép minden kapcsolódó erőforrások és egy belső és külső hálózati biztonsági csoportokkal (NSG) hoz létre. |
|**Virtuális gépek védelme**||
| [A Virtuálisgép- és adatlemezek titkosítása](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy Azure Key Vault, a titkosítási kulcs és a szolgáltatás egyszerű, majd a virtuális gépek titkosítja. |
|**Virtuális gépek figyelése**||
| [Virtuális gép és az Operations Management Suite figyelése](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Létrehoz egy virtuális gépet, hello Operations Management Suite ügynököt telepíti és regisztrálja az OMS-munkaterület VM hello.  |
|**Virtuális gépek hibaelhárítása**||
| [A virtuális gépek operációs rendszer lemezhiba elhárítása](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Csatlakoztatja hello operációsrendszer-lemez egy virtuális géptől adatlemezt, a második virtuális gép. |
| | |

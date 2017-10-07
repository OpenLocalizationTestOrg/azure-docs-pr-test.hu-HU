---
title: "aaaAzure CLI parancsfájl minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 1e792fdbb7daea92bf6a6589a5d8aab5b9b5a670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a>Felügyelt lemezes tárfiókokban hello a VHD-fájl létrehozása a parancssori felület ugyanahhoz az előfizetéshez

Ezt a parancsfájlt hoz létre egy felügyelt lemezes egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez. A parancsfájl tooimport egy speciális (nem általánosított/Sysprep használatával előkészített) VHD toomanaged OS lemez toocreate egy virtuális gép használja. Másik lehetőségként azt tooimport VHD toomanaged adatlemezt. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate felügyelt lemezes olyan virtuális merevlemezről. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az lemez létrehozása](https://docs.microsoft.com/cli/azure/disk#create) | Létrehoz egy virtuális merevlemez URI használatát a tárfiókokat hello felügyelt lemezes ugyanahhoz az előfizetéshez |

## <a name="next-steps"></a>Következő lépések

[Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

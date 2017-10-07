---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása a VHD-vel |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - egy virtuális merevlemez virtuális gép létrehozása."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Virtuális gép létrehozása virtuális merevlemezzel

Ebben a példában egy virtuális gép virtuális merevlemez használatával hoz létre.
Létrehoz egy erőforráscsoport, a tárfiók és a tároló, majd létrehoz egy virtuális gép hello VHD toohello tároló feltöltésével.
Lecseréli hello ssh nyilvános kulcsát a nyilvános kulccsal, hogy hozzáférési toohello virtuális gép.

Szüksége lesz egy rendszerindító virtuális Merevlemezt.
Hello használt VHD letöltését https://azclisamples.blob.core.windows.net/vhds/sample.vhd vagy a saját virtuális merevlemez használatára. hello parancsfájl megkeresi `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az tárolási fiók listája](https://docs.microsoft.com/cli/azure/storage/account#list) | Storage-fiókok listája |
| [az ellenőrzés-tárfióknév](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Ellenőrzi, hogy a tárfiók neve érvényes-e, és, hogy még nem létezik |
| [az tárolási fióklista kulcsok](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Kulcsok hello storage-fiókok listája |
| [az a tárolási blob létezik](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Ellenőrzi, hogy létezik-e hello blob |
| [az a tároló létrehozása](https://docs.microsoft.com/cli/azure/storage/container#create) | Egy tároló létrehoz egy tárfiókot. |
| [az tárolási blob feltöltése](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Blob feltöltése hello virtuális merevlemez által hoz hello a tárolóban. |
| [az vm listája](https://docs.microsoft.com/cli/azure/vm#list) | A használt `--query` ellenőrizze, hogy hello nevet a virtuális gép használja. | 
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Létrehozza a hello virtuális gépeket. |
| [az vm access-linux-felhasználói fiók beállítása](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Alaphelyzetbe állítja a hello SSH kulcs toogive hello aktuális felhasználó hozzáférési toohello virtuális gép. |
| [az vm-ip-címeinek listáját](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Lekérdezi a létrehozott virtuális gép hello hello IP-címét. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
